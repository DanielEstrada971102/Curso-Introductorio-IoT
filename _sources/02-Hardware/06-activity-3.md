# Actividad 3
**Adaptación del sistema de monitoreo de temperatura y humedad del ambiente con sensor DHT22**

**Objetivo:** Poner en practica los conceptos de interrupciones y comunicación serial en el sistema de monitoreo desarrollado en la [Actividad 1][1]. 

**Descripción:** En esta actividad, se realizará una adaptación del sistema de monitoreo de temperatura y humedad del ambiente utilizando el sensor DHT22. Se partirá del código de Arduino desarrollado en la actividad mencionada y se realizarán las modificaciones necesarias. El sistema debe obtener datos precisos de temperatura y humedad del ambiente utilizando el sensor DHT22 e implementará una tasa de muestreo fija para tomar las mediciones en intervalos regulares. Además, se debe añadir la funcionalidad de enviar los datos recopilados por el puerto serial únicamente cuando se reciba un comando específico. 

También, además del LED que se enciendecuando la temperatura supere un umbral predefinido, se debe agregar un LED adicional que parpadeará constantemente, simulando un indicador de funcionamiento del sistema.

Para llevar a cabo esta adaptación, se realizarán modificaciones en el código de Arduino, incorporando interrupciones para manejar la toma de datos el estado del LED.

A continuación se presenta la solución

```{code-block} arduino
#include <DHT.h>

// Declaración de pines
#define DHTPIN 2         // Pin digital para el sensor DHT22
#define IND_LEDPIN 3     // Pin digital para el LED indicador
#define OT_LEDPIN 4      // Pin digital para el LED de sobre temperatura

// Constante para el tipo de sensor DHT
#define DHTTYPE DHT22         // Tipo de sensor DHT22

// Umbral de temperatura para encender el LED
const float TEMPERATURE_THRESHOLD = 25.0;    // Umbral de temperatura en grados Celsius

// Banderas y contadores para las interrupciones
volatile int measureCounter = 0;
volatile int ledCounter = 0;
volatile int measureFlag = 0;
volatile int LedFlag = 0;

// Bandera para el envio de datos
int sendFlag = 0;

// Variables a medir
float temperature = 0;
float humidity = 0;

// Inicialización del sensor DHT
DHT dht(DHTPIN, DHTTYPE);

void setup() {
    // Iniciar comunicación serial
    Serial.begin(115200);

    // Configurar el pin del LED como salida
    pinMode(OT_LEDPIN, OUTPUT);
    pinMode(IND_LEDPIN, OUTPUT);

    // Iniciar el sensor DHT
    dht.begin();
    init_Timer();
    Serial.println("Sistema inciado");
}

void loop() {

    if (measureFlag == 1){
        measureFlag = 0;
        measure();
    }

    if (sendFlag == 1){
        sendFlag = 0;
        send_measurement();
    }

    if (LedFlag == 1){
        LedFlag = 0;
        digitalWrite(IND_LEDPIN, !digitalRead(IND_LEDPIN));
    }

    // Encender el LED si la temperatura supera el umbral
    if (temperature > TEMPERATURE_THRESHOLD) {
        digitalWrite(OT_LEDPIN, HIGH);    // Encender el LED
    } else {
        digitalWrite(OT_LEDPIN, LOW);     // Apagar el LED
    }

}


void init_Timer(){
    /* Con esta configuración se creara una interrupción cada 1 ms usando el Timer1
        y un preescalar de 1024.
        Calculos:
        clock --> 16 Mhz
        Prescalar --> 1024;
        velocidad de Timer 1= 16Mhz/1024 = 15625 Hz
        Pulso = 1/15625 hz =    64us
        Contador = 1 ms / 64us = 15625
    */
    cli();                // Se deshabilitan las interrupciones 
    TCNT1 = 0;	          // Se reinicia el contador del Timer 1.
    TCCR1A = 0;           // Se reinicia todo el registro TCCR1A
    TCCR1B = 0;           // Se reinicia todo el registro TCCR1A
    TCCR1A |= B00000101;	// Se pone CS12 y CS10 en 1, es decir un preescalar de 1024
    TIMSK1 |= B00000010;	// Se habilita la interrupción por compare mathc
    OCR1A = 15625;        // Se define el registo de comparacion A en este valor
    sei();                // Se habilitan las interrupciones
}

ISR(TIMER1_COMPA_vect){
    measureCounter++;

    // Frecuencia de muestreo de 200 ms
    if (measureCounter == 200){
        measureCounter = 0;
        measureFlag = 1;
    }

    // Frecuencia del Led indicador de 0.5s
    ledCounter++;
    if (ledCounter == 500)
    {
        ledCounter = 0;
        LedFlag = 1;
    }
}


void serialEvent(){
    // Si llega algo por el puerto serial se lee y se verifica si corresponde al comando ENVIAR
    String cmd = Serial.readStringUntil("\r\n");
    if (cmd == "ENVIAR") sendFlag = 1;
}


void measure(){
    // Leer la temperatura y la humedad del sensor DHT
    temperature = dht.readTemperature();
    humidity = dht.readHumidity();
}


void send_measurement(){
    // Envia la temperatura y la humedad por serial
    Serial.print("Temperatura (°C): " + String(temperature));
    Serial.print(",");
    Serial.println("Humedad (%): " + String(humidity));
}
```
```{admonition} Opcional
Puede tratar de implementar la escritura de los pines por medio de los registros. 
```

[1]: 03-activity-1.md
