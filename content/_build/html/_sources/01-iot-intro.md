![images](img/IoT_header.png) 
# Internet de las cosas

## ¿Qué es IoT?

```{figure} img/Fig1-notRed.png
---
scale: 70%
align: right
figclass: margin
name: Fig1
---
imagen tomada de https://www.flaticon.es/iconos-gratis/computadora 
```
Internet de las cosas (IoT - _Internet Of Thing_) hace referencia a los dispositivos físicos u objetos que se conectan e intercambian información a través de redes de comunicación como internet. En esta definición, se debe hacer una salvedad en a qué hace referencia "cosas" en el nombre, esto porque a partir de lo declarado, sería válido decir, por ejemplo, que una instalación de equipos de cómputo ordinarios conectados a través de una red local satisface las condiciones para representar un sistema IoT ({ref}`Fig. 1 <Fig1>`). Sin embargo, las "cosas" en este contexto siempre querrán hacer referencia a **objetos o dispositivos que comúnmente no están asociados con internet o algún tipo de comunicación por red**. Con esto en mente, el objetivo en este campo es equipar estas "cosas" con sensores, software y tecnología de red para conectarlos, configurarlos y controlarlos, recopilando y transmitiendo datos a través, principalmente, pero no restringido, de internet. Casi cualquier objeto puede ser susceptible de adaptarse como un dispositivo IoT, simplemente proporcionándole "inteligencia computacional" y un mecanismo para conectarse a una red. Así, objetos simples pueden potenciarse para proporcionar aplicaciones más amplias y eficientes. Alguno de los ejemplos clásicos de dispositivos IoT incluyen el [refrigerador inteligente](https://www.goodhousekeeping.com/appliances/refrigerator-reviews/g39784846/smart-refrigerators/), los [automóviles inteligentes](https://www.technologyhq.org/smart-vehicles-and-internet-of-things-what-changes-it-will-bring-to-our-lives/) y algunos dispositivos de [monitoreo médico](https://ordr.net/article/iot-healthcare-examples/).

Es importante distinguir que los dispositivos genéricos de computación y similares no se consideran dispositivos IoT debido a su propósito. Mientras que las computadoras personales se diseñan para ser tecnologías multipropósito que pueden ejecutar diversas tareas y cómputos, los dispositivos IoT se refieren a tecnologías diseñadas con un propósito específico, equipadas con capacidad de cómputo, hardware y software enfocados en el cumplimiento de una tarea particular. Un ejemplo claro es el automóvil, que a pesar de poder integrar procesadores y sensores muy poderosos, su diseño se enfoca en cumplir una tarea muy específica: la conducción del vehículo.

IoT es un campo que ha experimentado un crecimiento exponencial en las últimas décadas, gracias a varios factores. Uno de ellos es la reducción de costos y la miniaturización de las tecnologías de cómputo, lo que facilita su integración en diferentes dispositivos. Además, el aumento en las capacidades de cómputo ha permitido que procesar información que antes representaba grandes desafíos computacionales, ahora sea posible a través de servicios en la nube, como el procesamiento de texto, imágenes o lenguaje natural. Es importante destacar acorde a lo último, que la conectividad a internet es un requisito fundamental para IoT, y la creciente cobertura y accesibilidad de internet ha desempeñado un papel clave en el desarrollo y expansión de estas tecnologías.

En el contexto mundial, la implementación de las tecnologías IoT tiene un gran potencial de transformar la forma en que las personas interactúan con el mundo físico y cómo las empresas operan y toman decisiones. Esto, porque los datos que se recopilan pueden ser analizados y utilizados para crear soluciones inteligentes y automatizadas, como la optimización de procesos de producción de una empresa, el monitoreo de la salud y el bienestar, la gestión de inventarios, la seguridad del hogar, entre otros. Sin embargo, el uso de IoT también conlleva ciertas desventajas. Por un lado, existe el riesgo de potenciar el aislamiento social, ya que la automatización y la comunicación a través de dispositivos pueden reemplazar la interacción humana cara a cara. Por otro lado, muchos procesos y actividades cotidianas se vuelven dependientes de la infraestructura de red, lo que significa que una caída en la conectividad puede tener consecuencias graves. Además, la exposición constante de información personal a través de dispositivos IoT puede poner en riesgo la privacidad y seguridad del usuario, ya que pueden ser usados para el mercadeo o incluso para cometer delitos cibernéticos.

## Estructura de un sistema IoT

```{figure} img/Fig2-Iot_Components.png
---
scale: 50%
name: Fig2
---
imagen tomada de https://techvidvan.com/tutorials/architecture-of-iot
```
Un sistema IoT involucra varias etapas o procesos, que van desde la elección e implementación del hardware hasta la visualización y análisis de los datos obtenidos del sistema. En términos generales, se puede dividir el sistema en 3 etapas, cada una de las cuales representa una fase del desarrollo de un sistema IoT:

La primera capa es la de dispositivos y hardware, la cual implica el diseño, fabricación y configuración de los dispositivos físicos, como sensores y actuadores, así como la conectividad de red necesaria para enviar y recibir datos de los dispositivos.

La segunda capa es la de infraestructura y conectividad, que se enfoca en la infraestructura necesaria para admitir la comunicación y el intercambio de datos entre los dispositivos IoT y los servidores o la nube.

La tercera capa es la de aplicaciones y experiencia del usuario, la cual está relacionada con la interfaz de usuario y la experiencia que tiene el usuario final. Aquí es donde se desarrollan las aplicaciones y las interfaces que permiten a los usuarios interactuar con los dispositivos IoT y aprovechar los datos recopilados.

Es importante destacar que estas capas no son necesariamente secuenciales y pueden superponerse en ciertos casos. Además, a medida que la tecnología IoT evoluciona, surgen nuevas etapas y tecnologías. Sin embargo, esta visión proporciona una estructura básica para comprender los componentes principales de un sistema IoT.

## Teconologías para IoT

En la primera etapa del desarrollo de sistemas IoT, se utilizan tecnologías como Arduino o Raspberry Pi para crear dispositivos físicos que incluyen sensores y actuadores para recopilar datos del entorno y llevar a cabo acciones físicas en respuesta. Los datos recopilados por los dispositivos IoT se transmiten a través de protocolos de comunicación como MQTT, CoAP o HTTP, para enviarlos a la infraestructura subyacente. La conectividad de los dispositivos IoT se logra mediante tecnologías de red como Wi-Fi, Bluetooth, Zigbee o LoRaWAN, que permiten establecer conexiones y conectar los dispositivos a la infraestructura de comunicación necesaria. Es importante mencionar que algunas de estas tarjetas pueden no proporcionar conectividad a internet, sin embargo, relegan entonces sus tareas de conectividad a gateways auxiliares para resolver este problema.

En la fase de infraestructura y conectividad, se emplean diversas tecnologías para lograr una comunicación eficiente y un intercambio fluido de datos entre los dispositivos IoT y la nube. Las plataformas en la nube, como AWS IoT, Google Cloud IoT o Microsoft Azure IoT, ofrecen servicios de almacenamiento, procesamiento y análisis de datos, lo que permite gestionar de manera eficiente y escalable grandes volúmenes de información. Los gateways desempeñan un papel esencial al actuar como intermediarios que facilitan la comunicación y el enrutamiento de datos entre los dispositivos IoT y la infraestructura en la nube. En cuanto al almacenamiento y gestión de datos, se utilizan bases de datos tanto relacionales como NoSQL, destacando MongoDB como ejemplo en este ámbito. Además, existen frameworks como Flask o FastAPI, y lenguajes como JavaScript, que proporcionan un marco de desarrollo de aplicaciones web, permitiendo la creación de servicios e interfaces de programación de aplicaciones (APIs), fomentando una comunicación e interacción fluida entre los dispositivos IoT, la nube, y el usuario.

En la etapa de Aplicaciones y Experiencia del Usuario en los sistemas IoT, se emplean diversas tecnologías con el objetivo de mejorar la interacción y experiencia del usuario. Se desarrollan aplicaciones móviles y web para que los usuarios puedan interactuar con los dispositivos IoT y acceder a los datos en tiempo real. Las APIs permiten la integración de los sistemas IoT con otras aplicaciones y servicios, facilitando una comunicación y un intercambio de datos eficientes. En esta fase, se utilizan tecnologías como Java, Jupyter y PyQt. Jupyter es una plataforma interactiva que combina código, visualizaciones y texto, lo cual resulta útil para el análisis y visualización de los datos generados por los dispositivos IoT. PyQt, por su parte, es una biblioteca de enlaces para Python que facilita el desarrollo de interfaces gráficas de usuario (GUI) para aplicaciones IoT, mejorando la interacción del usuario con los dispositivos y los datos recopilados.

Adicionalmente, como la seguridad es un aspecto crítico en un sistema IoT, se deben implementar medidas para proteger los datos y dispositivos de posibles ataques. Estas medidas pueden incluir la autenticación y autorización de usuarios, el cifrado de datos, el control de acceso y la segmentación de redes. Se pueden utilizar protocolos de seguridad como TLS y SSH para cifrar las comunicaciones y proteger los datos de IoT. Sin embargo, este tema se encuentra fuera del alcance de este curso. 

De lo visto, la tecnología subyacente a conocer para embarcarse en IoT incluye la programación y adaptación de hardware, la conectividad de red, la computación en la nube, la inteligencia artificial y la analítica de datos. Por lo tanto, en adelante, encontrará material que le permitirá adquirir los conocimientos básicos para el desarrollo de sistemas IoT simples utilizando algunas de las tecnologías mencionadas, como Arduino, Raspberry Pi, MongoDB, Python y JavaScript. 

