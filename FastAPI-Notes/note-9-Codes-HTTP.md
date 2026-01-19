# Codigos http "obligatorios".

Se dividen en familias, estos seran los que usare el 90% del tiempo:

|Codigo|Nombre|Cuando usar?|
|:---:|:---:|:---|
|200|OK|Peticion Exitosa (tipico en GET)|
|201|create|Exito al crear un recurso (tipico de POST)|
|204|No conect|Exito pero no hay nada que devolver|
|400|Bad request|El cliente envio algo mal(error de sintaxis)|
|401|Unauthorized|Falta de autorizacion (no esta logueado)|
|403|Forbidden|Esta logueado pero no tiene permiso para ver eso.|
|404|Not Found|Lo que busca no existe.|
|500|Internal Server error|Tu codigo fallo (un bug o un error de logica)|


# Como agregar un codigo de respuesta a una funcion?

Por defecto, FastAPI devuelve un 200 OK. Si quieres cambiarlo a un 201 para indicar que creaste algo, se hace el decorador usando status_code.

```python
from fastapi import FastAPI, status 

app= FastAPI()

@app.post("/items/", status_code=status.HTTP_201_CREATED)
def crear_item(nombre: str):
    return {"mensaje":"Item creado", "nombre", nombre}
```

*Tip*: es mejor usar "status.HTTP_201_CREATED" en lugar de poner el numero 201 a mano por que hace el codigo mas legible.

# Manejo de HTTPException

A diferencia de status_code el decorador (que se usa cuando todo sale bien), la clase HTTPException se usa para lanzar errores cuando algo sale mal.

Cuando lanzas una HTTPException, FastAPI detiene la ejecucion de la funcion y envia la respuesta de error inmediatamente.

*Ejemplo*: validando un recurso existente.

```python
from fastapi import FastAPI, HTTPException, status

app= FastAPI()

productos={1: "teclado"}

@app.get("/producto/{id}")
def obtener_producto(id: int):
    if id not in productos:
        #Aqui lanzamos el error
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            details= f"El producto con ID {id} no existe en nuestra base de datos",
        )
        return {"producto": productos[id]}
```

## Como probar esto con HTTPie

Es genial para saber si realmente estas recibiendo el codigo correcto. Usa el comando normal y fijate en la primera linea de la respuesta:

```bash
#Probando un recurso que no existe para ver el 404

htttp GET :8000/producto/99
```

Se vera algo como:

`HTTP/1.1 404 Not Found content-type: aplication/json {"detail": "el producto con ID 99 no existe en nuestra bas de datos"}`

**Resumen:**

* Decorador (status_code): Para el "camino Feliz" (Exito).

* raise HTTPException (...): Para el "camino triste", (errores).
