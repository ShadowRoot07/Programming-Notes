# Construccion de finciones basicas CRUD.

## Construccion de POST, PUT, DELETE.

Para estos ejemplos usaremos una lista de Python como si fuera nuestra base de datos y BaseModel para validar los datos.

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import Optional

app= FastAPI()

# Definimos el modelo de datos
class Producto(BaseModel):
    nombre: str
    precio: float
    description: Optional[str]= None

# Base de datos falsa
db_productos= {
    1: {"nombre":"laptop", "precio":1200, "descripcion": "potente para programar"}
}

# --- POST (crear) ---
@app.post("/productos/")
def crear_producto(id: int, producto: Producto):
    if id in db_productos:
        raise HTTPException(status_code=400, detail="El ID ya existe")
        db_productos[id]=producto.dict()
    return {"mensaje": "producto creado con exito", "data": db_productos[id]}

# --- PUT (actualizar) ---
@app.put("/productos/{id}")
def actualizar_producto(id: int, producto_actualizado: Producto):
    if id not in db_productos:
        raise HTTPException(status_code=404, details="Producto no encontrado.")
    db_productos[id]= producto_actualizado.dict()
    return {"mensaje":"producto actualizado", "data": db_productos[id]}

#--- DELETE (eliminar) ---
@app.delete("/productos/{id}")
def eliminar_producto(id: int):
    if id not in db_productos:
        raise HTTPException(status_code=404, details="ID no encontrado")
        del db_productos[id]
        return {"mensaje":f"Producto con ID {id} fue eliminado"}
```

## Como concatenar una URL?

En el desarrollo de APIs, concatenar se refiere a como construimos la direccion para llegar a un recurso especifico.

* **En el codigo (Backend)**: No "concatenas" manualmente las rutas; FastAPI lo hace por ti al definir el decorador: *@app.put("/productos/{id}")*.

* **En el Cliente (navegador/HTTPie)**: Concatenamos la base de la URL con el PATH parameter y los Query parameters.

    * Base: http://127.0.0.1:8000
    * Path: /productos/1 (apunta directamente al ID 1)
    * Query: ?categoria=laptops (filtra la lista)

## Usando HTTPie para depurar estas funciones

Como estas peticiones nos e pueden hacer escribiendo una url en el navegador (elnavegador solo hace get), HTTPie es el mejor aliado en termux y linux.

Asegurate de que tu servidor este corriendo (uvicorn main:app --reload) y habre una nueva pestania es termux para usar estos comandos:

**Probar el POST (crear).**

Enviamos un JSON con los datos del producto. Usamos ":=" para numeros y "=" para texto:

```bash
http POST :8000/productos/id=2 nombre="monitor" precio:=300.50
```

**Probar el PUT (Actualizar datos).**

Cambiamos los datos del producto que tiene el ID 2:

```bash
http PUT :8000/productos/2 nombre="monitor curvo" precio:=350.00
```

**Probar el DELETE.**

Borramos el producto con el ID.

```bash
http DELETE :8000/productos/1 
```

**Probar el GET (ver Cambiamos).**

```bash
http GET :8000/productos/
```

## Resumen de comandos de HTTPie:

* Sintaxis basica: `http [METODO] [URL] [DATOS]`.

* Atajo de URL: puedes usar ":8000" en lugar de "http://127.0.0.1:8000".

* Tipos de datos: 
 * Campo=valor (envia texto/string).
 * Campo:=valor (envia numeros, booleanos o JSON anidado).
 * Ver detalles: agrega "-v" al principio para ver loa headera que envia tu API.
