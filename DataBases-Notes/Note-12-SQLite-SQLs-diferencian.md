# Diferencias estrategicas de SQLite con otros SQLs.

|**Caracteristicas**|*SQLite*|*MySQL*|*PostgreSQL*|*SQLServer*|
|:--|:--:|:--:|:--:|:--:|
|Tipo|Sin servidor, es un archivo|Servidor (serbidor cliente)|Servidor (Objeto relacional)|Servidor (Microsoft)|
|Uso ideal|Apps Moviles, loT, pruebas, desarrollo local| Sitios web, CMS (WordPress), e-commerce|Ciencia de datos, apps complejas, fastAPI|entornos corporativos Windows|
|Costo|Gratis (dominio publico)|Gratis (Community), Pago (Enterprise)|Gratis (Open Source total)|Pago (licencias caras)/ Gratis (Express)|
|Tipado|Debil (flexible)|Estricto|Muy estricto, seguridad total|Estricto|

## Diferencias de Sintaxis y codigo.

1. **Estructura de tablas, claves primarias:**

* SQLite: `id INTEGER PRIMARY KEY AUTOINCREMENT`
* MySQL: `id INTEGER PRIMARY KEY AUTO_INCRENET`
* PostgreSQL: `id SERIAL PRIMARY KEY`(o `IDENTITY`)
* SQLServer: `id INT PRIMARY KEY IDENTITY(1,1)`

2. **Funciones y Operaciones Clave:**

|**Caracteristica**|*SQLite*|*MySQL*|*PostgreSQL*|*SQLServer*|
|:--|:--:|:--:|:--:|:--:|
|SELECT NOW|date('now')|NOW()|NOW() o CURRENT_TIMESTAMP|GETDATE()|
|CEILING|CEIL(x)*|CEIL(x)|CEIL(x)|CELING(x)|
|FLOOR|FLOOR(x)|FLOOR|FLOOR(x)|FLOOR(x)|
|LIMIT|LIMIT 10|LIMIT 10|LIMIT 100|SELECT TOP 10|
|OFFSET|OFFSET 5|OFFSET 5|OFFSET 5|OFFSET 5 ROWS|
|LIKE|Case Insensitive|Case Insensitive|ILIKE (para insensitive)|Depende del Collation|

3. **Los JOINs (el gran cambio):**

    * LEFT JOIN: Todos lo soportan igual.
    * RIGHT JOIN/ FULL JOIN: El unico que no lo soporta es SQLite (pero se rmulan).
        * MySQL y PostgreSQL lo soportan nativamente.

## Como exportar y importar (migracion).

1. **De un SQL a otro SQL (el Dump):**

* En SQLite se usa el comando `.dump`.

```sql 
sqlite3 blog.db .dump > backung_blog.sql
```

2. **Hacia otra DB:** Tomas ese archivo y lo ejecutas en rl nuevo motor.

    * Ojo: Tendras que editar pequenias cosas, su sintaxis.

 Tomas ese archivo y lo ejecutas en rl nuevo motor.

3. **De CSV a Bases de Datos**

El formato CSV es el lemguaje universal de los datos.

     * SQLite:

```sql
.mode csv 
.import datos.csv NombreDeLaTabla
```

     * Pandas: 

```python
import pandas as pd 
import sqlite3

# Leer CSV
df = pd.read_csv('datos.csv')

# Enviar a cualquier SQL
conn= sqlite3.connect('destino.db')
df.to_sql('nueva_tabla', conn, if_exists='replace', index=False)
```
