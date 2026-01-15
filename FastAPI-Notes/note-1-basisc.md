# Conceptos basicos.

## Que es el Backend?

En el software, el backend es la parte de la aplicacion que corre el servidor, se encarga de:

* 1. **Gestionar la base de datos**, guardar datos, usuarios, productos.

* 2. **validar qie las contrasenias sean correctas**.

* 3. **Procesar pagos o calculos pesados**.

## Que es una API?

API significa *Aplication Programing Interface* (interfaz de programacion de aplicaciones). Una API es el puente que permite que dos programas hablen entre si.

## FastAPI.

FastAPI es un framework (marco de trabajo) que sirve para crear APIs en python, es muy moderno y de alto rendimiento.

Sirve para crear la estructura de tu API segura, sus puntos fuertes son:

* **Velicidad**: Es uno de los frameworks de python mas rapidos que existen, gracias a sus compiladores de Rust.

* **Validacion automatica**: Si esperas un numero y el usuario te da una letra, FastAPI da error al detectar eso sin que tu tengas que construir codigo extra.

* **Documentacion automatica**: Por solo escribir codigo, FastAPI genera una web "/docs" donde puedes probar tu API visualmente.

### Codigo de ejemplo:

Para ver esto en accion, se crea un archivo simple llamado main.py:

```python
from fastapi import FastAPI

#creamos la instancia de la api
app= FastAPI()

#Definimos una ruta (EndPoint)
@app.get("/")
def leer_raiz():
    return {"mensaje": "Hola mundo, esta es mi primera FastAPI"}

@app.get("/usuario/{nombre}")
def saludar_usuario(nombre: str):
    return {"saludo": f"Hola {nombre} bienvenido al backend."}
```

Para ejecutarlo en termux:

```bash
uvicorn main:app --reload
```

###Asi se crea un servidor sencillo en FastAPI.
