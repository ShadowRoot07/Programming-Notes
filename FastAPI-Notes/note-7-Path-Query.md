# Path parameters, Query Parameters.

## 1. PATH Parameters (parametros de ruta).

Se usan para indentificar un recurso especifico. Piensa en ellos como la "direccion exacta" de un objeto. Se definen dentro de la misma url en llaves "{}".

### Como hacer un GET con ID (filtrado por PATH)

Imagina que quierea obtener la informacion de un usuario usando su direccion ID.

```python
from fastapi import FastAPI, HTTPException

app= FastAPI()

usuarios=[
    {"id":1, "nombre": "Gohan"},
    {"id": 2, "nombre": "Videl"}
]

#El {user_id} es el PATH parameter 
@app.get("/usuarios/{user_id}")
def obtener_usuario(user_id: int):
    #Logica de filtrado, buscamos el ID en nuestra lista
    for u in usuarios:
        if u[id] == user_id:
            return u 
        #si no lo encuentra devolvemos un error 404
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
```

## 2. Query parameters (parametros de consulta).

Se usan para filtrar, ordenar o paginar una lista de resultados. No son obligatorios para qur la ruta funcione y aparcen despues del signo de interrogacion "?" en la URL.

### Como obtener los datos de una Query?

En FastAPI no necesitas configurar una ruta con llaves. Simplemente eniades el parametro como un argumento de funcion. FastAPI detectara automaticamente que es una Query por que no esta en la ruta.

```python
#ejemplo de url: http://127.0.0.1:8000/buscar/?d=1 
@app.get("/buscar/")
def buscar_datos(d: int):
    return {"mensaje": f"Has buscado el dato con valor{d}"}
```

## Ejemplo combinado: Filtrado Real.

Vamos a crear una funcion que devuelva todos los usuarios, pero que permita filtrar por nombre opcionalmente usando una Query.

```python
@app.get("/usuarios/")
def listar_usuarios(nombre: str= None): #None lo hace opcional 
    if nombre:
        #Ifltramos la lista si el usuario filtro un nombre por Query
        resultado= [u for u in usuarios if u[nombre].lower() == nombre.lower()]
        return resultado
    return usuarios
```

## Como se verian las llamadas en el Navegador?

1. llamada PATH: "http://127.0.0.1:8000/usuarios/1 
    * *Resultados*: {"id": 1, "nombre":"Gohan"}

2. llamada QUERY: http://127.0.0.1:8000/usuarios/?nombre=Videl
    * *Resultados*: {"id": 2, "nombre":"Videl"}

## Resumen de diferencias:

|Caracteristica|PATH parameter|QUERY parameter|
|:---|:---:|:---:|
|Uso principal|indentificar un recurso unico (ID)|Filtrar o buscar en una lista|
|En la url|Parte de la ruta: /usuarios/1|Despues del ?: /usuarios?id=1 |
|obligatorio|Si, normalmente|No, puedes poner valores por defecto|
|En FastAPI|Se define en la ruta: @app.get("/{id}")|Solo se define en la funcion: def f(id: int)|


