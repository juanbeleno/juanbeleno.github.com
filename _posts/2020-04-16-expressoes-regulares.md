---
layout: post
title: "Expressões regulares"
author: "Juan Beleño"
categories: journal
tags: [nlp,spanish]
---

As expressões regulares (ER) são uma linguagem pra busca em texto. Uma expressão regular permite a busca de textos com padrões especificados num documento ou num conjunto de documentos. Ditas expressões regulares diferenciam maiúsculas de minúsculas.

Você pode testar as expressões regulares explicadas nesta publicação usando sua linguagem de programação ou ferramenta favorita. Também existem algumas ferramentas na internet tipo esta [daqui](https://regexr.com/){:target="_blank"} que podem ser utilizadas (embora essa ferramenta está em inglês).

## Expressões regulares básicas

Na coluna de exemplos, aparecem sublinhadas as partes do texto que tem os padrões da expressão regular.

|  Tipo  |      Expressão Regular      |  Exemplos |
|---------------------|:---------------:|----------:|
| Sequência de carateres:  um ou mais carateres  |  /álcool/ | 1) Ao <ins>álcool</ins>! A causa e a solução de todos os problemas da vida - Homer Simpson |
| Disjunção: Qualquer caracter ou ER dentro dos corchetes |  /[Bb]ar/ | 1) Estou num <ins>bar</ins>zinho |
| Intervalos: Qualquer caracter entre dois valores. Funciona para letras (maiúsculas e minúsculas) e para números |  /[b-d]ei/ | 1) Dois <ins>bei</ins>jos na bochecha é o comum no Rio. <br/> 2) A <ins>cei</ins>a de natal demorou pra caramba <br/> 3) Eu <ins>dei</ins>xo você brincar de bola, se fizer as tarefas da escola | 
| Negação: Qualquer caracter ou ER por fora dos que estão nos colchetes |  /[^A-Z]/ | 1) D<ins>e</ins>scanso |
| Caracteres opcionais: Um caracter ou ER é opcional se depois dele tem um ? |  /br?anco/ | 1) Meus sapatos são <ins>brancos</ins> <br/> 2) Tenho dois milhões no <ins>banco</ins> |
| Kleene *: zero ou mais ocorrências do caracter ou ER anterior a * |  /[0-9]*/ | 1) <ins>c</ins>asa <br/> 2) <ins>1800</ins> BRL |
| Kleene +: uma ou mais ocorrências do caracter ou ER anterior a + |  /[0-9]*/ | 1) <ins>1800</ins> USD |
| Caracter coringa: ponto (.) pode ser qualquer caracter |  /1.000/ | 1) <ins>18000</ins> COP <br/> 2) <ins>13000</ins> BRL|
| Âncora - Começo do texto: ^ por fora dos colchetes |  /^O/ | 1) <ins>O</ins> avión|
| Âncora - Fim do texto: $ por fora dos colchetes |  /fim.$/ | 1) E morreu, <ins>fim.</ins>|
| Límite da palavra. Uma **palavra** é uma sequência de dígitos, letras ou o caráter underline. |  /\b5000\b/ | 1) $<ins>5000</ins> reais <br/> 2) Morreram mais de <ins>5000</ins> pessoas|
| Operador de disjunção |  /cachorro\|gato/ | 1) Eu gosto dos <ins>gato</ins>s <br/> 2) Os <ins>cachorros</ins>s são bonitinhos|
| Operador de parênteses |  /co(vid\|ronavírus\|rona vírus\|v-2)/ | 1) Existe uma pandemia de <ins>coronavírus</ins> no Brasil <br/> 2) O vírus sars-cov-2 produz o <ins>covid-19</ins> |

