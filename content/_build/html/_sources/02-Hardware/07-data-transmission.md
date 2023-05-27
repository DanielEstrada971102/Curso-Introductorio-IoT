# Transmisión de datos

La transmisión de datos desde los sistemas electrónicos de adquisición, como Arduino, Raspberry Pi y ESP32, hacia la infraestructura subyacente para su posterior procesamiento, almacenamiento o visualización. Existen varias opciones para realizar la transmisión de información, y la elección dependerá de diversos factores como el tipo de proyecto, los requisitos de comunicación, la distancia de transmisión y la disponibilidad de recursos.

Una de las formas más comunes de transmitir datos es a través de protocolos de comunicación serial ya mencionados, como UART (Universal Asynchronous Receiver-Transmitter) o SPI (Serial Peripheral Interface). Estos, permiten la comunicación punto a punto entre dispositivos y son ampliamente utilizados en aplicaciones de IoT y sistemas embebidos. Otra opción popular es la transmisión de datos a través de protocolos de comunicación inalámbrica, como Wi-Fi o Bluetooth. Raspberry Pi y ESP32 son dispositivos que ofrecen conectividad Wi-Fi y Bluetooth integrada, lo que permite la transmisión de datos de forma inalámbrica hacia otros dispositivos o hacia la nube. Estos protocolos son especialmente útiles cuando se requiere una comunicación remota o cuando la infraestructura subyacente se encuentra a cierta distancia física de los sistemas de adquisición. En caso de que las condiciones del proyecto demanden establecer una comunicación de mayor distancia, protocolos más robustos deben ser utilizado como LoraWan, Zigbe, u otros que involucren comunicación por radio o redes celulares. 

En el campo de la comunicación WEB se deben adoptar ciertos tecnologías o protocolos de red para la transmisión de datos, como lo es el conocido MQTT (Message Queuing Telemetry Transport), los tipicos Web server o HTTP (Hypertext Transfer Protocol), o WebSockets. La elección, de nuevo, dependerá de las necesidades específicas del proyecto, incluyendo la seguridad, el rendimiento, la eficiencia energética y la compatibilidad con la infraestructura existente.

Como se habrá notado, algunas simulaciones utilizadas en la [Actividad 1][1] permitieron mostrar la implementación del protocolo MQTT en aplicaciones de IoT sin entrar en detalles técnicos exhaustivos. Al final de esta sección, se tratará de ampliar este tema para lograr un entendimiento básico. Otros protocolos de red como HTTP o WebSocket serán explorados más adelante, cuando se este trabajando en el backend y frontend del sistema. Por ahora, nos enfocamos en realizar la transmisión de datos desde el sistema electrónico a través de protocolos de comunicación serial directamente a una computadora (o Raspberry), donde la información podrá ser analizada y publicada en una base de datos.

## Comunicación serial con Arduino
 la comunicación serial es una forma sencilla y ampliamente utilizada de establecer la comunicación entre un Arduino (en general cualquier placa de desarrollo) y otros dispositivos. Ofrece simplicidad, disponibilidad y bidireccionalidad, lo que la hace adecuada para una variedad de aplicaciones en las que se requiere la transferencia de datos de manera secuencial y en tiempo real.

 Al ser un tipo de comunicación síncrona, se debe asegurar una adecuada sincronización entre los dispositivos, esto es, establecer la velocidades de transmisión, conocidas como baudios igual en ambos puntos. Los baudios más comunes incluyen 9600, 19200, 38400, 57600 y 115200.

Para utilizar la comunicación serial en Arduino, se utilizan las funciones `Serial.begin()` para inicializar el puerto serial y `Serial.print()` o `Serial.write()` para enviar datos. Por otro lado, se utiliza `Serial.available()` para verificar si hay datos disponibles para leer y `Serial.read()` u otras para leer los datos recibidos. Puede consultar la documentación oficial ([link][3]) para explorar los metodos disponibles. También es una buena práctica encapsular las rutinas relacionadas con las lecturas del puerto serial en una función llamada `serialEvent()`, de forma que la lógica de lectura de comandos enviados al dispositivo quede separada del bloque principal. La aplicación de esto se ve en la [Actividad 3][4]


## Protocolo MQTT
Es un protocolo de mensajería ligero diseñado para la comunicación entre dispositivos conectados a través de redes. Fue creado por IBM y se ha convertido en un estándar ampliamente utilizado en el campo de la Internet de las cosas (IoT). MQTT sigue el paradigma de **publicación-suscripción**, lo que significa que los dispositivos pueden publicar mensajes en temas (**topics**) y suscribirse a temas para recibir los mensajes publicados.

**Principios básicos de MQTT:**

* **Broker MQTT**: Se utiliza un intermediario llamado "broker" para facilitar la comunicación entre los dispositivos. El broker es responsable de recibir los mensajes publicados y reenviarlos a los dispositivos suscritos a los temas correspondientes.

* **Publicación**: Los dispositivos publican mensajes en un tema específico. Un tema puede verse como una etiqueta o categoría que identifica el contenido del mensaje. Los dispositivos pueden publicar mensajes en múltiples temas.

* **Suscripción:** Los dispositivos pueden suscribirse a uno o más temas para recibir los mensajes publicados en esos temas. Al suscribirse a un tema, el dispositivo indicará al broker qué temas le interesan y el broker reenviará los mensajes correspondientes.

En la {ref}`Fig. X <MQTT-protocol>`, se puede apreciar un esquema general que ilustra el flujo de datos en una red de comunicación con el protocolo MQTT.

```{figure} ../img/MQTT-protocol.png
---
scale: 40%
name: MQTT-protocol
---
Imagen tomada de de [link][2] 

```

MQTT presenta una serie de ventajas significativas. En primer lugar, es un protocolo ligero que utiliza un formato de mensaje compacto, lo que reduce el consumo de ancho de banda y la carga en los recursos del sistema. Esta eficiencia lo hace especialmente adecuado para dispositivos con restricciones de energía y capacidad de procesamiento.

En segundo lugar, MQTT es altamente escalable, lo que significa que puede soportar una gran cantidad de dispositivos conectados simultáneamente. Esto se logra a través de su enfoque de comunicación basado en el modelo de publicación y suscripción. Permite una arquitectura flexible y distribuida, donde los dispositivos se pueden unir y dejar la red de manera dinámica.

En tercer lugar, MQTT proporciona niveles de QoS (Quality of Service) que garantizan la entrega confiable de los mensajes. Esto es especialmente importante en entornos donde la pérdida de datos no es aceptable, como en aplicaciones críticas o sistemas de monitoreo. Los niveles de QoS permiten controlar el flujo de mensajes y asegurar que lleguen en el orden correcto y sin pérdidas.

Otra ventaja de MQTT es su baja latencia. Los mensajes se entregan rápidamente entre los dispositivos conectados, lo que permite una comunicación en tiempo real y una respuesta rápida a eventos o cambios en el sistema.

Por último, MQTT es altamente compatible y se puede implementar en una amplia gama de dispositivos y plataformas. Esto facilita su integración en diferentes entornos y sistemas existentes, lo que lo convierte en una opción flexible y versátil para la comunicación en el Internet de las Cosas y otras aplicaciones de red.

[1]: 03-activity-1.md
[2]: <https://www.twilio.com/blog/what-is-mqtt>
[3]: <https://www.arduino.cc/reference/en/language/functions/communication/serial/>
[4]: 06-activity-3.md
