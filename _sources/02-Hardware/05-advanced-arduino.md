# Arduino - características avanzadas 
```{contents}
```
Para el desarrollo de dispositivos IoT, suele ser necesario ahondar un poco más en las características avanzadas, o de **bajo nivel**, de las tarjetas de desarrollo como Arduino. Esto, permite un mayor control y optimización de los dispositivos conectados. La habilidad de controlar directamente registros, Timers e interrupciones es especialmente importante, ya que permite aprovechar al máximo el potencial de los dispositivos. El control directo de registros, por ejemplo, permite una comunicación más eficiente y precisa con los sensores y actuadores utilizados. Por su parte, los Timers y las interrupciones son esenciales para programar tareas y eventos en función del tiempo, lo que resulta especialmente útil en aplicaciones en tiempo real. Por lo tanto, está sección ilustrara algunas concepciones básicas al respecto para que tengas más herramientas a la hora de diseñar sus dispositivos IoT. [^1]

## Control de los pines con registros
Aunque Arduino ofrece funciones sencillas para controlar los pines y otras características, en algunas ocasiones es necesario tener un mayor control y precisión sobre las operaciones que se realizan. En estos casos, la manipulación directa de los registros es una técnica muy útil. Al acceder directamente a los registros de entrada y salida (I/O) del microcontrolador, se pueden manipular los estados de los pines a nivel de bits y lograr tiempos de ejecución más rápidos, evitando operaciones innecesarias al invocar funciones preestablecidas. Esto es especialmente útil en aplicaciones que requieren ciclos de procesamiento rápidos, como la generación de señales de alta frecuencia, el control de actuadores o la comunicación con periféricos complejos.

Para visualizar esta problemática, imagine que se quiere generar un pulso a la máxima velocidad que permita el microcontrolador. En un principio, esto implica simplemente evitar el uso de `delay()` en el control del tamaño del pulso. Entonces, el código queda tan simple como:

```{code-block} arduino
void setup(){
    pinMode(9, OUTPUT); // Configurar el pin 9 como salida
}
void loop(){
    digitalWrite(9, HIGH); // Poner el estado del pin en HIGH
    digitalWrite(9, LOW);  // Poner el estado del pin en LOW
    delay(5);              // Solo para separar cada pulso
}
```
Lo que se esperaría de este programa es que genere un pulso con un ancho en el orden de los ~70 ns, que corresponde a una frecuencia de procesamiento de 16 MHz. Sin embargo, una inspección rápida de la señal con un osciloscopio permite evidenciar un pulso de aproximadamente 4 us (ver {ref}`Fig.15 <signalcomparation>`). Esto ocurre porque, como no debería ser sorpresa para usted, en la ejecución de la función `digitalWrite` se ejecutan más instrucciones internamente, lo que representa un gasto en tiempo de cómputo. Manipular directamente los registros de entrada y salida, en lugar de las funciones de manipulación de pines, ataca esta problemática. Para ello, es necesario entender qué registros hay y cómo se manipulan.

```{figure} ../img/singalcomparation.png
---
scale: 50%
name: signalcomparation
---
Izquierda: Pulso generado con digitalWrite; Derecha: Pulso generado con los registros.
```

Dependiendo de la placa de desarrollo que se esté usando, se tendrá un número diferente de puertos (pines). En el caso del Arduino Mega2560, se tienen un total de 86 pines disponibles para entrada/salida. Estos pines están distribuidos en diferentes puertos del microcontrolador ATmega2560, que se agrupan de a 8 pines (Puerto A, B, ..., L, ver el [diagrama de pines][4] de la placa y el [datasheet][1] del microcontrolador). Cada uno de estos puertos se asocia a un conjunto de registros específicos en el microcontrolador y se utilizan para configurar y controlar los pines correspondientes. Por ejemplo, para el Puerto# (# puede ser cualquiera de las letras que enumeran los puertos), los registros asociados son DDR# (registro de dirección de datos), que controla si el pin se usa como entrada o salida; PORT# (registro de salida), que permite cambiar el estado del pin; y PIN# (registro de entrada), que almacena el estado actual de cada pin, por lo tanto, leer este pin equivale a lo que haría digitalRead(). Veamos esto de forma más clara con un ejemplo:

