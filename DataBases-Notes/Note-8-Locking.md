# Bloqueos

SQLite aplica bloqueos por defecto para que mas de una persona no escriban al mismo tiempo y corrompan asi el archivo.

## Tipos de Bloqueos:

1. SHARED: Compartido, muchos pueden leer al mismo tiempo.

2. RESERVED: Alguien planea escribir pronto.

3. EXCLUSIVE: Alguien esta escribiendo, nadie mas puwde leer ni escribir en ese momento.

```sql 
BEGIN TRANSACTION;
INSERT INTO Autores (Nombre, Email)
VALUES ('Prueva', 'Erro@text.com');
-- imagina qie te arrepientes o hay un error
ROLLBACK
```

## Procedimientos almacenados

Un precedimient almacenado es un conjunto de instrucciones SQL que se guaedan en el servidor para ser reutilizadas. SQLite al ser ligero no soporta procedimientos almacenados, la logica de;esto se crea en conjunto con python usan la libreria sqlite3.

*ejemplo*:

```python
import sqlite3

#1. Definimos la funcion en Lython puro.

def estilo_shadow(texto):
    return f"[SR-07]{texto.upper()}"

#2. conectamos a la base de datos

conn = sqlite3.connect('blog.db')

#3. la magia, registramos la funcion de python en SQLite
# El primer argumento es el nombre que usaremos en SQL.
# El segundo es cuantos argumentos rwcibe.

conn.create_function("SHADOW_F0RMAT", 1, estilo_shadow)

#4. Ahora;podemos usarla en un SELECT nornal.

cursor = conn.cursor()
cursor.execute("SELECT SHADOW_F0RMAT(Nombre) FROM Autores")

print(cursor.fetchall())
```
