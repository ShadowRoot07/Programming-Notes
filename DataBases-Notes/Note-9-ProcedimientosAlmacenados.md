# Procedimientos almacenados

**Definicion tecnica**

Es un segmento de codigo SQL que se almacena fisicamente en la base de datos. A diferencia de una consulta normal, este se compila una vez y se puede ejecutar muchas veces pasando parametros.

*SQLite* no puede tener procedimientos almacenados, solo para esta practica se pueden usar MySQL y PostgreSQL, pero se podria usar python para crear esta tarea.

**Forma de trabajarlo con Python**

```python
import sqlite3 

def calcular_impuesto():
    return precio * 0.16 # IVA del 16%

conexion= sqlite3.connect("blog.db")

# Registramos la funcion en la base de datos
# nombre de sql, numero de argumentos, funcion de python
conexion.create_function("IVA", 1, calcular_impuesto)

cursor= conexion.cursor()
# Ahora SQL conoce la funcion "IVA"
cursor.execute("SELECT titulo IVA(Precio) FROM Posts")
```


