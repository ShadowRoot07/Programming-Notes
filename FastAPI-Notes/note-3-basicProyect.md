# Como montar y crear un proyecto.

En el mundo de FastAPI, no necesitas una estructura de carpetas grande como en Django. Co un solo archivo basta para empezar.

### Creacion del archivo principal:

```python
from fastapi import FastAPI

#creamos la instancia de la aplicacion
app= FastAPI(title= "Mi primera API")

#Una ruta de ejemplo:
@app.get("/")
def inicio():
    return {"mensaje": "Bienvenido a mi API creada con FastAPI!"}
```

## Como crear una operacion Get?:

La operacion get se usa para leer o recuperar informacion del servidor. En FastAPI se crea un decorador que conecta una direccion url con una funcion de python.

### Autonomia de un Get

* *@app.get("/ruta")*: El decorador define que cuando alguien entre a la ruta "/ruta" o la que este dentr del parametro; usando el metodo get, se dispare la funcion de abajo.

* *def nombre_funcion()*: la logica que se ejecuta.

* *return*: FastAPI convierte auntomaticamente los diccionarios en objetos JSON (el standar de las APIs).

**Ejemplo con parametros**:

```python
@app.get("item/{item_id}")
def leer_item(item_id: int):
    return {"item_id": item_id, "esdato": "localizado"}
```

## Como Desplegar un servidor con Uvicorn.

FastAPI es el codigo, peroe ste necesita un motor para ser ejecutado, correr y escuchar las peticiones dek internet. Ese motor en Uvicorn.

**El comando para desplegar en Termux:** 

```bash
uvicorn: main:app --reload
```

### Que significa cada parte?

* *main*: es el nombre del archivo de python (main.py), ai tu archivo principal se llama api.py, podrias poner "api:app" en el comando.

* *app*: es el nombre de la variable que creaste dentro del codigo (app = FastAPI), es un smestandar a seguit, pero si usas otro nombre como "api_app" en los comandos se deberia poner "main:api_app".

* *--reload*: Este es el **Mas importante para estudiar!**, hace que cada vez que hagas un cambio en el codigo, el servidor se reinicie solo, no tienes que apa ni encender el servidor cada vez que escribas una liena nueva.
