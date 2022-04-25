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

<iframe frameborder="0" style="width:100%;height:1114px;" src="https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=erosion.drawio#R5VtZb9s4EP41BtoFYlAHZfsxdtxugbYbJNvd7VPAyLStrixqKaqx8%2BuXFEldVBIltuWjQOuIw0PUzDcHh2TPmazWHymKl1%2FIDIc9G8zWPeeqZ9se8PivIGwkwRqNhpKyoMFM0QrCbfCIFREoahrMcFJpyAgJWRBXiT6JIuyzCg1RSh6qzeYkrL41RgtsEG59FJrUv4MZW0qq7TheUfE7DhZL%2FWoPurJmhXRr9SnJEs3IQ4nkTHvOhBLC5NNqPcGh4J5mjOz34YnafGYUR6xNBxC4F9%2BwPZiCy8XoUzD%2BMXz8cGHLUX6iMFVf%2FAXFSEgMCw7GLGDpTEweL3oTpze%2BjHgBU5IEWdHhRS%2Fkrx%2FfU%2F60EE85RRP4KHdZFxL1WTDX9Xym90WfjENso%2FnOmRWLxxnx01X2geOHZcDwbYx8QX%2FgcOO0JVuFvGSJ15E0muHZ53tNMPmjWPYTU4bXJZLi10dMVpjRDW%2Biau2hkp2C7wVU5YcCC5aW77IEA90PKfgt8qELAfEHJaNXyMsx5GWwDs84glWRULYkCxKhcFpQx1SyitcDXirafCYkVsz7gRnbKHVEKSNVXuN1wP4R3ftQlb6rwcTz1bpc2KjCk%2BJISEp9%2FNw3D5TiI7rA7JmGSicFA56VLsUhYsHPqoo3iUp1vSYBn3OOCsetogLWpS2%2FSPWqCTyfxhYYGB4FCN4uULelPLXgdyfQrXTPfdZWVo0iNzI%2BIzRAYZ8Xpte3H%2Fkfx7K8nnPJjR5aCesV3Sdx2WA%2BbUKvtQG9s4EFxI91J0brJ8u42Z5KWhKjqAIM779U%2BJuxT0JCs7kAurhH7ziC%2BT%2FOF9D49F48imG5B43YxRytgnAju69IRJLMKleaJBlsRAMQr4v36gnWQwbI5cB%2FpDvMS1ouMJMMp1yJZzErKEQBuThfamvlbTVU3zSMXQwj5Z%2FXSAxAiQKocWBDjgQbZliAVTRAjQeoEJGPLMWUj6w1VBDEXCsuUBAzJyjomVYKipUVS3OVrq4NN%2FKqnBGFgkFh%2BwpeDAvOgU1Bh16JLj1kMXqJ29pT5pXlflxrc%2FqiNIG6nLJiLqwysQoh1c7AWq4zUkWeCEOqRvKFCGQXAYdTdS0D1ww47KaAw9tXwAGPwtdwjtJNKeIQxe%2FluiLmyErbBx1eWx9lHybocEBN4nKmews6PAMHNyhhmAaPiJpqsySr%2BzR5WWWqwNiFAo1qbBo1ROygQYGsOj93pkGDo9Cgt2vCsKUm7FwRtuK6GSN%2FSCO%2FFKBlQRujKErmhK5QXiXitQkK%2FTTky3UqwjuaAf2YQF5blg5brkr3h3HLzCOcFsgtnVzqfI25Hd8tg%2B833z5PP10Z3OfQYlVmJYySf%2FFELgSuIhIJEcyDMKyRUBgsIl70OVO5GjhjAdTAR%2BGlqlgFs1kmvyY92Dn4LViz8J6Jfq8B%2FM7esO8eNiIqx0Ol8GjnEZHlnKaKmCkzPgdK8ZwT0yhgya%2BjKy44tK54J%2BYW4Gli3ly1yd2LbL0O1IaFyNfwKgA1Kn4JFWhI4XerAsODugvrde5CZ%2FovQB8MK8n%2BPnTdZxP%2BonDNF4icbQIH23qfw24CXFKKNqUGsVhnJ6WRa8t1t75cH3pl5LzYXpcLpMkZ7HRNb5lL0%2BmaZVlBZRp8Et5xaaEVSeTW3bmaCXcIq%2FIaHNhM2KZNPgEzAaoWYk8bgtbopG2B68IjtAUjwxZ8JVeIodwW%2FIV8kSK5vCK88O6rSLKDBLP3Z2wUBjVBOKZRGHVqFA6SVXmrUXi7gtsHy8bsRMEhOEIFt81U0R8pi1Mmd2krB3TOVqFriVO3YXegW4WGh1DoLRSzbQ7Idnetmdvx2UwCvfLshuuJCP4tBzWePqNR31DoerO5ZoYs0JAesps2y%2Fa222wfJD%2FU%2Fe6y3fYI1JGpkXkE6gbHlGy4whz1NvCw4RxFx9vA9knlfbaAdttEze5dxBMnJ%2BpYGNVEvOeTE7aZZSkHXs8f7TvbSMyrCWUADxuJOZbB7LPcxbPb5k66Uk%2FDVOtMXFfqaSY%2B%2FpS8scHk5jZPf8ijmueqj1ZdCg362Gn%2B0znwrnpXkaA%2BP%2FiiPg6PKhLU0263oFInqDJ%2FZyyZXns1qPNlUi1pYDfFkp2eyXVObBfdabuLfmQYN3fRL0DfcZzfivsMCrw9F1jlSw5jq%2B95Z%2Bwuat7CskxvYbmdugszyL7FIfZ9LhyxMgUxCeXuxULcV%2BllN0Zle0Jxou2WJebG%2F0Pzjs7Rrm1h0%2FHPbte2WtQnY5DaBsFHdgPNMWPVhstkygrpg6CFVQJ5Y%2BlZs%2FtCAEUzWdWiX6j6mWnrszFtbu3AkNdg2wZdmjbXOWwk3NXK1G0bCR%2FXWTs97brnYeLmHphjxFLpYEgUbs5Xbern7KBleqUdLSB76o5fKQ1Q3O5zpv8D"></iframe>