### Ejemplo - Blinking-LED
Realizaremos el clásico ejemplo de hacer titilar el LED del pin 13 integrado en la placa, pero usando los registros. Hay que tener en cuenta que el pin 13 se encuentra en el puerto B del microcontrolador en el bit 7 (MSB), ver ({ref}`Fig. 16 <PinRegControl>`). 

```{figure} ../img/pinRegControl.png
---
scale: 60%
name: pinRegControl
---
Ilustración de la manipulación de registros para el pin 13.
```

El código sería simplemente:

```{code-block} arduino
void setup() {  
  DDRB |= B10000000;  // Se configura el pin 13 como OUTPUT
}

void loop() { 
  PORTB |= B10000000;         // Se pone el estado del pin 13 en HIGH
  delay(1000);
  PORTB &= !B10000000;        //Se coloca el estado del pin 13 en LOW (Se usa ! para invertir el byte)
  delay(1000);
}
```
```{note}
Note que la manipulación de los bits en los registros va de la mano con las operaciones booleanas. Esto se realiza para evitar afectar el estado de los demás bits al hacer asignaciones en un bit específico.
```

## Interrupciones
Las interrupciones, básicamente, se tratan de señales que ocasiona una **suspensión temporal de la ejecución de un proceso**. Durante dicha suspensión, se ejecuta una subrutina de servicio de interrupción (ISR - Interruption Service Rutine) predefinida. Las interrupciones son una poderosa herramienta que permiten a los desarrolladores responder de manera rápida a eventos externos mientras se está ejecutando el programa principal. Con Arduino se dispone de dos tipos de interrupciones: las programadas o de *Timer* y las externas o de hardware. 

### Externas
Las interrupciones externas se activan cuando se detecta un cambio en el estado de un pin específico (depende de cada placa), ya sea un flanco ascendente (de bajo a alto) o descendente (de alto a bajo). Estas interrupciones son útiles cuando se necesita detectar eventos rápidos, como pulsaciones de botones o señales directas de un sensor.
Para usar las interrupciones, la plataforma de Arduino nos provee de funciones como: 
* `attachInterrupt(#interruptPin, ISR, mode)` : configura el pin especifico[^2] como puerto de interrupción. La ISR es la fusión que se quiere asociar a dicha interrupción. Y el modo puede escogerse entre “LOW”, “CHANGE”, “RISING” y “FALLING”. 
* `digitalPinToInterrupt(#pin)`: esta función traduce el número del pin de la placa en su respectivo número de interrupción. Por ejemplo, el pin numero de 2 de la tarjeta tiene asignada la interrupción numero 1 y el pin numero 19 la interrupción número 4. Usar este mapeo es indispensable a la hora de invocar las demás funciones y dependerá de cada placa. 
* `dattachInterrupt(#interruptPin)`: como su nombre lo indica, este método anula la configuración que se haya hecho.

Para ilustrar, Suponga que tiene un proyecto en el que al detectar pulsaciones de un botón se realice una acción. Se puede entonces utilizar una interrupción externa para detectar el cambio de estado del pin conectado al botón.

``` {code-block} arduino
const int buttonPin = 2;  // Pin digital utilizado para el botón
volatile bool buttonPressed = false;  // Variable volátil que indica si el botón ha sido presionado

void setup() {
  Serial.begin(115200);
  pinMode(buttonPin, INPUT_PULLUP);  // Configuramos el pin del botón como entrada con resistencia de pull-up
  attachInterrupt(digitalPinToInterrupt(buttonPin), buttonInterrupt, CHANGE);  // Asignamos la interrupción al pin del botón
}

void loop() {
  if (buttonPressed) {
    // Realizamos la acción deseada cuando se presiona el botón
    // Puede ser, por ejemplo, encender un LED o enviar un mensaje por serial
    Serial.println("¡Boton presionado!");
    
    buttonPressed = false;  // Reiniciamos la variable para detectar nuevas pulsaciones
  }
  // Resto del código del programa principal
}

void buttonInterrupt() {
  // Esta función se ejecutará cuando se active la interrupción del botón
  buttonPressed = true;  // Indicamos que el botón ha sido presionado
}
```

