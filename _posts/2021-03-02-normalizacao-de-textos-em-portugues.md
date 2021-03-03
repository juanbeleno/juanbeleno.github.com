---
layout: post
title: "Normalização de textos em português"
author: "Juan Beleño"
categories: journal
tags: [regex,spanish]
---

A função aqui apresentada foi inspirada na normalização usada por vários participantes do [Mercado Livre Challenge 2019](https://ml-challenge.mercadolibre.com/){:target="_blank"}. Algumas das normalizações nas que eu me baseei foram [esta](https://github.com/pablozivic/meli-challenge-2019/blob/master/preprocessing.py#L38-L52){:target="_blank"}, [esta](https://github.com/pablozivic/meli-challenge-2019-multilingual-classifier/blob/master/multilingual_title_classifier/src/vocab.py#L15-L58){:target="_blank"} e [esta](https://github.com/pablozivic/mercadolibre/blob/master/MeLi_BaseGen/MeLi_BaseGen_V2.ipynb){:target="_blank"}

```python
from unicodedata import normalize
import re

def normalizar_texto(texto):
    # Garante que o parâmetro é texto
    texto_normalizado = str(texto)
    # Transforma o texto em minusculas
    texto_normalizado = texto_normalizado.lower()
    # Tira os acentos das palavras incluindo a ç
    texto_normalizado = normalize(
        'NFKD', texto_normalizado).encode('ascii', errors='ignore').decode('utf-8')
    # Sustui os números por zeros
    texto_normalizado = re.sub(r'\d', '0', texto_normalizado)
    # Sustitui os pontos por vírgulas (eu faço isto por causa dos números decimais)
    texto_normalizado = re.sub(r'\.', ',', texto_normalizado)
    # Sustitui as vírgulas que não estejam entre números
    texto_normalizado = re.sub(r'(?<!\d),(?!\d)', ' ', texto_normalizado)
    # Tira os carateres que não sejam letras, números ou vírgulas. Por exemplo, sinais de pontuação diferentes da vírgula
    texto_normalizado = re.sub(r'[^a-z0,]', ' ', texto_normalizado)
    # Tira os espaços a mais no meio das palavras
    texto_normalizado = re.sub(r'[\s\n\r]+', ' ', texto_normalizado)
    # Tira os espaços no começo e no fim do texto
    texto_normalizado = texto_normalizado.strip(' \n\t\r')
    return texto_normalizado
```

Este código usa um monte de expressões regulares. Por isto, recomendo dar uma olhada no meu  [post sobre expressões regulares](https://juanbeleno.github.io/journal/expressoes-regulares.html){:target="_blank"}

Usando a função anterior o texto

```
     Olá, meu nome é Juan Beleño, tenho 28 anos e meço 1.76cm    - fiz computação na UNICAMP.
    é nois galera!
```

vira o seguinte

```
ola meu nome e juan beleno tenho 00 anos e meco 0,00cm fiz computacao na unicamp e nois galera
```

**Bilhete:** Eu não curto de tirar as *stop words* do texto porque acho que talvez isso [pode dar prejuízio na hora de usar para classificação de textos](https://twitter.com/lousylinguist/status/1068285983483822085){:target="_blank"}