## Creación de un mapa de aptitud desde el punto de vista de la distancia a la población de invasora más cercana 

+ Fuente de información: 
  + [Mapa de distribución de los páramos](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/geoinfo/col_paramos.tif) en formato raster. Cada píxel ocupador por páramos tiene valor 1. El resto tienen valor 0.
  + [Mapa de presencias de la especie](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/geoinfo/0219160-210914110416597.zip): Usamos como ejemplo la especie *Ulex europaeus*. Los datos proceden de la información de presencias de GBIF.
+ Procesamiento: Usaremos un algoritmo de QGIS que asigna a los píxeles de una capa etiquetados con un valor (*target*) su distancia a los píxeles con otro valor (*source*). Es decir, asignaremos la distancia de los píxeles de páramo a la población de especie invasora más cercana. 
+ Construcción del criterio: A valores más altos de distancia, menor idoneidad desde el punto de vista de la vulnerabilidad. Es, por tanto una relación lineal e inversa.


+ Flujo de trabajo:  [Aquí](https://github.com/aprendiendo-cosas/TP_variables_criterios_decision_TAO/raw/main/workflows/distancia_poblaciones_invasora.drawio.zip) se puede descargar el esquema mostrado a continuación.

<iframe frameborder="0" style="width:100%;height:2753px;" src="https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=distancia_poblaciones_invasora.drawio#R7V1rc6O2Gv41mUk74wxCXD%2FGzu6edtrtJad7tv2yI4NiswcjKiCx8%2BsrcTEG4UAWg4jdL7uRQFi8ep%2F3LnEFF5vtB4rC9c%2FExf6VqrjbK3h3parA0gH7j%2Ffssh5NNbSsZ0U9N7%2Br7Lj3nnHeqeS9iefiqHJjTIgfe2G10yFBgJ240ocoJU%2FV2x6IX%2F3VEK2w0HHvIF%2Fs%2FZ%2FnxuusV4XQKC%2F8B3urdfHThp6%2F4AYVd%2BevEq2RS54OuuC7K7ighMTZX5vtAvucfAVhsnHvj1zdz4ziIO4yQPG02R9YNd8ptyv7B2%2F%2B1Xp%2BP1OzpzwiP8nf%2BGcUIr5imFMwjL04cfnk8epqAa%2FmtwFrYEoiL21C1jR89vPzJWV%2Frfhf%2B56igz3lSzqEBDex91BcZzNdlmNSCsW7gu6MWCH%2F0yVOsklfcP609mJ8HyKH9z8xfmN963jjsxbgP0eSwMXuT8uiQ6RPTrJHTGO8PejK6fUBkw2O6Y7dUlxV7Xzxcv6dAaPoeSrZARRLvD7gBCvvQzkHrvZPL9eI%2FZEv0yuWzBSWTKAedhkT501C4zVZkQD578reOc2oxa4rrFXe8xMhYU6%2FrziOdzkiURKTKrmPUjciCXXwC%2FO3chgjusJxO2vyd3lxrSj2Uew9VgF7cqpbAtXfJ4FToiADTExRED0QukH7Szese4F8J%2FEZ%2FCmHFkVRjKmwaHRNNsskauf06toNw%2Ba60ZHNgTIUnxci9q3wNdQ7MrY1KcYupn3A2dfst95fKzNN001VuzEsU%2F%2Fuu%2B%2BZrP474cpqHlKy9Taei9wvFDuMyMhFV5oCyjtm1wdjGx4mrCzj1Li6EFFMyf%2FxgviEsp6ABHx5Hzzfr3Uh31sFrOmwBWOognPO9x7T4Lf5BTZRN%2BWNJlidHEu2LkAJmje6ACaoNIBJ1%2FsvMsCPn6Mf%2F%2Fq48qn592y%2B%2FPr5E5hBGVBiRKS7z3w8e%2F%2B8%2Befhtbtt%2FvCstausQg0erZhsfO%2FCZDyAZON96kgIfGmSBwBcRI%2BZTcv%2B9dnvsZenOMJM36SNVNX4XJXgKMSOhzsYYYoKbGAoMyawbaABoGjMVLXNG4f%2FVAeDrMoNLRrqCGga1vAojiyjiiOmZ0SNpDaByDiBQmpcJ1FQylNQJ0GH2hEdmkx0iB7Kgs0XUU4Ejo%2FfPvxwn4JlQ9J3WXJgnNK06sXGNqiysaU12FVNXHwSu6qRoFPyH07CxlpHNjZksrEmUP0e%2B9hxmFOc8jJKQwYB52HkP3ODiol4kor%2BJIjJXuwHCWaWEb%2F6zEYWvawvcT0yJcZXqoxv6F0dCnUoxrfPjfGNjoxvyWR8Q6D6hwRRN2X6GoNHe0S4vD0dXlarvKwD0Z63BgoBNetEMRohg5UnYOA3BJOaHSDYEwP50F%2BJx6ZYckbNSoV14ZVNLB9VW%2FX9NL6dEUQ%2BOAzfuh5TFd4yEcJUVd8hs6PSyH3Ib7wFFHFjqtWnSHy8%2FaJB1ejkQYyNWqvmicOGyK3apIEGcyA0URa%2BcQ0EujrYmi5TBwHRxf49DcJ6z1wPTUfR1LyF2Z4%2F5bkLQHTA5OqaQ01zoHhGCCbBjswO%2BgZ0m5WNJUTptVq8cGBtUxDggBU%2BIR6MrfPDlKK5vQBp1qO5zPgTAGk04BEOBkftUtHXkF65LPSJ8Uf2VErxA%2BtMAi8WXabzxSGwZONQSnZyCjg0Lx2HYgA1Kw3iU1KVvBqIGW78kqIXhtllwBJKhqUqUntMWILXwRJvvfjzwd8Ho1irHMQbA0BZ7eq9nQbKt5Si3cENIUdodBzpQDHq1heolbi1jgCmUuOpbBYnlQiq6F5%2BJHcoRnsp8Ak5aWTljsf3rz8SPucIx9Oug%2BglGoBiWrXFazCd7VFlg1zH9ZWy4ZQ4H9dx7Y%2FzQo1MDeei%2F%2FtLEocJI%2Bktt8F9vM0KXM8X1GKhoBigGhXUmih7ZaD8hGiFnWOqSvNijRNThSLhR889QAC65R5Gr16qGcagIY47bu4BSo7aSssQwq7KD45VhvviNA9TFDikZIedeNI5igmkKKDcmKg8ww72zaodM6%2BUqu2umuOmtRtq0uuGVir7b6J1eMbWFqhDTZfsQcnZkjGBoCc0BwGa%2FY04E4OiRsuDsjccDrBiTPS%2Fmb5VlcXv9%2Fs4SGawnStgLaUdr6NGQzXJyUJpRl9RZT18xKPf%2BigCbBYUp6WRDtqEPFgYJPiR%2B0OsyUgULKOw0b%2BZTG3kDIrm4Ki1kZpkR0eajtK6Ojq94wZHlFmNE8C36jLr5ecMrMq0o0UuRUbvjBVYUexVgFm2%2Fjq7kn2taxWL1rdo%2F5tC8fV6ZjbflowbqKeAayOGicRrooM4dvCx81EWo9c919cQWqJKHnnnJBCociF2aNdiGTlwB9B6G3g3%2F2WgFgYy7EGMOgCFEp89B7QYdv2Z0TRa0sCa1nsEUMwWjhcpoIyRbNbEkEo3FZftqSv1WdRBoTnE%2F8LYk989Wb0GoNJBsYEmA3U4xSY6CzIE1SkFTtcddbraU%2BD08wzEnW9RsplUokwI38%2BMhiMsRo2N6Gd3hIWudmVXqYdYFNM8IsybZDbjgfkROd%2F91Jc0U7abvmgX6orMpqzyqAUTxjT2WJ8SK50Pyuib7eqHFfGojNsgg4bvRd7%2BJIzyFDLWuM5bcTrPdG%2BnWFwrURcIDG51PUdyuPMw9GmcBDOB0Lne2ejpq0WOFVzo9o1t1DikKNkaqehCFwXePocbets0TJZ28wg4%2BwnlnDO5dVfUskwBrma5ZqNEw3X7QuFZ6PiLhmdBhEvc%2FmnZDfQvTtOQlZwyJMck5cFxNJ9rynAUPbpL3QXahE7TFvXluOiUe%2BTt29oIanT1DE%2BD6FdnBtR6StPK0h3Hg%2FbiCLVtRL2QqX1EPb5nwRfyAl3Gq%2FbNuHvZDdHX%2FnfnqlrfuWpZWsMJ3qNWXhti1dGo4uxGqUs0RW2Tabz1K6YeowBf1pMLra7HX05FaJm29lqh9fYEinjE3eHOjTJmd%2BYbZYUvaliKLVuGmKIHKUOonFIEjFsd8WoRIBTLTLbawGivNkCVzw842RHVWc7qLosOEicJkcsNhTCtl60U1aUnXKf1dyFZ%2BkioWygSU16RlfrDZ%2FAr%2B9lr7S9tsgfzyTiYOogfhN2aC6vLnknmwcTyFOk7h81pnFo6gWKs3OloFzfmMNvFGoQDrGuLofW7WJE9%2FMetepYN1a0ioDZ8hGHUOgxTyr6sEyLB7BoC7K14%2B9FZDNJdlx%2BpOih6q369iv9o%2B13MBVa%2BzxSU8F2s%2BvPO1s4FxaFOJbgaDscfNfJnSk6TSVNPZoM1fPwDdNIweUmWZvl9vOnanKpgcw55Uj5rlp%2B8zayS8tPB8N0%2F"></iframe>




## Videos de las sesiones
En esta sección se incrustarán los vídeos de las distintas sesiones. Se subirán a youtube aunque no estarán disponibles públicamente. Solo las personas que tengan acceso a este guión podrán ver los vídeos. 