```{warning}
* Note que la variable `buttonPressed` se declaro como `volatile`, esto es necesario cuando se utiliza una variable compartida entre el código principal y una ISR. Esto le indica al compilador que la variable puede cambiar de manera impredecible y que su valor puede ser alterado por eventos externos, como una interrupción. De esta forma se evita que el compilador realice optimizaciones de caché o registros que podrían introducir comportamientos inesperados en la lógica de programación.

* Es recomendable que las ISR sean **cortas y eficientes**, realizando solo las acciones necesarias para atender la interrupción específica. Por lo general, esto implica realizar tareas esenciales, como leer o escribir registros, cambiar estados o realizar cálculos rápidos. Esto porque se ejecutan de forma asincrónica, interrumpiendo el flujo normal del programa principal, y si es demasiado larga y lleva mucho tiempo en ejecutarse, puede causar retrasos significativos en el programa principal, lo que afectaría la capacidad de respuesta del sistema en general.
``` 

### Programadas o de Timer
Los microncontroladores de arduino cuantan con una serie de temporizadores (Timers) internos basados en el cristal oscilador de 16 MHz (o un oscilador interno de 8 MHz dependiendo del bootloader usado). Estos pueden ser configurados para generar una señal de interrupción que se envía cuando se alcanza un valor determinado. Lo cual se puede utilizar para ejecutar código en intervalos de tiempo específicos, incluso si Arduino está ocupado realizando otras tareas. Para utilizar las interrupciones de Timer es necesario configurar y activar el temporizador específico del microcontrolador. El proceso generalmente implica **Configurar el Timer** estableciendo parámetros como la frecuencia de temporización y el modo de funcionamiento, esto se hace mediante la configuración de **registros** específicos del microcontrolador. Una vez configurado se crea una función ISR que se asocie con el Timer configurado.

Se prodría pensar en fijar una interrupción cada 1/16000000 = 62.5 ns, sin embargo, este limite está restringido debido a que cada instrucción de Arduino requiere múltiples pulsos de reloj para ejecutarse (incluyendo al menos 5 pulsos para las interrupciones). La forma real en la cual se establecen las interrupciones es en base a un componente intermedio llamado **preescalar** que actúa como un divisor de frecuencia. El preescalar divide el número de pulsos de reloj en factores específicos (como 8, 64, 256, etc.), lo que resulta en una señal de reloj que es varias veces más lenta. El temporizador realiza su conteo basándose en estos pulsos de reloj del preescalar, lo que permite obtener señales de interrupción a frecuencias personalizadas al configurar adecuadamente el preescalar. 

```{admonition} Ejemplo - Preescalar
Para ilustrar el concepto del preescalar en la configuración del Timer, suponga que este se define en un valor de 8. Quiere decir que se tendrá un pulso de reloj 8 veces más lento que el del cristal oscilador, eso significa pasar de un reloj de 62.5 ns a uno de 500 ns. De igual forma, si se escoje un preescalar de 256 se pasaría a tener un pulso de reloj de periodo de 1.6 $\mu$s.
```

Con base en el pulso del preescalar, el Timer incrementará su contador hasta alcanzar un valor máximo para luego reiniciar la cuenta. El valor máximo depende del Timer utilizado. En el caso del Arduino Mega2560, se cuenta con 2 Timers de 8 bits y 4 de 16 bits, es decir, 2 Timers que pueden contar hasta 255 y 4 Timers que cuentan hasta 1024. Dependiendo del periodo de interrupción que se desee configurar, se debe utilizar uno u otro.

Existen varios modos de generación de interrupciones, siendo los más comunes el modo "*compare match*" y el modo "*overflow*". En el modo "*compare match*", la interrupción se genera cuando el contador alcanza un valor predefinido en un registro. En el segundo modo, las interrupciones ocurren cada vez que el contador se reinicia. También existe un tercer modo llamado "input capture interrupt", pero se puede consultar más información sobre este en la documentación oficial.

