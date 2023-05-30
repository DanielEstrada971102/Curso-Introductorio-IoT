# Actividad 1
**Diseño de un sistema de monitoreo de temperatura y humedad del ambiente con sensor DHT22**

**Objetivo:** Poner en practica los principios básicos de programación y la interacción entre componentes electrónicos en un sistema de monitoreo utilizando alguna de las teconologías vista y el sensor DHT22 para medir y visualizar en tiempo real la temperatura y humedad del ambiente, además de encender un LED cuando la temperatura supere un umbral predefinido. 

**Descripción:** En esta actividad de programación, se utilizará el sensor DHT22 junto con Arduino para obtener datos precisos de temperatura y humedad del ambiente. El programa permitirá leer los valores del sensor y mostrarlos en el monitor serial, proporcionando información actualizada sobre la temperatura en grados Celsius y la humedad en porcentaje. Además, se implementará una funcionalidad adicional que consiste en encender un LED cuando la temperatura supere un umbral establecido, lo que servirá como una señal visual de advertencia. Los participantes aprenderán a configurar los pines, inicializar el sensor DHT22, leer los datos del sensor y controlar un LED en función de la temperatura medida. Esta actividad les permitirá adquirir habilidades básicas de programación y entender el uso práctico de los sensores en la monitorización del ambiente.

A continuación se presenta la solución para cada plataforma.

```{note}
En caso de no contar con las placas de desarrollo o sensores debido a que está conectado desde la virtualidad se sugiere el uso de una plataforma de simulación, en particular [https://wokwi.com/](https://wokwi.com/).
```
## Arduino
Para el uso del sensor DHT22 se requiere importar la libreria especifica, si no la tiene en su maquina deberá hacerlo para que su programa funcone. Recuerda conectar el sensor DHT22 correctamente y asegurarte de que los pines utilizados coincidan con los definidos en ``DHTPIN`` y ``LEDPIN``. El esquema del sistema electronico es el siguiente:

```{figure} ../_static/img/actividad-1-arduino-dht22.png
---
scale: 60%
---
Conexion del sensor y el LED a la placa Arduino Mega 2560.
```

Y el código sería:

```{code-block} arduino
#include <DHT.h>

// Declaración de pines
#define DHTPIN 2          // Pin digital para el sensor DHT22
#define LEDPIN 3          // Pin digital para el LED

// Constante para el tipo de sensor DHT
#define DHTTYPE DHT22     // Tipo de sensor DHT22

// Umbral de temperatura para encender el LED
const float TEMPERATURE_THRESHOLD = 25.0;  // Umbral de temperatura en grados Celsius

// Inicialización del sensor DHT
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  // Iniciar comunicación serial
  Serial.begin(9600);

  // Configurar el pin del LED como salida
  pinMode(LEDPIN, OUTPUT);

  // Iniciar el sensor DHT
  dht.begin();
}

void loop() {
  // Leer la temperatura y la humedad del sensor DHT
  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();

  // Mostrar la temperatura y la humedad en el monitor serial
  Serial.println(
    "{Temperatura: " 
    + String(temperature)
    + " °C, Humedad: "
    + String(humidity)
    + " %}"
  );

  // Encender el LED si la temperatura supera el umbral
  if (temperature > TEMPERATURE_THRESHOLD) {
    digitalWrite(LEDPIN, HIGH);  // Encender el LED
  } else {
    digitalWrite(LEDPIN, LOW);   // Apagar el LED
  }

  delay(2000); // Retardo de 2 segundos para evitar lecturas rápidas
}
```
## ESP32
Dado que ESP32 es compatible con el IDE de Arduino, la implementación es practicamente la misma, solo hay que tener en cuenta que se debe usar una librería diferente, en este caso, ``DHTesp.h``. Dicho esto, la conexión del sensor a la tarjeta quedaría:

```{figure} ../_static/img/actividad-1-esp32-dht22.png
---
scale: 60%
name : esp32-dht22
---
Conexion del sensor y el LED a la placa ESP32.
```
Y el código sería:

```{code-block} arduino
#include "DHTesp.h"

// Declaración de pines
#define DHTPIN 15         // Pin digital para el sensor DHT22
#define LEDPIN 13          // Pin digital para el LED

// Constante para el tipo de sensor DHT
#define DHTTYPE DHT22     // Tipo de sensor DHT22

// Umbral de temperatura para encender el LED
const float TEMPERATURE_THRESHOLD = 25.0;  // Umbral de temperatura en grados Celsius

// Inicialización del sensor DHT
DHTesp dht;

void setup() {
  // Iniciar comunicación serial
  Serial.begin(115200);

  // Configurar el pin del LED como salida
  pinMode(LEDPIN, OUTPUT);

  // Iniciar el sensor DHT
  dht.setup(DHTPIN, DHTesp::DHT22);
}

void loop() {
  // Leer la temperatura y la humedad del sensor DHT
  float temperature = dht.getTemperature();
  float humidity = dht.getHumidity();

  // Mostrar la temperatura y la humedad en el monitor serial
  Serial.println(
    "{Temperatura: " 
    + String(temperature)
    + " °C, Humedad: "
    + String(humidity)
    + " %}"
  );

  // Encender el LED si la temperatura supera el umbral
  if (temperature > TEMPERATURE_THRESHOLD) {
    digitalWrite(LEDPIN, HIGH);  // Encender el LED
  } else {
    digitalWrite(LEDPIN, LOW);   // Apagar el LED
  }

  delay(2000); // Retardo de 2 segundos para evitar lecturas rápidas
}
```
```{warning}
El hecho de optar por usar ESP32 en lugar de arduino no represena una mejor alternativa si no se explota el potencial de esta tarjeta, es decir, si no se aprovecha la conectividad a la red mediante Wi-Fi. Por lo tanto, a continuación se repite la implementación pero cambiando la transferencia de datos serial por un protocolo MQTT, del cual se detallará más adelante en el curso. 
```
### ESP32 + MQTT
Para esta implementación se deben usar un par de librerias extra para establecee una conexión WiFi y conectarse al servidor MQTT especificado. La conexión con la tarjeta es la misma que en (({ref}`Fig. 5 <esp32-dht22>`)) y el código quedaría:

```{note}
Una sección con una descrición del protocolo MQTT será añadida proximamente, por ahora, si desea información sobre esto puede referirse a [link](https://www.twilio.com/blog/what-is-mqtt)
```

```{code-block} arduino
/*
Para ver los datos:

1. Ir a https://iotudeab4a1-fabioc9675.b4a.run/
2. Entrar a la pestaña MQTT Streaming
3. El Broker para esta plaraforma es broker.emqx.io y el puerto 1883.
3. Suscribase al topico iotUdeA/Nombre. Este corresponde al valor a poner en MQTT_TOPIC
4. Una vez se comienza la publicación de los datos, la información deberá poder visualizarse
   en el grafico. 

*/

#include <WiFi.h>
#include "PubSubClient.h"
#include <DHTesp.h>

// Declaración de constantes de red
const char* WIFI_SSID = "Wokwi-GUEST";
const char* WIFI_PASSWORD = "";
const char* MQTT_BROKER = "broker.emqx.io";
const int MQTT_PORT = 1883;
const char* MQTT_TOPIC = "iotUdeA/Daniel";

// Declaración de pines
#define DHTPIN 15          // Pin digital para el sensor DHT22
#define LEDPIN 13          // Pin digital para el LED

// Constante para el tipo de sensor DHT
#define DHTTYPE DHT22     // Tipo de sensor DHT22

// Umbral de temperatura para encender el LED
const float TEMPERATURE_THRESHOLD = 25.0;  // Umbral de temperatura en grados Celsius

// Inicialización del sensor DHT
DHTesp dht;

// Declaración de objetos para WiFi y MQTT
WiFiClient wifiClient;
PubSubClient mqttClient(wifiClient);

void setup() {
  // Iniciar comunicación serial
  Serial.begin(115200);

  // Conectar a la red WiFi
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.println("Conectando a WiFi...");
  }
  Serial.println("Conexión WiFi establecida.");

  // Configurar el pin del LED como salida
  pinMode(LEDPIN, OUTPUT);

  // Iniciar el sensor DHT
  dht.setup(DHTPIN, DHTesp::DHT22);

  // Configurar el servidor MQTT y la función de callback
  mqttClient.setServer(MQTT_BROKER, MQTT_PORT);
  mqttClient.setCallback(callback);
  reconnect();
}

void loop() {
  if (!mqttClient.connected()) {
    reconnect();
  }
  mqttClient.loop();

  // Leer la temperatura y la humedad del sensor DHT
  float temperature = dht.getTemperature();
  float humidity = dht.getHumidity();

  // Mostrar la temperatura y la humedad en el monitor serial
  Serial.println(
    "{Temperatura: " 
    + String(temperature)
    + " °C, Humedad: "
    + String(humidity)
    + " %}"
  );

  // Publicar los datos en el servidor MQTT
  // Solo se publica una variable para visualizar los datos en una grafica.
  String payload = "{\"author\":\"Daniel\", \"varname\":\"Temperatura\", \"varvalue\":" + String(temperature) + "}";
  mqttClient.publish(MQTT_TOPIC, (char *)payload.c_str());

  // Encender el LED si la temperatura supera el umbral
  if (temperature > TEMPERATURE_THRESHOLD) {
    digitalWrite(LEDPIN, HIGH);  // Encender el LED
  } else {
    digitalWrite(LEDPIN, LOW);   // Apagar el LED
  }

  delay(2000); // Retardo de 2 segundos para evitar lecturas rápidas
}

void reconnect() {
  while (!mqttClient.connected()) {
    Serial.println("Conectando al servidor MQTT...");
    if (mqttClient.connect("esp32Client")) {
      Serial.println("Conexión al servidor MQTT establecida.");
    } else {
      Serial.print("Error al conectar al servidor MQTT, rc=");
      Serial.print(mqttClient.state());
      Serial.println(" Intentando nuevamente en 5 segundos...");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  // Lógica de manejo de mensajes MQTT recibidos, si es necesario
}

```

## Raspberry Pi 4 - B
PENDIENTE....

