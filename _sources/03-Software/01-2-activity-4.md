# Actividad 4

**Recopilación y Publicación de datos a una base de datos de MongoDB con Python**

**Objetivo**: Aprender a utilizar la biblioteca `PyMongo`, `PySerial` y `Request` para establecer una conexión con una base de datos MongoDB y publicar los datos obtenidos del arduino a tráves de comunicación serial.

**Descripción:** En esta actividad, se realizará la integración del sistema de monitoreo de temperatura y humedad desarrollado en la [Actividad 3][1] con una base de datos para almecenar la información obtenida por Arduino. El objetivo es enviar los datos recopilados por el puerto serial a la base de datos para su almacenamiento y futura consulta. Para llevar a cabo esta integración, se utilizará la biblioteca `PyMongo`, `PySerial`, y `Request` que proporcionan interfaces de Python para trabajar con MongoDB,  con los puertos de comunicación serial, y con peticiones HTTP. Se debe tener en cuenta que para la lectura de los datos que envía el arduino hay que realiazar la decodificación de los datos para formar un mensaje en formato `.json` el cual es necesario para la publicación.

A continuación se presenta un par de soluciones.

## Usando el  gestor de la nubeMongoDB Atlas
El siguiete código lee los datos del arduino cada 2s, los códifica adecuadamente y los publica en la base de datos que se creo en el servidor MongoDB Atlas (Se debe crear antes, en caso de no haberlo hecho, sería el primer pasó a seguir).

```{code-block} python
import serial
import time
from pymongo.mongo_client import MongoClient
from pymongo.server_api import ServerApi

# Sting de conexión, se obtiene directamente de Atlas, cuando se consultan las formas de conexión.
# CAMBIARLO SEGUN EL SERVIDOR QUE HAYAN CREADO
uri = "mongodb+srv://DanielEstrada:daniel1234@iot-example-database.kp5zpry.mongodb.net/?retryWrites=true&w=majority"

# Se crea un nuevo cliente y se conecta al servidor
client = MongoClient(uri, server_api=ServerApi('1'))

# Se envía un "ping" para confirmar que se conector satisfacoriamente 
try:
    client.admin.command('ping')
    print("La conexión a MongoDB fue exitosa!")
except Exception as e:
    print(e)

# Seleccionar la base de datos y la colección adecuada
db = client['iot-example-database'] # CAMBIAR ACORDE A LA BASE DE DATOS CREADA
collection = db['temp-humidity'] # CAMBIAR ACORDE A LA COLECCIÓN CREADA

def publish_data(json_data):
    # Insertar el documento en la colección de la base de datos
    collection.insert_one(json_data)
    print('Datos publicados en la base de datos MongoDB!')


def extract_info(data):
    # Se construye un mensaje en formato json (diccionario) para poder publicar el dato
    temp, humidity = data.split(",")
    temp = float(temp.split(":")[1])
    humidity = float(humidity.split(":")[1])

    return {"temperature": temp, "humidity": humidity}

def main():
    # Seleccionar el puerto COM del arduino
    SerialPort = "COM10"

    try:
        dev = serial.Serial(SerialPort, 115200, timeout=1)
        dev.close() # Se cierra el puerto en caso de que este abierto
        dev.open() # Se abre el puerto
        print("Conectado al puerto serial: " + SerialPort)
        
        dev.flushInput() # Se limpia el bufer de entrada
        dev.flushOutput() # Se limpia el bufer de salida

        while (1):
            dev.write(str.encode("ENVIAR")) # Se envia la instruction al Arduino para enviar las mediciones
            
            # Se lee la respuesta del arduiono
            data = dev.readline().decode("utf-8").strip() # strip remueve los espacios y el final de linea

            # Si no es un dato vacio... 
            if data != "":
                data = extract_info(data)
                print(data)
                publish_data(data)

            time.sleep(2)

    except Exception as error:
        print(error)

    except KeyboardInterrupt:
        dev.close()

    finally:
        dev.close()


if __name__ == '__main__':
    main()
```
Si todo sale bien, usted podrá ver como sus datos son publicados en la base de datos creada en el servidor Atlas.

