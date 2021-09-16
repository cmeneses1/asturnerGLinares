# Descarga de la obra de García Linares
El padrón de García Linares puede encontrarse segmentada en varios tomos de la revista *Hidalguía:  La  Revista de Genealogía,  Nobleza  y  Armas*. Puede descargarse en las siguientes enlaces:
- [Parte 1](https://www.edicioneshidalguia.es/?product=revista-no-240-8-linajes-asturianos-padrones-del-concejo-de-allande-de-1698-y-1773)
- [Parte 2](https://www.edicioneshidalguia.es/?product=revista-no-242-8-linajes-asturianos-padrones-del-concejo-de-allande-de-1698-y-1773)
- [Parte 3](https://www.edicioneshidalguia.es/?product=revista-no-243-4-linajes-asturianos-padrones-del-concejo-de-allande-de-1698-y-1773)
- [Parte 4](https://www.edicioneshidalguia.es/?product=revista-no-246-5-linajes-asturianos-padrones-del-concejo-de-allande-de-1698-y-1773)
- [Parte 5](https://www.edicioneshidalguia.es/?product=revista-no-248-7-linajes-asturianos-padrones-del-concejo-de-allande-de-1698-y-1773)

## Procesado antes del OCR
La técnica del OCR es sensible en un sentido negativo a la presencia de figuras en el texto o líneas extrañas al texto por causa del escaneo. Por ello, vamos a realizar una tarea de recorte de los archivos PDF para eliminar la figura del escudo que aparece en cada página y algunas líneas de escaneo. Usaremos el software *PDF Arranger* que puede descargarse para Windows o Linux [aquí](https://github.com/pdfarranger/pdfarranger). En realidad, puede utilizar el programa de edición de pdf que más le guste, lo importante es que realice las siguientes tareas:
- Eliminar páginas.
- Poder recortar por separado cada uno de los márgenes del documento.

Ahora describimos el procedimiento paso a paso

### Eliminar páginas
De la *Parte 1* podemos eliminar las primeras 5 páginas. Por favor, no eliminar la sexta página porque contiene información del censo. De la *Parte 5* eliminaremos las últimas 15 páginas pues contienen las tablas del resumen estadístico del censo, pero no entradas del padrón en sí mismas. Además, el OCR no podría procesarlas con la misma configuración usada para procesar el resto del documento.

### Recortar los márgenes
Véase que todas la páginas tienen la figura de un escudo en la parte superior, también encontramos el encabezado, el número de página e incluso una línea a la derecha de algunas hojas debido a artefactos del escaneo. Todos estos elementos generan una detección incorrecta de algunos caracteres en el OCR. Podemos eliminar todos estos elementos de un sólo golpe recortando los márgenes de todas las hojas. Se hace así:

1. Se selecciona o bien todas las páginas que tienen el escudo a la izquierda o bien todas aquellas que tienen el escudo a la derecha.
2. Hacemos clic en *Editar* -> *Recortar*. Entonces se abre una ventana que dice *Recortar las páginas seleccionadas* y presenta casillas para decirle al programa que porcentaje recortar de cada margen. Tenga en cuenta que puede visualizar mejor todas las páginas del documento haciendo pequeño el zoom con la rueda del ratón. Además, puede ver el detalle de cada hoja haciendo zoom, también con la rueda del ratón.
3. Para la mayoría de las páginas, a mí me ha servido la siguiente configuración:
    - Para páginas con el escudo a la izquierda: 19% arriba, 18,5% abajo, 17% derecha, 23% izquierda.
    - Para páginas con el escudo a la derecha: 19% arriba, 18,5% abajo, 25% derecha, 17% izquierda.
4. No olvide revisar el detalle de cada hoja haciendo zoom. Lo ideal es que no quede rastro de los elementos indeseados nombrados, pero tampoco se debe recortar palabras o letras del texto de interés.

### Nota
Si como procesador de OCR va a usar VietOCR, el que uso yo, entonces no junte las partes del censo. Esto puede producirle un error de falta de espacio de memoria al procesar el documento al completo por el OCR.

## Procesamiento de OCR: sofware VietOCR
Se ha escogido VietOCR para procesar el PDF y obtener un fichero de texto. Puede descargarse y aprender a usarse [aquí](http://vietocr.sourceforge.net/). Para ejecutar VietOCR es imprescindible instalar Tesseract OCR: si su sistema operativo es Debian/Ubuntu, basta ejecutar `sudo apt-get install tesseract-ocr  tesseract-ocr-spa`, de lo contario, puede consultar la documentación de Tesseract OCR [aquí](https://tesseract-ocr.github.io/). La documentación del software está en inglés, pero el programa soporta el idioma español, tanto para la interfaz gráfica, como para la detección OCR. Tenga en cuenta que para usar un modelo entrenado de reconocimiento de español, necesita haberlo instalado en Tesseract OCR: en Debian/Ubuntu, se instala con el paquete `tesseract-ocr-spa`, para otro sistema operativo, se le dirige a la documentación. Ahora, vamos a detallar los pasos del procedimiento.

### Ejecutar OCR y cargar el archivo
Debemos comenzar ejecutando el programa: en Ubuntu yo abro en la terminal la carpeta de descarga del programa y ejecuto `sudo java -jar VietOCR.jar`. Una vez abiero, la configuración que me ofrece los mejores resultados es:
- Idioma OCR: Spanish.
- Configuración -> Modo de segmentación de página -> 4 - Assume a single column of text of variable sizes.
- Configuración -> Modo de motor OCR -> 2 - Legacy + LSTM engines.

Ahora cargamos una de las partes del censo con las opciones Archivo -> Abrir. Esta opción puede tardar un par de minutos.

### Ejecutar el OCR
invitamos al usuario a descubrir el comportamiento de todos los botones de la barra de herramientas. Detallamos el uso de algunas:
- Puntos suspensivos arriba a la izquierda: abre el navegador de las páginas del pdf.
- Lupa con un cuadrado en el interior: aleja el zoom para ver la imagen de una página al completo.
- Folio con las palabras "OCR": ejecuta el procesado OCR de la página que se visualiza en ese momento.
- Goma de borrar: borra todo el texto que se ha procesado con OCR hasta el momento.
- Binoculares: herramienta buscar y reemplazar.
- Ruedas dentadas: herramienta de postprocesado. Debe configurarse en Configuración -> Opciones.
- Marca de impresión junto a una x roja: elimina los saltos de línea. Así recuperaremos los párrafos íntegros, sin saltos de línea.

Comenzamos ejecutando el OCR en una página para verificar que los resultados son aceptables (botón con forma de folio). Borramos todo con la goma de borrar. Ahora hacemos clic en Comando -> OCR para todas las páginas. Una vez terminado, usamos la herramienta de eliminar saltos de línea (botón marca de impresión con una cruz). Ahora, hacemos uso de la herramienta de buscar y reemplazar (binoculares). En buscar escribimos un guion (-) y en reemplazar no escribimos nada. Hacemos clic en Reemplazar todo. Si lo hemos hecho bien, deben haberse encontrado varias coicidencias. Finalmente, usamos la herramienta de postprocesado (ruedas dentadas). Entonces ya tendremos todos los párrafos reunificados y sin palabras cortadas con guiones. Ahora guardamos el texto en un documento TXT.


### Reunificar todos los archivos de texto
Una vez repetido el proceso para todas las partes del censo, reunificamos todos los archivos de texto en uno sólo. Ya estamos en disposición de usar Spacy y Stanza para extraer la información del censo en TXT.
