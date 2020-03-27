---
layout: post
title: "Leer archivos de Excel con pandas"
author: "Juan Beleño"
categories: journal
tags: [pandas,spanish]
---

En el siguiente código, creamos un DataFrame a partir de un archivo de Excel. Un DataFrame es una estructura de datos en pandas que se representa usando filas y columnas (parecido a Excel).

```python
import pandas as pd

ruta_archivo_iris = '/Users/Juan/Downloads/iris.xlsx'
nombres_columnas = ['largo_sepalo', 'ancho_sepalo', 'largo_petalo', 'ancho_petalo', 'tipo_flor']
datos = pd.read_excel(ruta_archivo_iris, header=None, names=nombres_columnas)
```

Para este ejemplo, usé un conjunto de datos llamado [iris dataset](https://archive.ics.uci.edu/ml/datasets/iris){:target="_blank"} que contiene datos de 3 tipos de flores, donde cada registro tiene el largo del [sépalo](https://es.wikipedia.org/wiki/S%C3%A9palo){:target="_blank"}, el ancho del sépalo, el largo del [pétalo](https://es.wikipedia.org/wiki/P%C3%A9talo){:target="_blank"}, el acho del pétalo y el nombre del tipo de flor. El archivo está disponible [aquí](../assets/others/leer-un-archivo-de-excel-en-pandas/iris.xlsx){:target="_blank"} y a continuación muestro el contenido de las 5 primeras líneas:

| 5.1 | 3.5 | 1.4 | 0.2 | Iris-setosa |
| 4.9 | 3.0 | 1.4 | 0.2 | Iris-setosa |
| 4.7 | 3.2 | 1.3 | 0.2 | Iris-setosa |
| 4.6 | 3.1 | 1.5 | 0.2 | Iris-setosa |
| 5.0 | 3.6 | 1.4 | 0.2 | Iris-setosa |


## Parametros
Hay 3 parametros usados en el ejemplo:

**filepath_or_buffer:** Es la ruta absoluta del archivo en nuestro sistema operativo.

**header:** Es para saber si el archivo tiene los nombres de las columnas al comienzo del archivo o no. Tiene un valor de `None` si el archivo no tiene el nombre de las columnas en la primera línea del archivo. Tiene un valor de `0` si tiene el nombre de las columnas en la primera línea. La verdad este valor indica en que línea están los nombres de las columnas, entonces si por alguna extraña razón el nombre de las columnas están en la línea 12, pues ponemos `12`.

**names:** Es el nombre de las columnas. Solo se usa cuando `header=None` porque de otra manera ya tenemos el nombre de las columnas.

Generalmente, antes de escribir código, abro el archivo de Excel para saber si tiene el nombre de las columnas al inicio del archivo o no.

Puedes consultar la [documentación oficial](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html){:target="_blank"} para saber que otros parámetros te pueden ser útiles.

**Nota:** Estoy usando `pandas 1.0.3`, `pyhton 3.7` y un `Mackbook Pro 2015`, entonces posiblemente te toque cambiar las rutas absolutas de los archivos según tu sistema operativo.
