---
layout: post
title: "Leer archivos JSON con pandas"
author: "Juan Beleño"
categories: journal
tags: [pandas,spanish]
---

En el siguiente código, creamos un DataFrame a partir de un archivo de [JSON](https://www.json.org/json-es.html){:target="_blank"}. Un DataFrame es una estructura de datos en pandas que se representa usando filas y columnas (parecido a Excel).

```python
import pandas as pd

ruta_archivo_json = '/Users/Juan/Downloads/datos_ejemplo_1.json'
datos = pd.read_json(ruta_archivo_json, orient='records')
```

El archivo JSON tiene datos inventados por mi y está disponible [aquí](../assets/others/leer-un-archivo-json-en-pandas/datos_ejemplo_1.json){:target="_blank"}. A continuación se muestra el contenido del archivo JSON.

```
[
    {
        "nombre": "Juan",
        "altura": 1.76,
        "peso": 65
    },
    {
        "nombre": "Lina",
        "altura": 1.65,
        "peso": 68
    },
    {
        "nombre": "Katherine",
        "altura": 1.68,
        "peso": 67
    }
]
```

## Parametros
Hay 2 parametros usados en el ejemplo:

**path_or_buf:** Es la ruta absoluta del archivo JSON en nuestro sistema operativo o la URL del archivo JSON.

**orient:** Es el parametro que indica como está estructurado JSON para poderlo convertir en DataFrame. Usé el valor `records` porque es el que comunmente encuentro cuando trabajo con archivos JSON, pero hay más.

Puedes consultar la [documentación oficial](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_json.html){:target="_blank"} para saber que otros parámetros te pueden ser útiles y que valores puede tomar el parametro `orient`.

Generalmente, antes de escribir código, abro el archivo JSON para saber la estructura del archivo y saber que parametro `orient` pasar al método `read_json`. Sin embargo, en muchas ocaciones encuentro un formato JSON que no es compatible con el parametro `orient`.

## ¿Qué hacer cuando el formato del archivo JSON no es compatible con el parametro `orient`?

Fácil, leer el archivo JSON convertirlo a un diccionario o lista y trabajar con tipos de datos de python. Encontrar o construir una lista de diccionarios y pasarla como parametro a `pd.DataFrame()`. Por ejemplo, considere el siguiente archivo de JSON disponible [aquí](../assets/others/leer-un-archivo-json-en-pandas/datos_ejemplo_2.json){:target="_blank"}

```
{
    "datos": [
        {
            "nombre": "Juan",
            "altura": 1.76,
            "peso": 65
        },
        {
            "nombre": "Lina",
            "altura": 1.65,
            "peso": 68
        },
        {
            "nombre": "Katherine",
            "altura": 1.68,
            "peso": 67
        }
    ]
}
```

El código que me permite convertir ese archivo JSON en un DataFrame es el siguiente:

```python
import json
import pandas as pd

ruta_archivo_json = '/Users/Juan/Downloads/datos_ejemplo_2.json'
with open(ruta_archivo_json) as archivo_json:
    datos_json = json.load(archivo_json)
lista_de_diccionarios = datos_json['datos']
datos = pd.DataFrame(lista_de_diccionarios)
```


**Nota:** Estoy usando `pandas 1.0.3`, `pyhton 3.7` y un `Mackbook Pro 2015`, entonces posiblemente te toque cambiar las rutas absolutas de los archivos según tu sistema operativo.
