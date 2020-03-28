---
layout: post
title: "Leer archivos desde una URL con pandas"
author: "Juan Beleño"
categories: journal
tags: [pandas,spanish]
---

En el siguiente código, creamos un DataFrame a partir de un archivo en la nube usando la URL.

```python
import pandas as pd

url_iris = 'https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data'
nombres_columnas = ['largo_sepalo', 'ancho_sepalo', 'largo_petalo', 'ancho_petalo', 'tipo_flor']
datos = pd.read_csv(url_iris, sep=',', header=None, names=nombres_columnas)
```

La URL es de un conjunto de datos llamado [iris dataset](https://archive.ics.uci.edu/ml/datasets/iris){:target="_blank"} que contiene datos de 3 tipos de flores, donde cada registro tiene el largo del [sépalo](https://es.wikipedia.org/wiki/S%C3%A9palo){:target="_blank"}, el ancho del sépalo, el largo del [pétalo](https://es.wikipedia.org/wiki/P%C3%A9talo){:target="_blank"}, el acho del pétalo y el nombre del tipo de flor. A continuación se muestra el contenido de las primeras 5 líneas del archivo:

```
5.1,3.5,1.4,0.2,Iris-setosa
4.9,3.0,1.4,0.2,Iris-setosa
4.7,3.2,1.3,0.2,Iris-setosa
4.6,3.1,1.5,0.2,Iris-setosa
5.0,3.6,1.4,0.2,Iris-setosa
```

## Parametros
Hay 4 parametros usados en el ejemplo:

**filepath_or_buffer:** Es la URL de los datos.

**sep:** Es el separador entre un dato y el otro. Valores comunes para este campo son la coma `,`, el punto y coma `;` y el tab `\t`.

**header:** Es para saber si el archivo tiene los nombres de las columnas al comienzo del archivo o no. Tiene un valor de `None` si el archivo no tiene el nombre de las columnas en la primera línea del archivo. Tiene un valor de `0` si tiene el nombre de las columnas en la primera línea. La verdad este valor indica en que línea están los nombres de las columnas, entonces si por alguna extraña razón el nombre de las columnas están en la línea 12, pues ponemos `12`.

**names:** Es el nombre de las columnas. Solo se usa cuando `header=None` porque de otra manera ya tenemos el nombre de las columnas.

Generalmente, antes de escribir código, abro el archivo en texto plano para saber que separador usa, si tiene el nombre de las columnas al inicio del archivo o no, etc. Si el archivo es muy pesado, uso el comando `top` en Linux o Mac.

Puedes consultar la [documentación oficial](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html){:target="_blank"} para saber que otros parámetros te pueden ser útiles.

**Nota:** Estoy usando `pandas 1.0.3` y `pyhton 3.7`