La configuración del modo y el preescalar se realiza a través de los registros de control del Timer (TCCR#A y TCCR#B, donde "#" se refiere al número del Timer utilizado). El valor del contador del Timer se puede acceder a través del registro TCNT#. Por último, el registro TIMSK# permite habilitar los diferentes modos del Timer. Para obtener más detalles sobre los registros del Timer, se puede consultar el [datasheet][1] del microcontrolador ATmega2560. De allí se extrajeron las siguientes capturas :

```{figure} ../img/TCCRegisters.png
---
scale: 40%
name: TCCRegisters
---
Arduino Mega 2560 TCCR1 A y B. Tomado del datasheet.
```
```{figure} ../img/TCNCRegister.png
---
scale: 40%
name: TCNCRegisters
---
Arduino Mega 2560 TCNC1 L y H para completar los 16 bits. Tomado del datasheet.
```
```{figure} ../img/ComparatorRegisters.png
---
scale: 40%
name: ComparatorRegisters
---
Arduino Mega 2560 TIMSK1, OCR1A L y H, OCR1B L y H, OCR1C L y H. Tomado del datasheet.
```

Para dar claridad a lo expuesto, a continuación se muestra cómo se ve esto en la práctica (Usando el Timer0 que es de 8-bits).

#### Ejemplo 1: Interrupción de Timer en modo overflow

```{code-block} arduino
/**
 * Este código tiene como objetivo controlar el estado de un LED en intervalos regulares utilizando interrupciones de timer en Arduino.

 * Configuración del Timer:
 * - Se utiliza el Timer 1 (de 16 bits).
 * - Se establece un preescalar de 1024 (CS12 y CS10 en 1) para el Timer 1.
 * - Se habilita la interrupción por overflow del Timer 1.
 
 * Con esta información:
 * --> Velocidad del Timer 1: 16Mhz/1024 = 15625 Hz
 * --> Tiempo de pulso: 64 us
 * --> Tiempo de interrupción: 64 us * 2^16 ~ 4.194 s  
 */
bool CHANGE_LED = false; // Bandera para cambiar el estado de un LED;
int LED_STATE = 0;       // Estado del LED

void setup() {  
  pinMode(13, OUTPUT);
  cli();                // Se deshabilitan las interrupciones 
  TCNT1 = 0;            // Se reinicia el contador del Timer 1.
  TCCR1A = 0;           // Se reinicia todo el registro TCCR1A
  TCCR1B = 0;           // Se reinicia todo el registro TCCR1A
  TCCR1B |= B00000101;  // Se pone CS12 y CS10 en 1, es decir un preescalar de 1024
  TIMSK1 |= B00000001;  // Se habilita la interrupción por overflow
  sei();                // Se habilitan las interrupciones 
}

void loop() { 
  if (CHANGE_LED){
    LED_STATE = !LED_STATE;
    digitalWrite(13, LED_STATE);
    CHANGE_LED = false; 
  }
}

// Con la configuracion hecha, esta ISR será ejecutada cada 4.194 s.
ISR(TIMER1_OVF_vect){
  CHANGE_LED = true;
}
```
```{note}
La asignación de la ISR se hace a través del vector de interrupción. Para cada modo y comparador hay un vector. En el ejemplo anterior se utiliza el vector `TIMER1_OVF_vect` que corresponde al modo de *overflow*. Si se ustuviera usando el modo *compare match* habría que usar `TIMER1_COMP#_vect` donde # corresponde a si se está usando el comparador A, B, o C, ver registros de TIMSK1 en ({ref}`Fig. 18 <ComparatorRegisters>`).
```
#### Ejemplo 2: Interrupción de Timer en modo compare match
Si se quiere mayor control sobre el tiempo se usa el modo de *compare match*.

```{code-block} arduino
/**
 * Este código tiene como objetivo controlar el estado de un LED en intervalos regulares utilizando interrupciones de timer en Arduino.

 * Configuración del Timer:
 * - Se utiliza el Timer 1 (de 16 bits).
 * - Se establece un preescalar de 1024 (CS12 y CS10 en 1) para el Timer 1.
 * - Se habilita la interrupción por compare match del Timer 1.
 
 * Con esta información si se quiere configurar una interrupción de 500 ms:
 * --> Velocidad del Timer 1: 16Mhz/1024 = 15625 Hz
 * --> Tiempo de pulso: 64 us
 * --> El contador debe llegar a: 500 ms / 64 us = 7813 (Este es el valor a colocar en uno de los registros OCR1#)
 */
bool CHANGE_LED = false; // Bandera para cambiar el estado de un LED;
int LED_STATE = 0;       // Estado del LED

void setup() {  
  pinMode(13, OUTPUT);
  cli();                // Se deshabilitan las interrupciones 
  TCNT1 = 0;          // Se reinicia el contador del Timer 1.
  TCCR1A = 0;           // Se reinicia todo el registro TCCR1A
  TCCR1B = 0;           // Se reinicia todo el registro TCCR1A
  TCCR1B |= (1 << WGM12);  // Se establece el modo CTC (clear timer on compare match)
  TCCR1B |= B00000101;  // Se pone CS12 y CS10 en 1, es decir un preescalar de 1024
  TIMSK1 |= B00000010;  // Se habilita la interrupción por compare mathc
  OCR1A = 7813;         // Se define el registo de comparacion A en este valor
  sei();                // Se habilitan las interrupciones 
}

void loop() { 
  if (CHANGE_LED){
    LED_STATE = !LED_STATE;
    digitalWrite(13, LED_STATE);
    CHANGE_LED = false;
  }
}

// Con la configuracion hecha, esta ISR será ejecutada cada 32.8us aproximadamete.
ISR(TIMER1_COMPA_vect){
  CHANGE_LED = true;
}
```

#### Ejemplo 3: Libreria TimerOne
Si la aplicación no amerita complicarse demasiado con la manipulación directa de los registros, se pueden recurrir a librerias que ayude a controlar las interrupciones por Timer. Una opción es [TimerOne][2]. Con unos cuantos metodos permite habilitar una interrupción por Timer con el periodo deseado (min 1 us - max. 8.3 s). 

```{code-block} arduino
#include "TimerOne.h" 
// Una instancia de la clase TimerOne es creada automaticamente
// que en adelante es accesible a traves del nombre Timer1.

bool CHANGE_LED = false; // Bandera para cambiar el estado de un LED;
int LED_STATE = 0;       // Estado del LED

void setup() {  
  pinMode(13, OUTPUT);
  Timer1.initialize(1000000); // Se inicialize el Timer
  Timer1.attachInterrupt(callback); // Se le asigna la ISR
}

void loop() { 
  if (CHANGE_LED){
    LED_STATE = !LED_STATE;
    digitalWrite(13, LED_STATE);
    CHANGE_LED = false;
  }
}

void callback(){
  CHANGE_LED = true;
}
```
```{warning}
Esta libreria no es compatible con todas la tarjetas, más detalles pueden consultarse directamente en el [repositorio][3].
```
```{note} 

Los dispositivos IoT suelen requerir el control y monitoreo de varias cosas simultáneamente, lo cual implica diferentes tiempos de muestreo en general. Siguiendo lo mencionado anteriormente, podría parecer sencillo implementar múltiples temporizadores para cada tarea según sea necesario. Sin embargo, una práctica común es utilizar un número reducido de temporizadores y aprovechar sus interrupciones para incrementar el valor de una variable en el código. Cuando esta variable alcanza diferentes umbrales, se activan diferentes banderas. Estas banderas se pueden utilizar en el bloque principal junto con condiciones simples para realizar lecturas y controles de los diversos sensores y actuadores. Una ilustración más clara de este enfoque se presenta en la [Actividad 3][6].
```
[^1]: Mucho del material presentado en esta sección se basa en el curso de "[Arduino 101][5]" del canal de YouTube "Electronoobs" .
[^2]: Para el Arduino Mega2560, las interrupciones se externas se pueden hacer a través de los pines 2, 3, 18, 19, 20, 21.
 

[1]: <https://pdf1.alldatasheet.com/datasheet-pdf/view/107092/ATMEL/ATMEGA2560.html>
[2]: <https://www.arduino.cc/reference/en/libraries/timerone/>
[3]: <https://github.com/PaulStoffregen/TimerOne>
[4]: <https://docs.arduino.cc/static/2de2c8ff00fc05065634e3823a9266c4/A000067-pinout.png>
[5]: <https://www.youtube.com/watch?v=7mkMIz5Ofho&list=PLb_2Ipj07tdG5WSrmYRyfVM8M3fED2ur-&pp=iAQB>
[6]: 06-activity-3.md