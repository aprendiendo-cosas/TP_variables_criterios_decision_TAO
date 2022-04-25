# Creación de variables y de criterios sobre el problema decisional planteado


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

![lineal](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/imagenes/lineal.png)



## Creación de un mapa de aptitud desde el punto de vista de la erosión

+ Fuente de información: [Mapa de erosión de Colombia](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/geoinfo/P_erosion_2010_2011.zip). 2010-2011. IGAC Instituto Geográfico.
+ Estructura de datos:
  + Fichero de formas poligonal.
  + El campo *Clase* muestra el tipo de erosión presente.
  + El campo *Grado* indica la intensidad de la erosión mediante un texto.
  + El campo *RULEID* muestra la intensidad de manera cuantitativa:
    + 1: Erosión muy severa
    + 2: Erosión severa.
    + 3: Erosión moderada.
    + 4: Erosión ligera.
    + 5: Sin evidencias de erosión.
    + 6: Sin suelo con afloramiento rocoso.
    + 7: Cuerpos de agua.
    + 8: Zonas urbanas.
+ Construcción del criterio: A valores más bajos de la variable (*RULEID*) más aptitud. Por tanto es una función de transformación lineal e inversa. Aunque como la leyenda de la capa original está "invertida" (los valores más altos indican menos erosión), vamos a calcularla expresamente:

![estandar_erosion](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/imagenes/estandar_erosion.png)

+ Flujo de trabajo:  [Aquí](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/workflows/erosion.drawio.zip) se puede descargar el esquema mostrado a continuación.

<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;toolbar&quot;:&quot;zoom layers tags lightbox&quot;,&quot;edit&quot;:&quot;_blank&quot;,&quot;xml&quot;:&quot;&lt;mxfile host=\&quot;Electron\&quot; modified=\&quot;2022-04-25T14:07:01.479Z\&quot; agent=\&quot;5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/537.36 (KHTML, like Gecko) draw.io/17.4.2 Chrome/100.0.4896.60 Electron/18.0.1 Safari/537.36\&quot; etag=\&quot;pYbPJMiZFT8UrGwzLcwG\&quot; version=\&quot;17.4.2\&quot; type=\&quot;device\&quot;&gt;&lt;diagram id=\&quot;memp_5-ESO-QEF7mi9qf\&quot; name=\&quot;Page-1\&quot;&gt;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&lt;/diagram&gt;&lt;/mxfile&gt;&quot;}"></div>
<script type="text/javascript" src="https://viewer.diagrams.net/js/viewer-static.min.js"></script>


## Creación de un mapa de aptitud desde el punto de vista de la distancia a la población de invasora más cercana (INCOMPLETO)

+ Fuente de información: 
  + [Mapa de distribución de los páramos](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/geoinfo/col_paramos.tif) en formato raster. Cada píxel ocupador por páramos tiene valor 1. El resto tienen valor 0.
  + [Mapa de presencias de la especie](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/geoinfo/0219160-210914110416597.zip): Usamos como ejemplo la especie *Ulex europaeus*. Los datos proceden de la información de presencias de GBIF.
+ Construcción del criterio: A valores más altos de superficie, menor idoneidad desde el punto de vista de la vulnerabilidad. Es, por tanto una relación lineal e inversa.


+ Flujo de trabajo:  [Aquí](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/workflows/erosion.drawio.zip) se puede descargar el esquema mostrado a continuación.






## Videos de las sesiones
En esta sección se incrustarán los vídeos de las distintas sesiones. Se subirán a youtube aunque no estarán disponibles públicamente. Solo las personas que tengan acceso a este guión podrán ver los vídeos. 