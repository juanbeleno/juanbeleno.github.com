---
layout: post
title: "Leer archivos XML con pandas"
author: "Juan Beleño"
categories: journal
tags: [pandas,spanish]
---

Antes de escribir código, quiero mostrarles la estructura del XML porque el código va a varias según la estructura del XML con el que estémos trabajando. El archivo XML tiene datos inventados por mi y están disponibles [aquí](../assets/others/leer-un-archivo-xml-en-pandas/datos_ejemplo_1.xml){:target="_blank"}. El contenido del archivo XML se presenta a continuación.

```xml
<datos>
    <paciente nombre="Juan">
        <sexo_biologico>Masculino</sexo_biologico>
        <edad>16</edad>
    </paciente>
    <paciente nombre="Maria">
        <sexo_biologico>Femenino</sexo_biologico>
        <edad>24</edad>
    </paciente>
    <paciente nombre="Ariel">
        <sexo_biologico>Masculino</sexo_biologico>
        <edad>37</edad>
    </paciente>
</datos>
```

En el siguiente código, creamos un DataFrame a partir de un archivo de XML que tiene el contenido anteriormente mostrado.

```python
import pandas as pd
import xml.etree.ElementTree as et

# Obtenemos la raíz del árbol XML, es decir, <datos>
# Es poco intuitivo ver un árbol en el XML. Sin embargo, lo llamamos de esa
# manera y la raíz es la etiqueta que engloba todo el archivo, en este caso,
# <datos>. A cada etiqueta <paciente> dentro de <datos> la llamamos un nodo.
ruta_archivo_xml = '/Users/Juan/Downloads/datos_ejemplo_1.xml'
arbol = et.parse(ruta_archivo_xml)
raiz = arbol.getroot()

# A partir de aquí, podemos iterar sobre los datos de los pacientes y
# almacenarlos en una lista de diccionarios que es un formato que es un formato
# que admite pandas para convertirlo en DataFrame.
lista_de_diccionarios = []
for nodo in raiz:
    # Como el nombre es un atributo de la etiqueta paciente, lo obtenemos de
    # esta manera
    nombre = nodo.attrib.get("nombre")
    # Como la edad y el sexo biologico están dentro de la etiqueta paciente,
    # los obtenemos de la siguiente manera
    edad = int(nodo.find("edad").text)
    sexo_biologico = nodo.find("sexo_biologico").text

    # Creamos un diccionario con los valores extraídos del XML. Las llaves del
    # diccionario van a ser los nombres de las columnas del futuro DataFrame
    diccionario = {
        'nombre': nombre,
        'edad': edad,
        'sexo_biologico': sexo_biologico
    }

    lista_de_diccionarios.append(diccionario)

# Aquí está nuestro DataFrame :)
datos = pd.DataFrame(lista_de_diccionarios)
```

**Nota:** Estoy usando `pandas 1.0.3`, `pyhton 3.7` y un `Macbook Pro 2015`, entonces posiblemente te toque cambiar las rutas absolutas de los archivos según tu sistema operativo.
