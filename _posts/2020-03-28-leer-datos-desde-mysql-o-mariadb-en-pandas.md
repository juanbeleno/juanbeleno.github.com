---
layout: post
title: "Leer datos desde MySQL o MariaDB con pandas"
author: "Juan Beleño"
categories: journal
tags: [pandas,spanish]
---

En esta publicación explico como convertir el resultado de una consulta de una base de datos en [MySQL](https://www.mysql.com/){:target="_blank"} o en [MariaDB](https://mariadb.org/){:target="_blank"} a un DataFrame. Para eso, vamos a suponer que tenemos una tabla en algún esquema por defecto de nuestra base de datos que sigue la siguiente estructura:

```sql
CREATE TABLE partido_de_futbol (
    identificador INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nombre_equipo_local VARCHAR(30) NOT NULL,
    nombre_equipo_visitante VARCHAR(30) NOT NULL,
    nombre_liga VARCHAR(30) NOT NULL,
    fecha_de_juego DATETIME NOT NULL,
    resultado VARCHAR(30) NULL
)
```
Antes de continuar, debemos instalar un par de bibliotecas de python: [mysql-connector-python](https://pypi.org/project/mysql-connector-python/){:target="_blank"} y [sqlalchemy](https://pypi.org/project/SQLAlchemy/){:target="_blank"}.

```bash
# Es mejor usar las versiones actualizadas de las bibliotecas entonces
# antes de instalar consulta las versiones aquí:
# https://pypi.org/project/mysql-connector-python/
# https://pypi.org/project/SQLAlchemy/
pip install mysql-connector-python==8.0.19 sqlalchemy==1.3.15
```

A continuación muestro como convertir una consulta sobre la tabla `partido_de_futbol` en un DataFrame:


```python
import pandas as pd
from sqlalchemy import create_engine


# Cambia los valores de estas variables por los valores reales de tu base
# de datos MySQL o MariaDB
usuario = 'admin'
contrasena = ''
url_servidor = 'localhost'
puerto = '3306'
esquema = 'default'
plugin_autenticacion = 'mysql_native_password'

# Creamos una cadena de conexión válida que incluye todas las variables
# declaradas arriba.
c_conexion = 'mysql+mysqlconnector://{0}:{1}@{2}:{3}/{4}?auth_plugin={5}'
c_conexion = c_conexion.format(usuario, contrasena, url_servidor, puerto,
                               esquema, plugin_autenticacion)
motor_mysql_mariadb = create_engine(c_conexion)

consulta_sql = '''
SELECT
  -- Obtiene todos los partidos de fútbol que se jueguen en un día
  -- en específico que se va a mandar como parametro.
  *
FROM partido_de_futbol
WHERE DATE(fecha_de_juego) = %s
'''

# Las columnas de este DataFrame serán las columnas seleccionadas en la
# consulta SQL. En este caso, serían todas las columnas de la tabla
# partido_de_futbol: identificador, nombre_equipo_local, nombre_equipo_visitante,
# nombre_liga, fecha_de_juego y resultado
datos = pd.read_sql_query(
        consulta_sql, motor_mysql_mariadb, params=['2020-03-28'])

```

## Parametros
Hay 3 parametros usados en el ejemplo:

**sql:** La consulta SQL que se va a ejecutar.

**con:**  Un objeto `conection` o `engine` de `SQLAlchemy `.

**params:** Una lista de parametros que substituiran los `%s` en la consulta SQL.

Puedes consultar la [documentación oficial](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_sql_query.html){:target="_blank"} para saber que otros parámetros te pueden ser útiles.

**Nota:** Estoy usando `pandas 1.0.3`, `pyhton 3.7` y un `Macbook Pro 2015`.
