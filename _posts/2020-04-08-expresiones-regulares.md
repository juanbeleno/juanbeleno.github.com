---
layout: post
title: "Expresiones regulares"
author: "Juan Beleño"
categories: journal
tags: [nlp,spanish]
---

Las expresiones regulares (ER) son un lenguaje para la búsqueda en texto. Una expresión regular nos permite buscar los textos que cumplen unos patrones en un documento o en un conjunto de documentos. Las expresiones regulares distingue mayúsculas y minúsculas.

Puedes probar las expresiones regulares explicadas en esta publicación usando tu lenguaje de programación o herramienta de preferencia. Existen también algunas herramientas online como [esta](https://regexr.com/){:target="_blank"} que pueden ser de ayuda (aunque está en inglés).


## Expresiones regulares básicas

En la columna de ejemplos, se listan uno o más ejemplos y se subraya la parte del texto que cumple con los patrones de la expresión regular. 

|  Tipo  |      Expresión Regular      |  Ejemplos |
|---------------------|:---------------:|----------:|
| Secuencia de caracteres:  uno o más caracteres  |  /Alcohol/ | 1) ¡Por el <ins>Alcohol</ins>! la causa y la solución de todos los problemas de la vida - Homer Simpson |
| Disyunción: Cualquier carácter o ER dentro de los corchetes |  /[Aa]lcohol/ | 1) Estoy un poco <ins>alcohol</ins>izado |
| Rango: Cualquier carácter que esté entre los 2 designados. Funciona para letras (mayúsculas y minúsculas) y números |  /[b-d]eso/ | 1) <ins>deso</ins>rdenes <br/> 2) Dame un <ins>beso</ins> <br/> 3) Me retiraron un abs<ins>ceso</ins> | 
| Negación: cualquier carácter o ER que no esté dentro de los corchetes |  /[^A-Z]/ | 1) D<ins>e</ins>scanso |
| Caracteres opcionales: Es opcional el carácter o ER antes de ? |  /bl?anca/ | 1) <ins>blanca</ins> <br/> 2) Transacción <ins>banca</ins>ria |
| Kleene *: Cero o más apariciones del carácter o ER antes de * |  /[0-9]*/ | 1) <ins>c</ins>asa <br/> 2) <ins>1800</ins> COP |
| Kleene +: Una o más apariciones del carácter o ER antes de + |  /[0-9]*/ | 1) <ins>1800</ins> COP |
| Caráter comodín: . puede ser cualquier carácter |  /1.000/ | 1) <ins>18000</ins> COP <br/> 2) <ins>13000</ins> BRL|
| Ancla - Comienzo del texto: ^ por fuera de corchetes |  /^El/ | 1) <ins>El</ins> avión|
| Ancla - Fin del texto: $ por fuera de corchetes |  /fin.$/ | 1) Y murió, <ins>fin.</ins>|
| Límite de palabra. Una **palabra** es una secuencia de dígitos, letras o raya al piso. |  /\b5000\b/ | 1) $<ins>5000</ins> pesos <br/> 2) Murieron más de <ins>5000</ins> personas|
| Operador de disyunción |  /perro\|gato/ | 1) Me gustan los <ins>gato</ins>s <br/> 2) Los <ins>perro</ins>s son fieles|
| Operador de paréntesis |  /co(vid\|ronavirus\|rona virus\|v-2)/ | 1) Existe una pandemia de <ins>coronavirus</ins> en Brasil <br/> 2) El virus sars-cov-2 produce el <ins>covid-19</ins> |

En general, las expresiones regulares funcionan como [algoritmos golosos](https://es.wikipedia.org/wiki/Algoritmo_voraz){:target="_blank"}, es decir, toman la cadena de caracteres más grande que cumpla con los patrones especificados. Eso lo podemos notar en el ejemplo 2 de Operador de paréntesis, donde hay 2 textos que cumplen con la expresión regular: cov-2 y covid-19; Sin embargo, solo se resalta uno: covid-19. Existen algunas excepciones a esta regla como *? o +?

## Operadores de conteo en expresiones regulares

|  Operador |  Descripción |
|-----------|:------------:|
| *         |  Cero o más apariciones del anterior carácter o ER |
| +         |  Una o más apariciones del anterior carácter o ER |
| ?         |  Exactamente cero o una aparición del anterior carácter o ER |
| {n}       |  Exactamente n apariciones del anterior carácter o ER |
| {n,m}     |  Desde n hasta m apariciones del anterior carácter o ER |
| {n,}      |  Al menos n apariciones del anterior carácter o ER |
| {,m}      |  Hasta m apariciones del anterior carácter o ER |

## Alias para conjuntos comunes de caracteres

|  Alias |  Expresión Regular | Descripción  |
|--------|:------------------:|-------------:|
| \d     |  [0-9]             | Cualquier dígito |
| \D     |  [^0-9]            | Cualquier carácter, exceptuando los dígitos |
| \w     |  [a-ZA-Z0-9_]      | Cualquier alfanumérico ó guión bajo |
| \W     |  [^\w]             | Cualquier carácter, exceptuando los alfanuméricos y guión bajo|
| \s     |  [ \r\t\n\f]       | Espacio en blanco o tabulación |
| \S     |  [^\s]       | Cualquier carácter, exceptuando el espacio en blanco y tabulación |


## Caracteres especiales

|  Expresion Regular |  Descripción |
|-----------|:------------:|
| \*        |  Asterisco |
| \.        |  Punto |
| \?        |  Signo de interrogación |
| \n        |  Salto de línea |
| \t        |  Tabulación |


## Jerarquía de precedencia de operadores

En algunos casos, puede parecer que las expresiones regulares son algo ambiguas. Por ejemplo, si queremos encontrar textos donde el número 10 se repite varias veces (101010)
¿Usariamos la expresión regular `/10+/` ó `/(10)+/`? Respuesta: `/(10)+/`. Por lo tanto, a continuación muestro la jerarquía de precedencia de operadores (lo que se tiene en cuenta primero al evaluar una expresión regular):

1) Paréntesis: () <br/>
2) Contadores: + * ? {} <br/>
3) Secuencias de caracteres y anclas: texto ^ $ <br/>
4) Disyunciones: | <br/>

