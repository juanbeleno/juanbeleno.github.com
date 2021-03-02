---
layout: post
title: "Normalizar textos en español"
author: "Juan Beleño"
categories: journal
tags: [regex,spanish]
---

Esta función fue inspirada en la normalización usada por varios participantes del [Mercado Libre Challenge 2019](https://ml-challenge.mercadolibre.com/){:target="_blank"}. Algunas de las normalizaciones en las que me inspiré fueron [esta](https://github.com/pablozivic/meli-challenge-2019/blob/master/preprocessing.py#L38-L52){:target="_blank"}, [esta](https://github.com/pablozivic/meli-challenge-2019-multilingual-classifier/blob/master/multilingual_title_classifier/src/vocab.py#L15-L58){:target="_blank"} y [esta](https://github.com/pablozivic/mercadolibre/blob/master/MeLi_BaseGen/MeLi_BaseGen_V2.ipynb){:target="_blank"}

```python
from unicodedata import normalize
import re

def normalizar_texto(texto):
    # Garantiza que el parametro es texto
    texto_normalizado = str(texto)
    # Transforma el texto en minúsculas
    texto_normalizado = texto_normalizado.lower()
    # Quita las tildes a las palabras, la dieresis a la ü y la ~ a la ñ
    texto_normalizado = normalize(
        'NFKD', texto_normalizado).encode('ascii', errors='ignore').decode('utf-8')
    # Reemplaza los números por ceros
    texto_normalizado = re.sub(r'\d', '0', texto_normalizado)
    # Reemplaza puntos por comas (esto lo hago por los números decimales)
    texto_normalizado = re.sub(r'\.', ',', texto_normalizado)
    # Quita las comas que no estén entre números
    texto_normalizado = re.sub(r'(?<!\d),(?!\d)', ' ', texto_normalizado)
    # Quita los caracteres que no sean letras, números o comas, como signos de puntuación (diferentes de la coma)
    texto_normalizado = re.sub(r'[^a-z0,]', ' ', texto_normalizado)
    # Quita los espacios que sobren entre palabras
    texto_normalizado = re.sub(r'[\s\n\r]+', ' ', texto_normalizado)
    # Quita los espacios al comienzo y al final del texto
    texto_normalizado = texto_normalizado.strip(' \n\t\r')
    return texto_normalizado
```

Este código usa bastantes expresiones regulares, por lo que te recomiendo echarle una mirada a [mi post sobre expresiones regulares](https://juanbeleno.github.io/journal/expresiones-regulares.html){:target="_blank"}

Usando la función anterior el texto

```
     Hola, mi nombre es Juan Beleño, tengo 28 años y mido 1.76cm    - estudié en la UNICAMP.
```

se convertiría en

```
hola mi nombre es juan beleno tengo 00 anos y mido 0,00cm estudie en la unicamp
```
