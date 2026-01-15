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

([falta por escribir])
