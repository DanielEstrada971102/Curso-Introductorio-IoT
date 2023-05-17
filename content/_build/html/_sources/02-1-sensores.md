# Sensores

Un sensor es un dispositivo electrónico diseñado para detectar y responder a estímulos físicos o químicos del entorno, convirtiéndolos en señales eléctricas que pueden ser procesadas y utilizadas por sistemas electrónicos como un Arduino. Los sensores desempeñan un papel fundamental en los sistemas de Internet de las Cosas (IoT), ya que permiten recolectar información del mundo físico y enviarla a los dispositivos para su posterior análisis y toma de decisiones.

Existen varios tipos de sensores, cada uno diseñado para detectar un tipo específico de estímulo que pueden varian tanto como fenomenos físicos y sus combinaciones hay, algunos ejemplos serían:

* **Sensores Acústicos:** Se basan en al medición de ondas, espectros y velocidades de onda.
* **Sensores Eléctricos:** Se basan en al medición de corrientes, carga, potenciales, campos eléctricos, permitividad y conductividad.
* **Sensores Magnéticos:** Se basan en al medición de campos magnéticos, flujos magnéticos y permeabilidad.
* **Sensores Térmicos:** Se basan en al medición de temperatura, calor específico y conductividad térmica.
* **Sensores Mecánicos:** Se basan en al medición de posición, aceleración, fuerza, presión, estrés, deformación, masa, densidad, momento, torque, forma, orientación, rugosidad, rigidez, complianza, cristalinidad y estructural.
* **Sensores Ópticos:** Se basan en al medición de intensidad lumínica, índice de refracción, reflectividad, absorción y emisividad.

En el mercado se encuentran disponibles una amplia variedad de sensores para diferentes aplicaciones. Algunos ejemplos comunes incluyen termistores, termopares y sensores de semiconductor para la medición de temperatura; fotodiodos, fototransistores y sensores de luz ambiente para la detección de luz; sensores infrarrojos pasivos, acelerómetros y giroscopios para la detección de movimiento; sensores capacitivos o resistivos para la medición de humedad; sensores piezoeléctricos o capacitivos para la medición de presión; y sensores electroquímicos, infrarrojos o de semiconductor para la detección de gases.

Estos son solo algunos ejemplos, pero existen muchos otros tipos de sensores para diferentes aplicaciones. Es importante tener en cuenta que las señales generadas por los sensores generalmente son analógicas, es decir, varían en forma continua. Y como se menciono antes se requieren señales digitales para su procesamiento. Por lo tanto, antes de que la señal llegue a la placa de adquisión es común utilizar circuitos de acondicionamiento, lo cual no se abordara acá, y convertidores analógico-digitales (ADC) para transformar las señales analógicas en señales digitales comprensibles por el microcontrolador. Esto ultimo es de vital importancia y se vera en la siguiente sección.

```Note
Si quieres aprender más sobre sensores y sus aplicaciones, puedes consultar los siguientes recursos:

1. [Introducción a los sensores y transductores](https://www.electronicshub.org/sensors-and-transducers-introduction/)
2. [Tipos de sensores con sus diagramas de circuito](https://www.elprocus.com/types-of-sensors-with-circuits/)
```