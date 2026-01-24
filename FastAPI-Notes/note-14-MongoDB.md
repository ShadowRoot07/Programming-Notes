# MongoDB.

Con MongoDB la API pasara a ser un "juguete" de practica qur olvida todo a ser una verdadera aplicacion profecional, que guarda loa datos permanentwmente.

## Diferencias: Relacional (SQL), No Relacional (NoSQL).

Imagina que estas organizando una biblioteca:

* **Relacional (SQL):** Es como un eatante con etiquetas fijas, tienes que definir qur columnas tienen cada tabla ()ID, Nombre, edad). Si quieres agregar un dato nuevo que no estaba planeado, tienes que remodelar todo el estante.

* **No Relacional (No SQL):** Es como una serie de carpetas (documentos). Cada caroeta puede tener hojas distintas. Un usuario puede tener telefono y otro no, y a MongoDB no le importa. Se guarda en formato JSON (tecnicamente BSON), lo cual encaja perfecto con FastAPI.

|Caracteristica|SQL (Relacional)|MongoDB (No SQL)|
|:---|:---|:---|
|Estructura|Tablas y filas|Colecciones y documentos|
|Esquemas|Rigido (fijo)|Flexible (Dinamico)|
|Relacionales|Complejas (JOINs)|Embebidas (documentos dentro de otros)|
|Escalabilidad|Vertical (Mas potencia)|Horizontal (Mas servidores)|


1. **Iniciar el servidor.**

Para que MongoDB funcione, el "motor" debe de estar encendido en una pestania de termux:

```bash
mongod --dbpath $PREFIX/var/lib/mongodb
```

*Dejala abierta, y **abre** una nueva pestania de termux para seguir con wl proceso*

## como visualizar los datos?

En termux no tenemos una interfaz grafica como "MongoDB Compass", pero tenemos el *Mongo Shell*, que es muy potente:

1. En una nueva pestania escribe mongosh (o mongo segun tu version).

2. **Comandos basicos de Shell:**

    * show dbs (Ver todas las bases de datos).
    
    * use mi_base_de_datos (Crear o usar una base de datos).

    * db.usuarios.find().pretty() (Ver todos los usuarios de forma legible).


## Conectando con FastAPI y MongoDB (Ejemplo rapido).

Aqui hay un ejemplo de como insertar un usuario en una base de datos local en vez de una lista:

```python
from pymongo import MongoClient

# Conexion al servidor loacal:
cliente= MongoClient("mongodb://localhost:27017/")

# Crear, acceder a la base de datos

db= cliente["mi_app_fastapi"]

# Acceder a una colecion, equivalente a una tabla.

colecion_usuarios= db["usuarios"]

# Ejemplo de insertar un dato:
@app.post("/usuarios/")
def guardar_usuario(nombre: str):
    nuevo_usuario= {"nombre": nombre,"Activo": True}
    resultado= colecion_usuarios.insert_one(nuevo_usuario)
    return {"id insertado": str(resultado.insert_id)}
```