```{figure} ../img/AtlasdataView.png
---
scale: 80%
name: AtlasdataView
---
Captura de cloud.mongodb.com
```

```{warning}
En la practica, la manipulación directa de la base de datos no se recomienta, esto, para tratar de preservar la integridad de la base de datos y la gestión de la información. La interacción con la base de datos se suele automatizar a través de las APIs y queda oculta a la manipulación directa. Esto es lo que se hará posteriormente en el curso. Por ahora, a continuación se muestra como utilizar el protocolo HTTP para enviar los datos a un servidor que implementa la metodología mencionada.
```

## Usando el gestor creado para el curso

Como podrá recordar, para este curso se ha montado una [plataforma](https://iotudeab4a1-fabioc9675.b4a.run/) que implemeta los protocolos de comunicación que se trabajaran durante el módulo. Allí podrá encontrar implementado el protocolo HTTP o Web server. El servidor que despliega esta plataforma tiene implementada la API adecuada que publica en una base de datos de de MongoDB el dato enviado. El siguiente código usa la libreri `Request` para este fin. 

```{code-block} python
import serial
import time
import requests

# dirección a la cual hacer el POST de los datos.
uri = "https://iotudeab4a1-fabioc9675.b4a.run/api/instrumentation/"


def publish_data(json_data):
    x = requests.post('https://iotudeab4a1-fabioc9675.b4a.run/api/instrumentation/',json=json_data)
    print(x)


def extract_info(data):
    # Se construye un mensaje en formato json (diccionario) para poder publicar el dato
    temp, humidity = data.split(",")
    temp = float(temp.split(":")[1])
    humidity = float(humidity.split(":")[1])

    data__temp_json = {
		"topic": "iotUdeA/<webPost>", 
		"author":"DanielE", # Poner el nombre correspondiente
		"type":"webserver", 
		"varname":"Temperature",
		"varvalue":temp,
	}
    data__humd_json = {
		"topic": "iotUdeA/<webPost>", 
		"author":"DanielE", # Poner el nombre correspondiente
		"type":"webserver", 
		"varname":"Humidity",
		"varvalue":humidity,
	}

    return data__temp_json, data__humd_json

def main():
    # Seleccionar el puerto COM del arduino
    SerialPort = "COM10"

    try:
        dev = serial.Serial(SerialPort, 115200, timeout=1)
        dev.close() # Se cierra el puerto en caso de que este abierto
        dev.open() # Se abre el puerto
        print("Conectado al puerto serial: " + SerialPort)
        
        dev.flushInput() # Se limpia el bufer de entrada
        dev.flushOutput() # Se limpia el bufer de salida

        while (1):
            dev.write(str.encode("ENVIAR")) # Se envia la instruction al Arduino para enviar las mediciones
            
            # Se lee la respuesta del arduiono
            data = dev.readline().decode("utf-8").strip() # strip remueve los espacios y el final de linea

            # Si no es un dato vacio... 
            if data != "":
                data_temp_json, data_humd_json = extract_info(data)
                print(data_temp_json)
                publish_data(data_temp_json)
                print(data_humd_json)
                publish_data(data_humd_json)
                print("Datos publicadores")

            time.sleep(2)

    except Exception as error:
        print(error)

    except KeyboardInterrupt:
        dev.close()

    finally:
        dev.close()


if __name__ == '__main__':
    main()
```
```{note}
Para las personas que asisten de forma virtual y no tienen acceso a una tarjeta de desarrollo para la lectura del puerto serial, se recomienda enfocarse en la implementación de rutinas para la publicación de datos utilizando diferentes protocolos de comunicación. 
```

[1]: ../02-Hardware/03-activity-1.md
