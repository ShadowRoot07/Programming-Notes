# Como usar un From?

Para entrar antes hay que tenrr un entorno activo, y ejecutar:

```bash
python manage.py shell
```

Una vez dentro se vera el simbolo (>>>), de deb de importar los modelos, se usa la ruta app cuando se encuentre en la raiz del proyecto.

```bash
from tasks.models import Tarea, Catrgoria
```

* `from tasks.models`: Indica la carpeta 'tasks' y el archivo 'models'.

* `import tarea`: trae la clase especifica a usar.

Manera de crear un registro:

```bash
# paso 1: Crear la instabcia en memoria
nueva_tarea= Tarea(titulo="Aprender Shell", descripcion="Practicando comandos")

# paso 2: guardar en la base de datos (aqui se ejecuta el insert de SQL)
nueva_tarea.save()
```

Si quieres hacer todo en un solo paso, usamos `create()`:

```bash
Tarea.objects.create(titulo="Tarea rapida", descripcion="Sin usar save()")
```

*consultas con `all()` y otros metodos*:

El objeto .objects es el manager de Django, es el encargado de generar el SQL.

* Traer todo "SELECT":

```bash
todas= Tarea.objects.all()
print(todas)
```

* filtrar ("SELECT" y "WHERE"):

```bash
# trae todas las cimpletadas
completadas= Tarea.objects.filter(completada= True)
```

* Obtener un solo registro:

```bash
# Si no existe o hay varios, dara error. Buscar por ID.
tarea_especifica= Tarea.objects.get(id=1)
```
