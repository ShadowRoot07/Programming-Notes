# CRUD con python y SQLite.

**Uso de la sentencia with**

Es la manera profecional, establece la conexion, maneja los datos automaticamente, y puede crear el cierre automaticamente como las transacciones tambien.

## Ejemplo de CRUD completo.

```python
import sqlite3

database = "blog.db"

# Create
def crear_autor(nombre, email):
    with sqlite3.connect(database) as conn:
        cursor = conn.cursor()
        query = "INSERT INTO Autores (Nombre, Email) VALUES (?,?)"
        cursor.execute(query, (nombre, email))
# no hace falta conn.commit() si el context manager lo maneja bien
# pero es buena practica en algunas versuones


# Read
def obtener_autores():
    with sqlite3.connect(database) as conn:
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM Autores")
# fetchall() devuelve una lista de tuplas
# fetchone() devuelve solo la primera fila
        return cursor.fetchall()


# Update
def actualizar_email(autor_id, nuevo_email):
    with sqlite3.connect(database) as conn:
        cursor = conn.cursor()
        cursor.execute("UPDATE Autores SET Email =? WHERE AutorId = ?", (nuevo_email, autor_id))


# Delete
def eliminar_autor(autor_id):
    with sqlite3.connect(database) as conn:
        cursor = con..cursor()
        cursor.execute("DELETE FROM Autore WHERE AutorId = ?", (autor_id,))

if __name__ == "__main__":
    obtener_autores()
    actualizar_email()
    eliminar_autor()
```

* `cursor.execute()`: envia la orden.
* `cursor.fetchone()`: Trae la siguiente fila, ideal para buscar por id.
* `cursor.fetchall()`: Trae todos los registros de golpe, cuidado con la ram al hacer esto.

**Regla de oro: Parametros Seguros**

Nunca uses f-strings, siempre usa los parametros string vistos aqui, eso evita que alguien borre la base de datos mediante inyeccion.


**Por que se usa `if __name__ = "__main__":`**

Es una buena practica de Python, significa que si este archico corre correctamente, corre eatas funciones. Sirve mucho en proyectos grandes.


