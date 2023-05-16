# Curso-Introductorio-IoT
Este curso introductorio proporcionará herramientas generales para la construcción de sistemas basados en Internet de las Cosas (IoT) y dará una visión general de los requerimientos técnicos y de diseño para la implementación de estos sistemas en diferentes entornos científicos e industriales.  Se cubren temas como la adquisición,  el procesamiento y la transmisión de datos mediante sistemas basados en Arduino, así como el almacenamiento y la persistencia de estos utilizando bases de datos como MongoDB. Además, los participantes aprenderán los fundamentos de la gestión y visualización a través de APIs basadas en Python.  Al finalizar el curso, se tendrá una comprensión sólida de las tecnologías básicas de IoT y estarán listos para aplicarlas en proyectos más avanzados.

Para acceder al curso interactivo diríjace a [http://www.iotintro.com/](https://danielestrada971102.github.io/Curso-Introductorio-IoT/intro.html)

# Contribuciones
Si eres contribuidor de este proyecto, por favor, trata de seguir las pautas para publicar tus aportaciones. Esto es con el fin de preservar la integridad del repositorio y evitar fallos en el sitio desplegado. La construcción de Jupyter Book está completamente de acuerdo con lo indicado en el [sitio oficial](https://jupyterbook.org/en/stable/intro.html). Para ello, es necesario instalar las dependencias adecuadas. Con el fin de evitar esto, se incluye el archivo requerimets.txt para facilitar la inicialización de un entorno virtual donde se pueda reconstruir el notebook sin generar conflictos. En términos generales, para publicar los cambios en Jupyter Book se sigue la siguiente metodología:

1. Realizar los cambios en los archivos dentro del directorio `/content` o crearlos en caso necesario (No olvides actualizar la estructura de `_toc.yml` si es necesario).
2. Reconstruir Jupyter Book con el siguiente comando:
```bash
jupyter-book build content/
```
Esto actualizará y construirá los archivos HTML necesarios para desplegar el sitio.
>**Warning**
>Puede ser necesario, para evitar inconsistencias con el contenido que se motrará en le sitio, borrar el directorio `_build/` que se genera cuando se construye el Jupyter Book. De no borrarlo, puede ocurrir que contenido viejo que se borro en una versión más reciente quede como remanente en los archivos.

3. Confirmar y hacer `push` a los cambios en la rama principal del repositorio.
4. Actualizar la rama `gh-pages` con el siguiente comando:
```bash
ghp-import -n -p -f content/_build/html
```
Esto actualizará y dejará publicado Jupyter Book.

Estos pasos se toman de la documentación oficial, por lo tanto, si tienes algún inconveniente o deseas conocer más detalles, puedes consultar allí.

# Reporte de problemas
Como en cualquier proyecto de GitHub, la mejor forma de dar seguimiento y solución a los posibles inconvenientes que puedan surgir es mediante el uso de los `issues`. Ante cualquier problema, te invitamos a abrir uno nuevo y se tratará de resolver tan pronto como sea posible.