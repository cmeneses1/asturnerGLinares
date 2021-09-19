# asturnerGLinares
Código para la resolución del problema NER y extracción automática de información en un censo de linajes asturianos. Para ejecutar este código es necesario tener instaladas las librerías siguientes:
- Python 3
- _Jupyter_.
- [SpaCy](https://spacy.io/)
- [Stanza](https://stanfordnlp.github.io/stanza/#getting-started)
- [spacy-stanza](https://spacy.io/universe/project/spacy-stanza)

Para la aplicación del algoritmo de k-Medias es necesario:
- R
- Yo uso `RStudio` para compilar el *notebook* de *R*.

## Introducción
Este código diferentes *notebooks* de *Python 3* y de *R* que pretenden extraer información del padrón de linajes asturiano mecanografiado y publicado por Antonio García Linares y que data de los años 1698 y 1773. Se ha usado *notebooks* con el fin de resolver el problema de forma escalonada, interactiva y también pedagógica, porque puede servir de práctica para aprender métodos de extracción de información de otros documentos históricos. Ahora bien, por motivos del Copyright no podemos colgar en formato TXT, lista para ser procesada, la obra de García Linares; por lo tanto, se muestran los pasos a seguir para descargarla y procesarla mediante un OCR. La tabla de datos total tampoco, por constituir una transformación de una obra protegida. Ahora bien, todos los pasos están detallados para que el usario pueda obtener sus propios resultados y emprender su investigación indivualmente.

No importa si no dispone del censo al completo en TXT, todo el código en este repositorio puede ejecutarse con un pequeño extracto denominado `PadronOCRPrueba.txt`. No obstante, en este repositorio se detalla cómo descargar y procesar el censo en PDF al completo. El código funciona en ambos casos.

## Descarga de la obra de García Linares
Si no se dispone de la obra de García Linares, *Linajes Asturianos. Padrones del Concejo de Allande*, en TXT puede seguir los pasos detallados en [README_descarga.md](https://github.com/cmeneses1/asturnerGLinares/blob/main/README_descarga.md) para obtener una copia PDF para uso personal y transformarla en un documento TXT.

## Puesta a punto del TXT
Una vez se dispone de la obra en TXT, deben hacerse las siguientes modificaciones para que el código de esta repositorio funcione correctamente:
- Eliminar el preámbulo de la obra. El documento a procesar por SpaCy debería comenzar a partir de las líneas "PADRONES" y "EMPADRONADORES GENERALES". Por otro lado, no debe aparece  el procesado OCR de las últimas 15 páginas, que hacen referencia a tablas estadísticas.
- Buscar cada uno de los títulos del censo que hacen referencia a parroquias y rescribirlos, de ser necesario, en mayúsculas. Por ejemplo, "PARROQUIA DE ARANIEGO (ParaJas)" se cambia a "PARROQUIA DE ARANIEGO (PARAJAS)". Además, hay que dejarlos en líneas de texto aisladas, es decir, al menos un salto de línea antes y otro después de la frase.
- Buscar cada uno de los títulos del censo que hacen referencia a localidades y reescribirlos, de ser necesario, en mayúsculas. Por ejmplo, "Boxo" cambiarlo a "BOXO". Dejarlos también en líneas aisladas.
- Buscar todas las frases "Empadronadores locales:" y "Empadronadores Locales:" y dejarlas en líneas aisladas.

## Procesamiento del TXT con *spacy-stanza*
Ejecute el código del *notebook* `A_Procesar_Guardar.ipynb` para procesar el texto de prueba `PadronOCRPrueba.txt` con *spacy-stanza*. Si dispone del censo al completo en TXT cambie la ruta `direcciontxt`.  Este código busca todas las entidades de personas y guarda el documento procesado en un binario para su posterior estudio.

Ahora, está en disposición de ejecutar el *notebook* `B_Añadir_Etiquetas.ipynb`. Este código automatiza la búsqueda y extracción de los datos. De nuevo, si dispone del censo al completo en TXT cambie la ruta `direcciontxt`.

## Aplicación de k-Medias con R
Una vez se ha logrado la extracción de los datos, se dispone de una tabla con datos como la de este repositorio `GeolocLocalidadesPrueba.tsv`. Ésta contiene los campos
- Nombre.
- Año.
- Parroquia.
- Geolocalización de la parroquia (latitud y longitud).
- Localidad 1.
- Gelocalización de la localidad 1.
- Hasta otras dos localidades más (porque a veces los individuos del censo vienen agrupados en tres localidades al mismo tiempo, por ejemplo: "CABO, FURADA Y RUBIEIRO").

El *notebook* de *R*, `C_Aplicación_k-Medias_en_R.Rmd`, sirve para la aplicación del algoritmo de k-Medias aplicado a un apellido concreto. Ejecuta k-Medias para las entradas correspondiente a 1698 y las de 1773. Además, compara los *clusters* de uno y otro año. Finalmente, puede verse un resultado de la ejecución del cuaderno de *R* en el documento `R notebook.pdf` que se encuentra en la carpeta `Documentos`.

## *Notebooks* `Z_...`
Los *notebooks* cuyo título empiezan por *Z_* no están pensados para ejecutarse independientemente como `A_...`, `B_...` y `C_...`. Por el contrario, están pensados para albergar funciones y variables de ayuda necesarias para el uso de `A_Procesar_Guardar.ipynb` y `B_Añadir_Etiquetas.ipynb`.

## Resultados
Si se ejecuta el código de este repositorio para todo el censo al completo, se puede obtener una lista de datos estructurados en formato TSV con más de 3600 entradas.
