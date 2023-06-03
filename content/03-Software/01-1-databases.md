![images](../_static/img/mongoLogo.png) 

# Persistencia de la información
```{contents}
```
```{warning}
Esta sección será actualizada próximamente para incluir explicaciones más detalladas sobre MongoDB y su arquitectura.
```
Dentro del backend, una vez los datos son recibidos desde el sistema electrónico, ya sea a través de comunicación serial o de protocolos web, una de las cosas más importantes es garantizar la persistencia de la información, preferiblemente de forma estructurada para facilitar su manipulación. Con este fin, en el diseño de los sistemas IoT es indispensable hacer uso de las bases de datos.

## Bases de datos
Una base de datos es una colección estructurada de información almacenada en un medio digital, como discos duros, servidores o en la nube. Los datos se gestionan a través de un sistema de gestión de bases de datos (Database Management Systems, DBMS) para administrar y controlar la información, con el cual podemos interactuar mediante consultas usando un lenguaje específico como SQL (Structured Query Language). Las consultas permiten realizar acciones en la base de datos, como crear, modificar, almacenar y recuperar datos de manera eficiente.

```{figure} ../_static/img/sqlAndNoSql.png
---
scale: 45%
align: right
figclass: margin
name: sqlAndNosql
---
Tipos de bases de datos.
```
En este contexto se pueden distinguir varios tipos de bases de datos (ver {ref}`Fig 23. <sqlAndNosql>`), que pueden ser SQL o NoSQL[^1]. Particularmente, usted seguramente está familiarizado con: Las relacionesles (RDB - *relational data base*) y las documentales.  Las primeras son las más comunes y consisten en bases de datos donde la información se organiza en tablas diferentes, las cuales pueden tener o no vínculos entre ellas mediante claves primarias y secundarias (o foráneas). En la {ref}`Fig 24. <brelacional>` se ilustra esto, se puede observar cómo tres tablas distintas están relacionadas mediante sus claves. Dentro de esta categoría, uno de los gestores más significativos es **MySQL**.

```{figure} ../_static/img/brelacional.png
---
scale: 40%
name: brelacional
---
Estructuración de la información en una base de datos relacional.
```

Por otro lado, las bases de datos documentales son una de las principales variantes de las bases de datos no relacionales (NoSQL). Se caracterizan por almacenar la información en registros, cada uno de los cuales funciona como una unidad autónoma de información. Como su nombre lo indica, esos registros se almacenan como documentos con información semiestructurada, generalmente utilizando la notación de objetos de JavaScript (JSON). Dentro de esta categoría, un gestor significativo es **MongoDB**. En adelante nos enfocaremos en este último ya que es de código abierto y, general, es fácil de usar. 

## MongoDB

MongoDB es un sistema de gestión de bases de datos NoSQL documental de código abierto y altamente flexible. Los datos se almacenan en documentos BSON (Binary JSON), similares a los objetos JSON pero con la capacidad de representar tipos de datos más complejos. Cada documento contiene pares clave-valor, donde las claves son cadenas de texto y los valores pueden ser diversos tipos de datos como números, cadenas, booleanos, arreglos y documentos anidados. Esta flexibilidad permite a los desarrolladores almacenar y consultar datos de manera más natural, adaptándose fácilmente a los cambios en la estructura de los datos sin requerir modificaciones en el esquema de la base de datos.

En MongoDB, la estructura de la base de datos se basa en colecciones, que son el equivalente a las tablas en las bases de datos relacionales. Cada colección puede contener diferentes documentos, que son el equivalente a los registros en las bases de datos relacionales. Los documentos contienen información estructurada en formato JSON. Esta estructura es visualizada de forma gráfica en la {ref}`Fig. 25 <mongoEstructura>`.

```{figure} ../_static/img/mongoEstrucura.png
---
scale: 40%
name: mongoEstructura
---
Estructuración de la información en la base de datos documental MongoDB.
```

MongoDB también proporciona un conjunto completo de funcionalidades para consultas y operaciones avanzadas. Utilizando su lenguaje de consulta y manipulación de datos llamado Aggregation Framework, los desarrolladores pueden realizar consultas complejas, operaciones de agregación, indexar los datos para mejorar la velocidad de las consultas y realizar análisis en tiempo real.

Para comenzar a trabajar con MongoDB, es importante tener en cuenta que existen dos variantes importantes que se utilizan en diferentes contextos. Por un lado, está **MongoDB *Community Edition***, que es la versión gratuita y de código abierto de MongoDB. Está diseñada para ser instalada y ejecutada en tu propio entorno, ya sea en tu propio servidor o en la nube. Proporciona todas las características y funcionalidades básicas de MongoDB, lo que te permite almacenar, consultar y manipular datos de forma flexible. MongoDB Community Edition es ideal para aquellos que desean tener un control total sobre su infraestructura y desean personalizar la configuración y el despliegue de su base de datos MongoDB.

Por otro lado, **MongoDB Atlas** es un servicio de base de datos gestionado y completamente administrado a tráves MongoDB cloud. Es una opción en la nube que te permite desplegar, escalar y administrar fácilmente tus bases de datos MongoDB sin preocuparte por la configuración de la infraestructura subyacente. MongoDB Atlas se encarga de tareas como el aprovisionamiento de servidores, la configuración de la replicación, las copias de seguridad automáticas y la escalabilidad, lo que te permite centrarte en el desarrollo de aplicaciones en lugar de la gestión de la base de datos. Es especialmente útil para aquellos que desean una solución simplificada y sin problemas de administración de bases de datos MongoDB, con características adicionales como el monitoreo integrado y la distribución geográfica de datos.

Para usar MongoDB Community Edition, simplemente debes realizar la instalación en tu máquina local. Puede referirse a la [página oficial](https://www.mongodb.com/try/download/community) para descargarla. Una guía detallada del proceso se puede encontrar en ["MongoDB Curso, Introducción Práctica a NoSQL"](https://www.youtube.com/watch?v=lWMemPN9t6Q). Una vez completada la instalación, puede desplegar un servidor local de MongoDB y realizar operaciones **CRUD** (Crear, Leer, Actualizar, Eliminar). Para nuestro sistema IoT, el uso de MongoDB Atlas proporciona una gestión simplificada, escalabilidad y características avanzadas de seguridad para bases de datos MongoDB.

Para usar MongoDB Atlas, es necesario crear una cuenta. Una vez registrado, podrás crear un proyecto y desplegar un servidor que aloje la base de datos. Allí podrás crear un usuario y recibirás las credenciales necesarias para poder conectarte y comenzar a realizar operaciones CRUD. Un paso a paso de este proceso se puede encontrar en el video ["MongoDB Atlas, NoSQL en la Nube"](https://www.youtube.com/watch?v=Imwk0HtEuGY). En la [Actividad 4][1] podrá ver como utilizarlo de forma práctica. 


[^1]: Se usa el termino SQL y NoSQL para diferencias aquellas que se gestionan usando lenguaje de consulta SQL o otros diferentes como CQL (*Contextual Query Language*), GQL (*Graph Query Language*), o JSON (*JavaScript Object Notation*).

[1]: 01-2-activity-4.md
