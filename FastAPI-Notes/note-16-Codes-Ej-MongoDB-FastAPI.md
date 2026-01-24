# Codigos de Ejemplo de *FastAPI con MongoDB*.

# 1. Como apuntar la base de datos de forma local?

En python usamos la URIde forma local. Por defecto, MongoDB corre en el puerto 27017.

```python
from pymongo import MongoClient

# Conexion local estandar:
# 'localhost' le dice qur busque el motor en tu propio termux.
conexion= MongoClient("mongodb://loaclhost:27017/")

# Crear o apuntar a una base de datos especifica.
db=conexion["mi_proyecto_db"]

# Apuntar a una coleccion como una tabla:

usuarios_col= db["usuarios"]
```

## Operacion GET (Leer datos).

Para leer usamos .find() (para muchos) o find_one() (para uno solo).

**Importante:** MongoDB devuelve los IDs como un objeto especial (ObjectID). Para enviarlo por FastAPI, debemos convertirlo a str.

```python
from bson import ObjectID

@app.get("/usuarios/")
async def obtener_usuarios():
    usuarios=[]
    # Busacamos todos los documentos 
    for user in usuarios_col.find():
        # Convertimos el ID de Mongo a string para que node error
        user["id"]= str(user["id"])
        usuarios.append(user)
    return usuarios


@app.get("/usuarios/{id}")
async def obtener_usuario(id: str):
    # Busacamos por el ID unico de MongoDB
    user = usuarios_col.find_one({"id": ObjectID(id)})

    if user:
        user["id"]= str(user["_id"])
        return user

    raise HTTPException(status_code= 404, detail="No encontrado")
```

## Operacion PUT (Actualizar datos).

Para actualizar utilizamos `update_one()`. Necesitamos dos cosas: un filtro (a quien actualizar), y los nuevos datos.

```python
@app.put("/usuarios/{id}")
asycn def actualizar_usuario(id: str, nuevos_datos: dict):
    # El operador '$set' es fudamental en Mongo para cambiar valores
    resultado= usuarios_col.update_one(
        {"_id": ObjectId(id)},
        {"$set": nuevos_datos}
    )

    if resultado.modified_count == 1:
        return {"msg": "Actualizado con exito"}

    raise HTTPException(status_code=404, detail="No se encontro o no hay cambios")
```

## Como limpiar la base de datos o borrar archivos.

En MongoDB no borramos "archivos", borramos documentos (filas), o colecciones (tablas completas).

* **Borrar un documento especifico:**

```python
usuarios_col.delete_one({"_id": ObjectId(id)})
```

* **limpiar una coleccion completa (Borrar toda la columna):**

```python
usuarios_col.delete_many({}) # El diccionario vacio significa "todos".
```

* **Borrar toda la base de datos:**

```python
conexion.drop_database("mi_proyecto_db") # No me esperaba esta funcion (o0o)
```

## Comandos rapidos desde la terminal (MongoShell).

Si quieres limpiar o ver los datos rapido sin usar python o HTTPie, entra a la consola con mongosh y usa estos comandos:

1. `use mi_proyecto_db` (seleccionar base de datos).

2. `db.usuarios.find()` (Ver todos los usuarios).

3. `db.usuarios.deleteMany()` (Borra todos los usuarios, cuidado con este).

4. `db.dropDatabase()` (Borra la base de datos completa dpnde estas parado).

## Mapa que resume las diferencias (Antes vz Ahora):

|Accion|Antes (listas)|Ahora (MongoDB)|
|:---|:---|:---|
|Aniadir|.append()|insert_one()|
|Buscar|for x in lista if...|find_one()|
|Actualizar|lista.pop() o del|.delete_one()|