## Enfoque iterativo para la construcción de expresiones regulares

Supón que tenemos un conjunto de textos sobre computadores y queremos los computadores con discos duros de 500GB o más. Este tipo de problema se aborda de manera iterativa. Por ejemplo,

1) Comenzamos identificando los números al lado de un GB: `/[0-9]+GB/` <br/>
2) Aveces los textos pueden contener un espacio separando el número del GB. Por ejemplo, `650 GB`. Para solucionar ese problema usamos `/[0-9]+ ?GB/` <br/>
3) Como queremos 500GB o más entonces usamos `/([5-9][0-9]{2}|[1-9][0-9]{3,}) ?GB/` <br/>
3.1) La primera parte (`[5-9][0-9]{2}`) se encarga de encontrar los números entre 500 y 999 <br/>
3.2) La segunda parte (`[1-9][0-9]{3,}`) se encarga de encontrar los números mayores a 999

Si queremos refinar aún más nuestra expresión regular podríamos tener en cuenta que la palabra `gigabytes` puede ser usada en vez de `GB`. Podríamos tener en cuenta minúsculas. Podríamos tener en cuenta de después de 1023 GB comienzan los `terabytes`. En fin, aún hay campo para mejorar la expresión regular que tenemos. Al ir iterando podríamos obtener una expresión regular con la que abarcaríamos la gran mayoría de casos.

Este enfoque iterativo se usa para minimizar los falsos positivos (cadenas de texto que identificamos, pero que son equivocadas) y minimizar los falsos negativos (cadenas de texto que no identificamos, pero que deberíamos).

## Otros usos de las expresiones regulares

Las expresiones regulares pueden ser usadas para la substitución usando la siguiente sintaxis: `s/regex/patrón`. Por ejemplo, `s/substitución/sustitución/` para reemplazar todas las apariciones de `substitución` por `sustitución`.

Grupo de captura: una forma de almacenar patrones en memoria para su uso en la misma expresión regular. Cualquier patrón entre paréntesis puede ser almacenado en memoria. Por ejemplo, la expresión regular `/más sabe el (.*) por (.*) que por \1 y se lo digo yo que soy \2/` identifica la cadena de texto `más sabe el diablo por viejo que por diablo y se lo digo yo que soy viejo`, pero no la cadena `más sabe el diablo por viejo que por diablo y se lo digo yo que soy diablo`. Para especificar que un patrón dentro de paréntesis no debe ser un grupo de captura se coloca `?:` después de abrir paréntesis. Por ejemplo, la expresión regular `/(?:algunos|unos pocos) (perros|gatos) odian a otros \1/` identifica la cadena de texto `/algunos perros odian a otros perros/`.

Los patrones de ancho cero se usan para parar la búsqueda apenas se cumple el patrón y engloban los siguientes operadores:

1) Anticipación positiva `(?= patrón)` es verdadero si se cumple el patrón y para la búsqueda <br/>
2) Anticipación negativa `(?! patrón)` es verdadero si NO se cumple el patrón y para la búsqueda <br/>

Supón que queremos identificar, al comienzo de una línea, cualquier palabra que no comience con Volcán. Usamos la siguiente expresión regular: `/^(?!Volcán)[/w]+/` . Esta expresión regular para, si encuentra una palabra diferente de `Volcán` al comienzo de una línea.

## Referencias

Esta publicación es un resumen de la sección 2.1. del libro Speech and Language Processing. Daniel Jurafsky & James H. Martin. Draft of October 2, 2019. Disponible [aquí](https://web.stanford.edu/~jurafsky/slp3/){:target="_blank"}
