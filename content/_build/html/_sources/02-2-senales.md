# Señales
Retomando lo dicho en la sección anterior, la señal física al ser capturada por un sensor sufre cierta degradación debido a factores intrínsecos como la resolución y el tiempo de respuesta del transductor utilizado. Esto, junto con el objetivo de proporcionar una señal lo más precisa posible al sistema de conversión analógico-digital, justifica la inclusión de una etapa de acondicionamiento de la señal (({ref}`Fig. 4 <Fig4>`)). Esta etapa tiene como finalidad mejorar la calidad de la señal, eliminar el ruido no deseado y adaptarla a los requisitos del sistema.

```{figure} img/Fig4-Acondicionamineto.png
---
scale: 50%
name: Fig4
---
imagen tomada y modificada de https://es.wikipedia.org/wiki/Adquisici%C3%B3n_de_datos
```
Posterior a está esta etapa, el siguiente proceso que sufre la señal es la transformación de una señal analogíca a digital. 

## Conversión analago-digital (ADC)
La digitalización de señales se lleva a cabo mediante un dispositivo electrónico especializado que convierte una señal analógica continua en una señal digital compuesta por valores discretos que representan la información de la señal mediante valores numéricos (es conocido como un ADC por sus siglas en inglés). Este proceso se divide en cuatro etapas:

En la primera etapa, conocida como **muestreo**, se toman muestras periódicas de la amplitud de la onda para pasar de tener un conjunto continuo de valores a uno discreto ({ref}`Fig. 5 <Fig5>`). La frecuencia de muestreo debe cumplir con el teorema de [Nyquist-Shannon](https://es.wikipedia.org/wiki/Teorema_de_muestreo_de_Nyquist-Shannon) para evitar la pérdida de información.
```{figure} img/Fig5-muestreo.png
---
scale: 50%
name: Fig5
---
imagen tomada *Sedra, A., Microelectronic circuits, Fifth Ed. 2004*.
```

La siguiente etapa es la **retención**, donde las muestras tomadas se mantienen en un circuito especializado durante un tiempo suficiente para permitir su evaluación en términos de nivel (cuantificación). Aunque este proceso es necesario debido a limitaciones prácticas, no se considera en el análisis matemático y no cuenta con un modelo matemático específico.

```{figure} img/Fig6-ADC_esquema-3bits.png
---
scale: 70%
align: right
figclass: margin
name: Fig6
---
El digitalizador más simple conceptualmente - el ADC flash. Imagen tomada de https://www.digikey.com/es/articles/match-the-right-adc-to-the-application
```
En la etapa de **cuantificación**, se mide el nivel de voltaje de cada muestra y se asigna un rango de valores de la señal analizada a un único nivel de salida. Sin embargo, a partir de esta etapa, la transformación de la señal no puede ser invertida completamente debido a la pérdida de información inherente al redondeo, lo que se conoce como ruido de cuantificación. En terminos practicos, la asignación puede visualizarse a través de un circuito conceptual que ayuda a comprender el proeso (({ref}`Fig. 6 <Fig6>`)).

```{figure} img/Fig7-cuantificacion.png
---
scale: 50%
name: Fig7
---
Representación gráfica de un proceso de cuantidicación de 3 bits.
```

Finalmente, en la etapa de **codificación**, se traducen los valores obtenidos durante la cuantificación al código binario, lo que permite el procesamiento de la información. Está etapa se logra en la {ref}`Fig. 6 <Fig6>` gracias al modulo etiquetado como "Encoder". La codificación a binario no es un proceso muy complejo, para ilustrarlo tome como ejemplo el numero 6, si se quisiera llevar esto a la base binaria, se operaria obteniendo los residuos:

\begin{align*}
\text{LSB} \rightarrow & r(\underbrace{\frac{6}{2}}_{\text{Cociente = 3}}) = 0 \\
 & r(\underbrace{\frac{3}{2}}_{\text{Cociente = 1}}) = 1 \\
 \text{MSB} \rightarrow & r(\underbrace{\frac{1}{2}}_{\text{Cociente = 0}}) = 1\\
\end{align*}

Con esto, el numero 6 en represenación binaria resulta en `110`.

## Resolución
La resolución de un ADC hace referencia a la mínima escala de voltaje que es capaz de identificar el cuantificador. Claramente esto dependerá entonces de los voltajes de referencia y de la cantidad de bits que contiene el conversor (cuantos niveles hay disponibles). Si se toma un caso particular como el del Arduino, que usualmente cuenta con un ADC de $n=10$ bits y unos voltajes de referencia de $0$-$5$ V, se puede estimar la resolución del ADC como:

$$ R = \frac{\Delta V}{2^n} = \frac{5 V}{2^{10}} = 4.8 mV.$$

```{admonition} info
Acá se deja un link con un código que simula el muestre de una señal.
[https://colab.research.google.com/drive/1hiGdkQvX7JDZTz59b5MeMpeyOZgs5sOa?usp=sharing](https://colab.research.google.com/drive/1hiGdkQvX7JDZTz59b5MeMpeyOZgs5sOa?usp=sharing)
```

```{note}
Si quieres aprender más sobre el procesamiento de señales, puedes consultar los siguientes recursos:

1. [Adquirir una Señal Analógica: Ancho de Banda, Teorema de Muestreo de Nyquist y Aliasing](https://www.ni.com/es-co/shop/data-acquisition/measurement-fundamentals-main-page/analog-fundamentals/acquiring-an-analog-signal--bandwidth--nyquist-sampling-theorem-.html)
2. Teledyne SP Devices, *The art of pulse detection*, 2018, [pdf](https://www.spdevices.com/documents/application-notes/73-pulse-detection-application-note/file).
```