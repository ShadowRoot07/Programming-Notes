# APIRouter.

En proyectos reales se podrian tener cientos de rutas, con el usa de esta extencion de "*APIRouter*" se puede simplificar el uso y creacion de rutas en directorios de diferentes carpetas.

## Que es APIRouter?

Es una herramienta de FastAPI que permite modularizar tu aplicacion, imagina que `app= FastAPI()` es el edificio principal y los APIRouter son los diferentes departamentos: usuarios, productos, lecturas, ect.

## Como usarlo (paso a paso).

para practicar esto, vamos a separar un codigo que tengo hecho en dos archivos:

* Archivo 1: routers/heroes.py 

Aqui creamos los routers especificos para los heroes:

```python
from fastapai import APIRouter, HTTPException

#Creamos el Router en lugar de la app
router= APIRouter(
    prefix="/heroes", #todas las rutas de este archivo empezara con heroes
    tags= ["Heroes"] #esto las agrupa finalmente en la documentacion
)

base_de_datos= []

@router.get("/")
def obtener_heroes():
    return base_de_datos

@router.post("/")
def crear_heroe(nombre: str):
    base_de_datos.append(nombre)
    return {"mensaje": "Heroe aniadido"}
```

* Archivo 2: main.py (El corazon de la app).

Aqui es donde conectas todas las piezas.

```python
from fastapai import FastAPI
from routers import heroes #Importamos el nuevo archivo

app= FastAPI()

#Incluimos el router en la aplicacion principal

app.include_router(heroes.router)

@app.get("/")
def home():
    return {"Mensaje": "Bienvenido a la API central"}
```

## Ventajas de usar APIRouter.

* 1. Organizacion: Puedes tener un archivo para autenticacion.py, y otro para usiarios.py, ect.

* 2. Prefijos: Al poner `prefix="/heroes"` ya no tienes que escribir `"/heroes"` en cada `router.get`. FastAPI lo aniade automaticamente.

* 3. Mantenimiento: Si ahy un error en la logica de alguna parte, ya sabes en cual archivo buscar.

## Tip especial.

Cuando empieces a usar parios archivos asegurate de crear una carpeta llamada *routers/* y dentro de ella coloca un archivo vacio llamado *__init__.py*, esto le dice a Python que esa carpeta es un paquete y que puede importar loa archivos de adentro.

```bash
mkdir routers/
touch routers/__init__.py
```
