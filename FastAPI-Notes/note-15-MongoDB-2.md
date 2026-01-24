# MongoDB (Cambiar la base dedatos de local a la nube).

Pasar de un MongoDB local a uno profecional (como MongoDB Atlas) se tienen que seguir estos pasos:

1. **Crear una cuenta en MongoDB Atlas:** Es el estandar gratuito en la nube.

2. **Obtener la cadena de conexion (Connection String):** Sera algo como "mongodb+srv://usuario:password@cluster0.mongodb.net/...".


3. **Cambiar en tu codigo:**

```python
# local termux:
cliente= MongoDBClient("mongodb://localhost:27017/")

# NUBE (Atlas).
cliente= MongoDBClient("mongodb+srv://usuario:password@cluster0.mongodb.net/...")
```

## Diferencia clave:

En la nube no necesitas ejecutar mongod en tu terminal, el servidor siemore esta encendido.

## Funcion POST: Diferencias entre Lista y MongoDB.

Antes usabamos `.append()` en una lista de python. Con MongoDB usamos .insert_one().

**codigo de ejemplo:**

```python
@app.post("/heroes/")
async def crear_heroe(heroe: Heroe):
    # Convertimos el modelo de pydantic a diccionario
    heroe_dict= heroe.dict()

    # Insertamos en MongoDB
    resultado= db.coleccion_heroes.insert_one(heroe_dict)

    # Retornamos el ID que MongoDB genero automaticamente
    return {"id": str(resultado.inserted_id), "msg": "Heroe guardado"}
```

## Diferencias principales:

* **Persistencia:** Si apagas termux, los datos siguen en MongoDB, en la lista se borraran.

* **ID Automatico:** MongoDB crea un campo _id unico para cada registro. Ya no necesitas gestionar tu los IDs manualmente.

* **Asincronia:** Se recomienda usar `async def` para no bloquear el servidor mientras la base de datos escribe el archivo.

## Esquemas y modelos con PyMongo:

Aqui hay un truco importante: **PyMongo no entiende los modelos de PyDantic** directamente. Necesitamos un "traductor".

* **Esquema de PyDantic:** Define como entran los datos (validacion).

* **Modelo de base de Datos:** Como se guardan.

*Ejemplo de manejo prpfesional*:

```python
from pydantic import BaseModel, Field
from bson import ObjectId # Para manejar los IDs de Mongo 

# Clase para que PyDantic acepte el formato ID de Mongo
class PyObjectId(ObjectId):
    @classmethod
    def __get_validators__(cls):
        yield cls.validate


# El Esquema de tu objeto:
class HeroeModel(BaseModel):
    nombre: str 
    poder: str
    fuerza: int = Field(gt=0, lt=101) #validacion entre 1 y 100
```

## Estructuras de carpetas para proyectos grandes.

Cuando un proyecto crece el archivo "main.py" desaparece y se convierte en una estructura modular. Esta es la estructura estandar se la industria:

```text
mi_gran_proyecto/
|--app/
|   |--__init__.py 
|   |--main.py # Punto de entrada para iniciar FastAPI
|   |--database.py # Configuracion de la conexion a MongoDB 
|   |--routers/ # Separacion por temas
|   |   |--usuarios.py 
|   |   |--heroes.py 
|   |--models/ # Esquemas de pydantic
|   |   |--heroe.py 
|   |   |--usuario.py 
|   |--auth/ # Logica de JWT y Passlib
|   |   |---security.py 
|---static/ # Archivos estaticos (iImagenes, CSS)
|   |---.env # Variables secretas
|---requeriments.txt # Lista de librerias para intalar
```
