# BaseModel

## Que es un BaseModel y para que sirve?

Un BaseModel es una clase de la livreria de Pydantic. Sirve para definir el "Esquema" o la forma que deben de tener loa datos que tu API va a recibir on enviar.

Sin BaseModel tendrias que validar manualmente si el usuario envio un nombre, si es texto, si envio la edad, si es entero, ect. Con BaseModel FastAPI lo hace por ti.

## Ejemplo: Construyendo un Ejemplo para una API.

Imagina que estamos creando una API para una tienda de videojuegos. Queremos manejar un objeto tipo "Juego".

*###Copiar este codigo luego en tu main.py*
```python
from fastapi import FastAPI
from pydantic import BaseModel
fron typing import Optional

app= FastAPI()

#Definimos el esquema ddl proyecto usando BaseModel
class Juego(BaseModel):
    titulo: str
    consola: str
    precio: float
    #El Optional te permite que este campo no sea obligatorio
    stock: Optional[int]=10

#Base de datos falsa (una lista)
base_datos_juegos= []

#---OPERACION-CREATE (POST) ---
@app.post("/Juegos/")
def crear_juego(juego: Juego):
    #convertimos el objeto a diccionario para guardarlo
    base_datos_juegos.append(juego.dict())
    return {"mensaje":"juego guardado", "datos": juego}

#--- Operacion Read GET ---
@app.get("/juegos/")
def listar_juegos():
    return base_datos_juegos
```

## Como funciona esto en la practica?

1. **Definicion**: La crear class Juego(BaseModel), le dices a FastAPI:"Cualquier cosa que le llegue a la ruta /juegos/ debe de tener un titulo (str), una consola (str), un precio (float)".

2. **Validacion Automatica**: Si tu intentas enviar un precio que sea una palabra (ej. "caro"), FastAPI rechazara tu peticion antes de que llegue a la funcion crear_juego.

3. **Uso de datos**: Dentro de la funcion, juego ya no es un simple texto, es un **objeto de python**, puedes acceder a sus datos con un punto: juego.titulo.
