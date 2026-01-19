# Recursos estaticos

Los estaticos son archivos que el servidor envia al cliente tal cual estan, sin procesar la logica de Python. Hablamos de imagenes, archivos CSS, documentos pdf o archivos de JavaScript para el frontend.

En FastAPI para manejar esto, necesitamos una libreria llamada StaticFiles(que viene incluida con FastAPI/Startlette).

# Preparar la estrutura.

Antes de escribir el codigo, necesitamos una carpeta fisica donde van a vivir estos archivos.

```bash
mkdir static/
#opcional: mete una imagen de prueba ahi 
#twrmux dowland-manager puede ayudarte o simplemente crea un txt
echo "Hola, desde un archivo estatico" > static/hola.txt
```

## Como exponer Recursos Estaticos (Codigo).

Ora "exponer" o "montar" la carpeta, usamos el metodo mount. Esto le dice a FastAPI: "Cualquier peticion que empieze con /static debe de buscarse dentro de la carpeta fisica static".

```Python
from fastapi import FastAPI 
from fastapi.staticfiles import StaticFiles

app = FastAPI()

#Montaje del recurso estatico
#1. path: el nombre que vera el usuario en la url (ej: static/foto.jpg)
#2. app: la instancia de StaticFiles apuntando tu carpeta fisica.
#3. name: un nombre interno para usarlo como url_for si fuera necesario
app.mount("/static", StaticFiles(directory="static"), name="static")

@app.get("/")
def home():
    return {"mensaje": "API funcionando, ve a /static/hola.txt para ver el contenido del archivo"}
```

## Como acceder a los recursos?

si tu servidor esta corriendo en el puerto "8000", las rutas quedarian asi:

* **URL**: http://127.0.0.1:8000/static/hola.txt -> Veras el texto

* **URL**  http://127.0.0.1:8000/static/imagen.png -> El navegador mostrara la imagen

## Puntos claves.

1. **Seguridad:** Ten cuidado. Todo lo qur metas en la carpeta static sera publico. Nunca metas archivos ".env" o codigos con contrasenias ahi.

2. **Uso comun:** Se usa mucho para el favicon.icon (el icono de la pestania del navegador) o cuando estas haciendo un web sencilla y quieres y quieres cargar tu propio archivo CSS.

3. **Multiples Carpetas:** Puedes montar tantas carpetas como quieras. por ejemplo:

```Python
app.mount("/assets", StaticFiles(directory="assets"), name="assets")
app.mount("/descargas", StaticFiles(directory="public/dowlands"), name= descargas)
```

Una prueba rapida seria: 

* Crear un archivo static/test.html con cualquier texto.

* Monta la carpeta en tu main.py .

* Entra a: http://127.0.0.1:8000/static/test.html .
