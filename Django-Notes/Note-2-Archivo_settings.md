# Manejo del archivo Settings.

Este archivo es el que contiene las configuraciones basicas del proyecto.

1. **Segiridad basica** 

    * **SECRET_KEY**: Es una cadena aleatoria para firmar secciones y Cookies, nunca se sube a GitHub, siempre se usa variables de entorno para almacenar estw dato.

    * **DEGUB**: 
        * True: Muestra errorres detallados para el desarrollo.
        * False: Para produccion, se encuentra en ALLOWED_HOST.

2. **Estructura y extwnciones**

    * **INTALLED_APPS**: La lista de modulos activos. Django es modular, si creas una app y no la pones, para Django no existe para el proyecto. Aqui tambien se ponen librerias externas.

    * **MIDDLEWARE**: Son capas de cebolla que la peticion atraviesa antes de llegar a tu vista. Manejan seguridad, seciones y autrnticacion.

3. *Codigo de ejemplo de muestra.*

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR/'db.sqlite3',
    }
}
```

Para el momento qie se suba a Neon.tech se tieen que cambiar a `postgresql` y se necesigan campos como NAME, USER, PASSWORD, HOST Y PORT.

4. **Archivos estaticos**

Django separa el codigo de python de los archivos que no cambian (CSS, JS, imagenes).

* `STATIC_URL`: La direccion en el navegador para acceder a ellos.

* `STATICFILES_DIRS`: Carpetas donde guardas tus archivos CSS/JS durante el desarrollo.

* `STATIC_ROOT`: La carpeta de Django "reune" todos los estatitcos cuando la carpeta va produccion (comando collectstatic).
