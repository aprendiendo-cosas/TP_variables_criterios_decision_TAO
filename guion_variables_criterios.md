# Construcción de un modelo conceptual para responder a la pregunta planteada


> + **_Curso_**: DECISION - Uso de información sintética como apoyo a la toma de decisiones. Proyecto TAO (Tropical Andes Observatory)
> + **_Autor_**:  Curro Bonet-García (fjbonet@uco.es)
> + **_Duración_**: 3 horas.



## Objetivos del acto docente

El objetivo general de esta sesión es preparar las variables que necesitamos para implementar el modelo conceptual descrito en las clases anteriores. De manera más concreta, trabajaremos los siguientes objetivos específicos:

+ Reconocer las fuentes de datos necesarias para generar los mapas de las variables útiles en nuestro trabajo.
+ Procesar las fuentes de datos primarios para que se ajusten a nuestras necesidades.
+ Transformar los mapas de distribución de las variables en mapas de criterios.

En esta sesión se describe cómo trabajar con una serie de variables. No dará tiempo a trabajar con todas las que hemos propuesto en el mapa conceptual. Se intentará que estén representadas todos los tipos de procesamiento previsto. En concreto trabajaremos con las siguientes variables:

+ Variables relacionadas con la vulnerabilidad a la invasión:
  + Erosión
  + Superficie del parche.
  + Distancia de cada punto a la población de invasora más cercana.
  + Fricción al desplazamiento provocado por el relieve o por la idoneidad del hábitat para la invasora.
+ Variables relacionadas con la dispersión de la invasora:
  + Distancia a vías de comunicación.



## Preparación de un proyecto de QGIS genérico

Vamos a crear un proyecto de QGIS vacío para ir procesando en él la información que necesitamos. Damos los siguientes pasos:

1. Creamos un proyecto y lo guardamos en una carpeta que no esté en el escritorio. En el nombre no poneos ni espacios ni tildes. Ej. paramo_colombia.qgz
2. Ahora vamos a desplegar una fotografía aérea de la zona de estudio. Para eso nos conectamos a un servicio WMS. Una opción fácil es instalar un complemento (plugin) llamado *QuickMapServices*. Este complemento permite añadir ortoimágenes de Google, Bing y otros. Una vez instalado, vamos al menú web de QGIS y seleccionamos "Bing Satellite" como imagen de fondo.
3. Cargaremos en primer lugar un shapefile que muestra la distribución de los páramos de Colombia. Será nuestra zona de estudio. Descarga [este](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/geoinfo/COL_paramos.zip) archivo (COL_paramos.zip) en la carpeta de trabajo. Descomprime el archivo y carga el fichero de formas al proyecto anterior.
4. También cargaremos [esta](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/geoinfo/col_paramos.tif) versión rasterizada de la capa anterior. Nos servirá como referencia para que todas las demás que generemos tengan su misma resolución (500m), número de filas y de columnas. Descarga el archivo anterior y guárdalo en la carpeta de trabajo. Luego cárgalo al proyecto de QGIS que tengas abierto. 

Otras consideraciones generales:

+ Trabajaremos siempre con el sistema de referencias EPSG 3116.
+ Nuestro análisis se realizará en formato raster y la resolución de cada capa será de 500 m.
+ Todas las capas obtenidas deberán de tener la misma extensión. 



## Aspectos teóricos de la estandarización de criterios

En alguna ocasión hemos comentado que las variables (capas que representan la distribución espacial de un factor importante) han de ser transformadas en criterios decisionales (capas que muestran la distribución de la idoneidad desde el punto de vista de nuestro problema). Así, por ejemplo, un mapa de distancias a carreteras puede convertirse en varios criterios en función de la decisión que debamos adoptar. Si el problema decisional implica que a más distancia de cada punto a una carretera, peor, tendremos una capa de criterio diferente a que si consideramos lo contrario. El proceso por el cual se transforman las variables en criterios se denomina estandarización. Y además implica que la leyenda de las capas se normaliza a valores de 0 a 1 o de 0 a 255. 

Hay muchas formas de transformar una variable en criterio. Estas formas dependen de la función matemática utilizada. La siguiente figura resume el concepto y muestra algunos ejemplos de funciones de transformación.

![transformacion](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/imagenes/estandarizacion.jpg)



En este curso usaremos solo funciones de transformación lineales. Son las más fáciles de implementar y se ajustan bien a lo que necesitamos. Para calcular los parámetros de las funciones de transformación usamos los puntos conocidos de la recta. Siempre sabremos que el valor más bajo de la variable tomará el valor más alto de aptitud o el más bajo (dependiendo de si la función es inversa o directa). Lo mismo ocurre con el valor más alto de la variable. Esto nos permite despejar fácilmente los parámetros de la ecuación de la recta. La imagen inferior muestra cómo proceder en ambos casos. 

## Creación de un mapa de aptitud desde el punto de vista de la erosión

+ Fuente de información: [Mapa de erosión de Colombia](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/geoinfo/P_erosion_2010_2011.zip). 2010-2011. IGAC Instituto Geográfico.
+ Estructura de datos:
  + Fichero de formas poligonal.
  + El campo *Clase* muestra el tipo de erosión presente.
  + El campo *Grado* indica la intensidad de la erosión mediante un texto.
  + El campo *RULEID* muestra la intensidad de manera cuantitativa. Valores más bajos indican mayor intersidad. 
+ Flujo de trabajo:  











+ Paso a paso:
  + ddd
  + ddd
  + 












## Videos de las sesiones
En esta sección se incrustarán los vídeos de las distintas sesiones. Se subirán a youtube aunque no estarán disponibles públicamente. Solo las personas que tengan acceso a este guión podrán ver los vídeos. 