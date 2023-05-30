# Sistemas de adquisición
```{contents}
```
Una vez la señal a registrar se ha digitalizado, esta es adquirida y enrutada hacia la nube a través de los getway por una unidad de procesamiento y control programada con este fin. Como se comentó antes, este sistema de computo puede variar según las necesidades del propio sistema, pero en términos generales uno puede destacar cuatro tipos de tecnologías que se pueden emplear: Los microcontroladores, los microprocesadores, los microcomputadores e incluso las FPGAs. Estos se han nombrado en orden de complejidad interna y poder de computó. 

## Microcontroladores
```{figure} ../_static/img/microcontroladores.png
---
scale: 40%
align: right
figclass: margin
---
Microcontroladores.
```
Un microcontrolador es un circuito integrado o "chip" compacto y altamente integrado, que combina en un solo encapsulado un gran número de componentes y que tiene la capacidad de ser programado. Está especialmente diseñado para controlar y supervisar sistemas en tiempo real. Los microcontroladores son reconocidos por su eficiencia energética, tamaño reducido y bajo costo. Además, suelen contar con una arquitectura RISC (Reduced Instruction Set Computer) que los hace adecuados para aplicaciones con recursos limitados, lo cual es ideal para proyectos de IoT y sistemas embebidos de diversos niveles de complejidad. Un microcontrolador debe incluir tres elementos básicos en su interior: una unidad central de procesamiento (CPU), varios tipos de memorias (volátiles y persistentes) y diferentes tipos de entradas y salidas. Algunos de los microcontroladores más comunes y populares en el mercado son el PIC (Peripheral Interface Controller) de Microchip, el STM32 de STMicroelectronics y los diferentes microcontroladores de la familia AVR de Atmel, que son lo que se integran en las placas Arduino. [ref Torrente, O. 2013]

## Microprocesadores
```{figure} ../_static/img/microprocesadores.png
---
scale: 40%
align: right
figclass: margin
---
Microprocesadores.
```
Son dispositivos electrónicos que contienen una unidad central de procesamiento (CPU), junto con otros componentes esenciales para su funcionamiento, pero, a diferencia de los microcontroladores, los microprocesadores generalmente no incluyen periféricos integrados en el chip, y se espera que se conecten a través de buses externos. Además, los microprocesadores suelen tener una memoria caché de nivel 1, pero requieren módulos de memoria externos para el almacenamiento principal. Los microprocesadores son más potentes y pueden incluso llegar ejecutar sistemas operativos completos y aplicaciones más complejas. Estos chips son conocidos por su capacidad de procesamiento, flexibilidad y capacidad para ejecutar tareas exigentes. Algunas de las características comunes de los microprocesadores incluyen múltiples núcleos de procesamiento, altas frecuencias de reloj y cachés de memoria de gran tamaño. Entre los microprocesadores más populares se encuentran los fabricados por Intel y AMD, como los procesadores Intel Core i7 y los AMD Ryzen. 

## Microcomputadoras
También conocidos como microordenadores de placa única (SBC, por sus siglas en inglés), son dispositivos compactos que integran en una sola placa un microprocesador, memoria, entradas/salidas y periféricos, así como puertos de conexión como USB, HDMI, Ethernet, entre otros. Estos dispositivos son similares a los microprocesadores en cuanto a su capacidad de ejecutar sistemas operativos completos y aplicaciones más complejas, pero se diferencian en que incluyen componentes adicionales para brindar capacidades de conectividad y expansión. Algunos de los microcomputadores más comunes y populares en el mercado incluyen la Raspberry Pi, la BeagleBone Black y la Odroid.
```{figure} ../_static/img/microcomputadoras.png
---
scale: 40%
---
Microordedenadores.
```

## FPGA
Son dispositivos electrónicos que ofrecen una alta flexibilidad y capacidad de personalización en términos de hardware. A diferencia de los microcontroladores y microprocesadores, las FPGA permiten configurar y reconfigurar la lógica digital en el campo, lo que significa que se pueden adaptar y modificar según las necesidades específicas de una aplicación. Estos dispositivos contienen una matriz de bloques lógicos y elementos de interconexión que pueden programarse para realizar funciones lógicas y de procesamiento. Las FPGA son ideales para aplicaciones que requieren un alto rendimiento, procesamiento paralelo y adaptabilidad, como sistemas de comunicaciones, procesamiento de señales, aceleración de algoritmos, prototipado rápido y más. Algunas de las FPGA más comunes y populares en el mercado son las de Xilinx, como las series Virtex y Spartan, y las de Intel (anteriormente Altera), como las series Cyclone y Stratix. La últimas tendencias en este campo apuntan a placas que integran la teconolgía FPGA y los microprocesadores. 

## Programación
Es claro, entonces, que la elección de la tecnología a utilizar como unidad de adquisición y procesamiento dependerá de las características del proyecto. Una vez definido esto, el siguiente paso es la programación de estas tecnologías. Cada una tiene su enfoque específico. Los microcontroladores se programan en lenguajes de bajo nivel como C o ensamblador. Sin embargo, es importante mencionar que existen alternativas de alto nivel que simplifican su uso, como entornos de desarrollo integrados o IDE. Por otro lado, los microprocesadores suelen ser programados utilizando lenguajes de alto nivel como C++, Java o Python, y se ejecutan en sistemas operativos. Los microcomputadores también admiten una amplia gama de lenguajes de programación y frameworks, lo que los hace flexibles en términos de desarrollo de software. En cuanto a las FPGA, se programan utilizando lenguajes de descripción de hardware (HDL) como VHDL o Verilog, que permiten describir la lógica y las conexiones dentro del dispositivo. Estos lenguajes se sintetizan y se cargan en la FPGA para su funcionamiento.

## Placas de desarrollo 
Hoy en día, la programación de microcontroladores y otros dispositivos no se realiza directamente quemando el programa en el microcontrolador, sino que se aprovechan los ecosistemas de desarrollo que se han creado. Estos ecosistemas ofrecen placas de desarrollo y entornos amigables y accesibles para prototipar y desarrollar proyectos con microcontroladores, microprocesadores y otros dispositivos. Un ejemplo destacado es Arduino, una plataforma ampliamente utilizada y de código abierto que proporciona una placa de desarrollo basada en microcontroladores. Cuenta con un entorno de programación intuitivo basado en C++ y una comunidad activa de usuarios. Otra placa popular es ESP32, que combina un microcontrolador de alto rendimiento con capacidades de conectividad Wi-Fi y Bluetooth. Es compatible con el entorno de desarrollo de Arduino, lo que lo convierte en una opción ideal para aplicaciones IoT. Por último, la Raspberry Pi es una placa de microcomputadora que permite ejecutar sistemas operativos completos y ofrece una amplia gama de puertos y capacidades. Esto la convierte en una elección versátil para proyectos de computación y automatización.
```{figure} ../_static/img/placas_de_desarrollo.png
---
scale: 40%
---
Placas de desarrollo.
```
A continuación, se mostrará cómo utilizar estas herramientas para el diseño del sistema electrónico de dispositivos IoT.

