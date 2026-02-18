# Anatomia de un projecto Django.

Aqui hay una explicacion clara de lo que hace cada pieza de un proyecto.

1. **Raiz del proyecto.**

    * **manage.py**: Es el panel de control. No se edita, se usa para correr el sevidor, crear migraciones, crear superUsuarios, ect. Es el intermediario entre tu y Django.

2. **Carpeta "/core"**(el cerebro).

    * `__init__.py`: un archivo vacio que le dice a python "oye, trata a esta carpeta como si fuera un paquete".

    * `settings.py`: El mas importante ahora. Aqui se define la base de datos, el idioma, la seguridad, las apps instaladas, ect.

    * `urls.py`: El indice de tu sitio. Aqui defines que direccion lleava a cual funcion.

    * `wsgi.p`/`asgi.py`: Son tuneles para que el servidor (Como Render) se comunique con Django. ASGI se usa si quieres cisas en tiempo real.

3. **Carpeta "/tasks"** (El muscululo, la App).

    * `migrations/`: Aqui se guardan los archivos que traducen de python a SQL.

    * `admin.py`: Aqui se administran los modelos que pueden aparecer en el panel de administrador de Django.

    * `apps.py`: configuracion interna de esta aplicacion especifica.

    * `models.py`: Aqui se disenian las tablas que Django convertira en SQL.

    * `test.py`: Para hacer pruebas y que el codigo no explote.

    * `views.py`: La logica. Aqui se escribe la funcion que recibe una peticion y lo devuelve a logica "html".
