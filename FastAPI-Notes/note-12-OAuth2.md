# OAuth2.

Es el estandar que usan Facebook, Google y twiter para asegurar sus aplicaciones. En FastAPI, esto se maneja de forma elegante gracias al modulo "fastapi.security".

## Que es OAuth2 en FastAPI?

OAuth2 es un Protocolo (un conjunto de reglas) que permite al usuario identificarse y obtener un token (una llave digital). con esa llave el usiario puete realizar sin tener que enviar una contrasenia en cada peticion.

Para usarlo importamos "OAuth2PasswordBearer", que le dice a FastAPI donde debe de buscar el token (normalmente en la ruta /token).

```python
from fastapi.security import OAuth2PasswordBearer #Esto define de donde sacaremos el token

oauth_scheme= OAuth2PasswordBearer(tokenUrl= "token")
```

## Como Autenticar tareas (Dependencias).

La magia ocurre tras la inyeccion de Dependencias. Usamos Depends para obligar a que una funcion solo se ejecute si el usuario envia un token valido.

```python
from FastAPI import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer

app= FastAPI()
oauth2_scheme= OAuth2PasswordBearer(tokenUrl="token")

@app.get("/tareas-privadas")
def leer_tareas(token: str= Depends(oauth2_scheme)):
    #Si llega aqui es por que FastAPI encontroun token en los headers
    return {"Token_recibido": token, "tareas": ["Lavar platos", "Estudiar FastAPI"]}
```

## Funciones para Autenticar el usuario.

Normalmente el proceso tiene dos pasos:

1. **Login:** El usuario envia usuario/contrasenia, y tu devuelves un token.

2. **Verificacion:** Tomas el token y verificas que el usuario sea quien dice ser.

Ejemplo de funcion de validacion simple:

```python 
from fastapi.security import OAuth2PasswordRequestForm

@app.post("/token")
def login(form_data: OAuth2PasswordBearerForm= Depends()):
    #Aqui validarias con tu base de datos (MongoDB)
    if form_data.user_name == "Gohan" and form_data.password == "1234":
        return {"accses_token": "llave secretapro", "token_type": "bearer"}
    raise HTTPException(status_code=401, detail= "Usuario o contrasenia incorrectos")
```

## La clase Status en FastAPI.

La clase status es un listado de codigos HTTP. Se usa para qur tu codigo sea legible y **estandar**.

**Para que usarla?:** 

* Evitar errores tipograficos (es mejor status.HTTP_404_NOT_FOUND).

* Tu codigo se explica solo.

**Ejemplo de uso:**

```python
from fastapi import status

@app.delete("/item/{id}", status_code= status.HTTP_204_NO_CONTENT)
def borrar():
    return None 
```

## Diferentes tipos de Autenticacion.

FastAPI soporta varios metodos a travez de fastapi.security: 

1. **OAuth2 (password flow):** El que estamos viendo. El usuario envia credenciales y recibe un JWT (Json Web Token).

2. **API key:** se envia una clave fija en los headers o en la URL (Comun en los servicios de mapas o clima).

    * from fastapi.security import APIKeyHeader

3. **HTTP Basic:** El navegador abre una ventana pidiendo rl usuario y clave (es el mas antiguo).

4. **OpenID Connect:** Para hacer "Login con Google" o GitHub.

