---
layout: post
title: "Expresiones regulares"
author: "Juan Beleño"
categories: journal
tags: [nlp,spanish]
---

Las expresiones regulares (ER) son un lenguaje para la búsqueda en texto. Una expresión regular nos permite buscar los textos que cumplen unos patrones en un documento o en un conjunto de documentos. Las expresiones regulares distingue mayúsculas y minúsculas. Una **palabra** es una secuencia de dígitos, letras o raya al piso.


## Expresiones regulares básicas

En la columna de ejemplos, se listan uno o más ejemplos y se subraya la parte del texto que cumple con los patrones especificados en la expresión regular. 

|  Tipo  |      Expresión Regular      |  Ejemplos |
|---------------------|:---------------:|----------:|
| Secuencia de caracteres:  uno o más caracteres  |  /Alcohol/ | 1) ¡Por el <ins>Alcohol</ins>! la causa y la solución de todos los problemas de la vida... |
| Disyunción: Cualquier carácter o ER dentro de los corchetes |  /[Aa]lcohol/ | 1) Estoy un poco <ins>alcohol</ins>izado |
| Rango: Cualquier carácter que esté entre los 2 designados. Funciona para letras (mayúsculas y minúsculas) y números |  /[b-d]eso/ | 1) <ins>deso</ins>rdenes <br/> 2) Dame un <ins>beso</ins> <br/> 3) Me retiraron un abs<ins>ceso</ins> | 
| Negación: cualquier carácter o ER que no esté dentro de los corchetes |  /[^A-Z]/ | 1) D<ins>e</ins>scanso |
| Caracteres opcionales: Es opcional el carácter o ER antes de ? |  /bl?anca/ | 1) <ins>blanca</ins> <br/> 2) Transacción <ins>banca</ins>ria |
| Kleene *: Cero o más apariciones del carácter o ER antes de * |  /[0-9]*/ | 1) <ins>c</ins>asa <br/> 2) <ins>1800</ins> COP |
| Kleene +: Una o más apariciones del carácter o ER antes de * |  /[0-9]*/ | 1) <ins>1800</ins> COP |
| Caráter comodín: . puede ser cualquier carácter |  /1.000/ | 1) <ins>18000</ins> COP <br/> 2) <ins>13000</ins> BRL|
| Ancla - Comienzo del texto: ^ por fuera de corchetes |  /^El/ | 1) <ins>El</ins> avión|
| Ancla - Fin del texto: $ por fuera de corchetes |  /fin.$/ | 1) Y murió, <ins>fin.</ins>|
| Límite de palabra (ver definición de palabra arriba) |  /\b5000\b/ | 1) $<ins>5000</ins> pesos <br/> 2) Murieron más de <ins>5000</ins> personas|
| Operador de disyunción |  /perro\|gato/ | 1) Me gustan los <ins>gato</ins>s <br/> 2) Los <ins>perro</ins>s son fieles|
| Operador de paréntesis |  /co(vid\|ronavirus\|rona virus\|v-sars-2)/ | 1) Existe una pandemia de <ins>coronavirus</ins> en Brasil <br/> 2) El virus <ins>cov-sars-2</ins> produce el covid-19 |

En general, las expresiones regulares funcionan como [algoritmos golosos](https://es.wikipedia.org/wiki/Algoritmo_voraz){:target="_blank"}, es decir, toman la cadena de caracteres más grande que cumpla con los patrones especificados. Eso lo podemos notar en el ejemplo 2 de Operador de paréntesis, donde hay 2 textos que cumplen con la expresión regular: cov-sars-2 y covid-19; Sin embargo, solo se resalta uno: cov-sars-2. Existen algunas excepciones a esta regla como *? o +?
 

## Jerarquía de precedencia de operadores

En algunos casos, puede parece que las expresiones regulares son algo ambiguas. Por ejemplo, si queremos encontrar textos donde el número 10 se repite varias veces (101010)
¿Usariamos la expresión regular `/10+/` ó `/(10)+/`? Respuesta: `/(10)+/`. Por lo tanto, a continuación muestro la jerarquía de precedencia (lo que se tiene en cuenta primero al evaluar una expresión regular):

1) Paréntesis: () <br/>
2) Contadores: + * ? {} <br/>
3) Secuencias de caracteres y anclas: texto ^ $ <br/>
4) Disyunciones: | <br/>

## Referencias

Esta publicación es un resumen de la sección 2.1. del libro Speech and Language Processing. Daniel Jurafsky & James H. Martin. Copyright c 2019. All
rights reserved. Draft of October 2, 2019. Disponible [aquí](https://web.stanford.edu/~jurafsky/slp3/){:target="_blank"}