Em geral, as expressões regulares funcionam como [algoritmos gulosos](https://pt.wikipedia.org/wiki/Algoritmo_guloso){:target="_blank"}, isso quer dizer que as expressões regulares pegam a maior cadeia de caracteres que tem os padrões especificados. A gente pode ver isso no exemplo 2 de Operador de parêntese, onde tem 2 textos que com o padrão da expressão regular: cov-2 y covid-19; No entanto, só aparece sublinhado um deles: covid-19. Há algumas exceções desta regla tipo *? o +?

## Operadores para contar em expressões regulares

|  Operador |  Descrição |
|-----------|:------------:|
| *         |  Zero ou mais ocorrências do caracter anteior ou ER anterior |
| +         |  Uma ou mais ocorrências do caracter anteior ou ER anterior |
| ?         |  Exatamente zero ou uma ocorrência do caracter anterior ou ER anterior |
| {n}       |  Exatamente n ocorrências do caracter anterior ou ER anterior |
| {n,m}     |  Desde n até m ocorrências do caracter anterior ou ER anterior |
| {n,}      |  Ao menos n ocorrências do caracter anterior ou ER anterior |
| {,m}      |  Até m ocorrências do caracter anterior ou ER anterior |

## Pseudônimo para alguns conjuntos de caracteres

|  Pseud^nimo |  Expressão Regular | Descrição  |
|--------|:------------------:|-------------:|
| \d     |  [0-9]             | Qualquer dígito |
| \D     |  [^0-9]            | Qualquer caracter, com exceção dos dígitos |
| \w     |  [a-ZA-Z0-9_]      | Qualquer caracter alfanumérico ou underline |
| \W     |  [^\w]             | Qualquer caracter, com exceção dos alfanuméricos e underline |
| \s     |  [ \r\t\n\f]       | Espaço em blanco ou tabulação |
| \S     |  [^\s]       | Qualquer caracter, com exceção do espaço em blanco ou tabulação |


## Caracteres especiais

|  Expressão Regular |  Descrição |
|-----------|:------------:|
| \*        |  Asterisco |
| \.        |  Ponto |
| \?        |  Ponto de interrogação |
| \n        |  Salto de linha |
| \t        |  Tabulação |


## Hierarquia de precedência de operadores

Em alguns casos, pode parecer que as expressões regulares são um pouco ambiguas. por exemplo, se queremos encontrar textos onde o número 10 se repete várias vezes (101010), usariamos a expressão regular `/10+/` ou `/(10)+/`? Resposta: `/(10)+/`. Portanto, a continuação apresento a hierarquia de precedência de operadores (o que se leva em consideração ao evaluar uma expressão regular):

1) Parênteses: () <br/>
2) Contadores: + * ? {} <br/>
3) Sequências de caracteres e âncoras: texto ^ $ <br/>
4) Disjunções: | <br/>

## Uma abordagem iterativa pra construção de expressões regulares

Suponha que temos um conjunto de textos sobre computadores e queremos os computadores com discos duros de 500GB ou mais. Este tipo de problema pode ser solucionado usando uma abordagem iterativa. Por exemplo,

1) Começamos identificando os números ao lado de um GB: `/[0-9]+GB/` <br/>
2) Às vezes os textos podem conter um espeço em branco separando o número do GB. Por exemplo, `650 GB`. Para solucionar esse problema usamos `/[0-9]+ ?GB/` <br/>
3) Como a gente quer 500GB o mais então usamos `/([5-9][0-9]{2}|[1-9][0-9]{3,}) ?GB/` <br/>
3.1) A primera parte (`[5-9][0-9]{2}`) encontra os números entre 500 e 999 <br/>
3.2) A segunda parte (`[1-9][0-9]{3,}`) encontra os números maiores a 999

Se a gente quiser melhorar ainda mais a expressão regular, podemos levar em conta que a palavra `gigabytes` pode ser usada em vez de `GB`. Podemos levar em conta o uso de minúsculas. Podemos levar em conta que depois de 1023 GB começa os `terabytes`. Portanto, ainda tem muita coisa pra melhorar a expressão regular. A abordagem iterativa permite obter uma expressão regular complexa a partir de pequenas melhoras de uma expressão regular simples.

Esta abordagem iterativa procura a minimização dos falsos positivos (cadeias de texto que identificamos, mas estão erradas) e minimização dos falsos negativos (cadeias de texto que não identificamos, mas deberiamos)

## Outros usos das expressões regulares

As expressões regulares podem ser usada para a substituição de caracteres ou ER usando a seguinte síntaxis: `s/regex/padrão`. Por exemplo, `s/\bpra\b/para a/` para substituir todas as ocorrências de `pra` por `para a`.

Grupo de captura: um jeito de armazenar padrões em memória para seu uso na mesma expressão regular. Qualquer padrão no meio de parênteses pode ser armazenado em memória. Por exemplo, a expressão regular `/MAIS SABE O (.*) POR SER VELHO QUE POR SER \1\./` identifica o texto `MAIS SABE O DIABO POR SER VELHO QUE POR SER DIABO.`. A gente pode especificar que não quer um grupo de captura usando `?:` depois de abrir parênteses. Por exemplo, a expressão regular `/(?:alguns|uns poucos) (cachorros|gatos) odeiam a outros \1/` identifica o texto `/alguns cachorros odeiam a outros cachorros/`.

Os padrões de largo zero se usam para parar a busca assim o padrão é identificado e são os seguintes:

1) Antecipação positiva `(?= padrão)` é verdadeiro se é identificado o padrão e para a busca. <br/>
2) Antecipação negativa `(?! padrão)` é verdadeiro quando NÃO é identificado o padrão e para a busca <br/>

Suponha que queremos identificar, no inicio da linha, qualquer palavra que não começa com Volcão. Usamos a seguinte expressão regular: `/^(?!Volcão)[/w]+/`. Essa expressão regular para, se encontra uma palavra diferente de `Volcão` no inicio da linha.

## Referências

Esta publicação é um resumo da seção 2.1. do livro Speech and Language Processing. Daniel Jurafsky & James H. Martin. Draft of October 2, 2019. Disponível [aqui](https://web.stanford.edu/~jurafsky/slp3/){:target="_blank"}
