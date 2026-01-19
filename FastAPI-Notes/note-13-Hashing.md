# Hashing (Encriptacion).

Se mostraran metodos para encriptar con diferentes librerias auxiliares para FastAPI: PassLib, PyJWT.

## Como usar las librerias en conjunto.

Para que tu API sea segura, necesitas 3 herramientas que trabajen en equipo:

1. **PassLib:** Encripta la contrasenia para guardarla en la base de datos (Hashing).

2. **PyJWT:** Crea el token (la llave) que el usuario lleva en su celular.

3. **FastAPI (OAuth2):** El portero que revisa si el portero es valido.

## Metodos de Encriptacion (Hashing) con PassLib.

Nunca guardes contrasenias en texto plano. Usamos "CryptContext" de PassLib para manejar esto.

```python
from passlib.context import CryptContext

# Configuramos el motor de encriptacion (Bcrypt es el edtandar)
pwd_context= CryptContext(schemes= ["bcrypt"], deprecated="auto")

def obtener_hash(password: str):
    return pwd_context.hash(password)

def verificar_password(password_plana, password_hasheada):
    return pwd_context.verify(password_plana, password_hasheada)
```

## Creacion de Tokens con PyJWT.

En JWT es un string largo que contiene datos (como el nombre de usuario) y una firma secreta para que nadie pueda falsificarlo.

```python
import jwt 
from datetime import datetime, timedelta

SECRET_KEY= "tu llave super secreta" # No la compartas
ALGORITHM= "HS256"

def crear_token_acceso(data: dict, expires_delta: timedelta= None):
    datos_a_cifrar= data.copy()

    # El Token debe de inspirar por seguridad
    if expires_delta:
        expire= datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=15)
    datos_a_cifrar.update({"exp": expire})

    # Creamos el JWT
    token_jwt= jwt.encode(datos_a_cifrar, SECRET_KEY, algorithm= ALGORITHM)
    return token_jwt
```

## Ejemplo de Login Real en FastAPI

Aqui unimos todo el flujo en una sola ruta de login:

```python
from fastapi import FastAPI, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordRequestForm

app= FastAPI()

@app.post("/login")
def login(form_data: OAuth2PasswordRequestForm= Depends()):
    # 1. Buscar usuario en DB (Ejemplo manual).
    user_db= {"username": "gohan", "pass": obtener_hash("1234")}

    # 2. verificar contrasenia
    if not verificar_password(form_data.password, user_db["pass"]):
        raise HTTPException(
            status_code= status.HTTP_401_UNAUTHORIZED,
            detail="credenciales invalidas"
        )
    # 3. Generar Token
    acces_token = crear_token_acceso(data= {"sub": user_db["username"]})

    return {"acces_token": acces_token, "token_type": "bearer"}
```

## Resumen:

* **Encapsulamiento:** No necesitas entender toda la matematica de de Bcrypt, solo que `pwd_context.hash()` es de ida (no se puede desifrar) y `pwd_context.verify()` compara si la entrada es correcta.

* **Seguridad:** El SECRET_KEY ese corazon de tu sistema. Si alguien lo roba, puede crear Tokens de administrador falsos.

* **Interoperabilidad:** El Token generado por PyJWT puede ser leido por cualquier lenguaje  (Node, java, Go) Siempre que tenga la llave secreta.


