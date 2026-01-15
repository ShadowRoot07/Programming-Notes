# Type Hints

Sugerencias de tipo, son una caractetistica qur python sugiere que deberia pasarse o como deberia ser una variable, un parametro de funcion o el valor que devuelve una funcion.

Eb python normal las variables son dinamicas, en python con type hints le pones una "etiqueta" para ser mas claro:

```python
#sin type hints python no sabe que esperar
def saludar(nombre):
    return "hola" + nombre

#Con type hints le decimos a nombre que es un string
def saludar(nombre: str)->str:
    return "hola" + nombre
```

## Por que son vitales para FastAPI?

FastAPI no usa los type hints solo para que el codigo se vea bonito; los usa como instruciones de contrucion, aqui van las tres raxones principales:

* 1. *Validacion de datos automatica*:

Si tu declaras que un parametro es entero (edaf: int), un usuario envia por la API el texto "veinte", FastAPI detecta automaticamente qir no coincide y responde con un error de 422 Unprocessable Entity, tu no tienes que escribir ni un solo "if".

* 2. *Convercion de datos (**Parsing**)*:

Si el usuario envia un numero en un texto "25", a travez de la url, FastAPI lee tu type hint, ve que quieres un int y lo transforma automaticamente en un numero real de python antes de que llegue a tu funcion.

* 3. *Documentacion interactiva (**Swagger**)*:

FastAPI lee tus tipos de datos para construir la pagina de documentacion (/docs). Si pones str, la web aparecera con un cuadro de texto; si pones int, sabra que es un campo numerico.

### Ejmeplo practico en FastAPI:

```python
from fastapi import FastAPI

app= FastAPI()

@app.get("/validar-edad/{edad}")
#gracias al type hint 'int', FastAPI validara esto por ti 
def check_edad(edad: int):
    return {"tu edad es": edad, "en diez anios": edad + 10}
```
