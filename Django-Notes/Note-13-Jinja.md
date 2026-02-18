# Jinja.

## Jinja loops.

Para recorrer datos de una lista, en lugar de usar python puro usamos templates:

```html
{% for tarea in mis_tareas %}
            <li>
                {% if tarea.completada %}
                    <span class="completada">{{ tarea.titulo }}</span> ✅
                {% else %}
                    <span class="pendiente">{{ tarea.titulo }}</span> ⏳
                {% endif %}

                <small>(ID: {{ tarea.id }})</small>
            </li>
        {% empty %}
            <p>No hay tareas registradas. ¡Hora de descansar!</p>
        {% endfor %}
```

## Jinja conditionals.

Sirven para mostrar contenido solo si se cumple una funcion.

```html
{% if tarea.completada %}
                    <span class="completada">{{ tarea.titulo }}</span> ✅
                {% else %}
                    <span class="pendiente">{{ tarea.titulo }}</span> ⏳
                {% endif %}
```

## Template Inheritance (herencia de plantillas).

Es para tener una plantilla para no copiar en cada archivo.


**tasks/templates/tasks/base.html:**

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}Mi App de Tareas{% endblock %}</title>
    <style>
        body { font-family: sans-serif; background: #f4f4f4; padding: 20px; }
        .container { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .completada { color: green; text-decoration: line-through; }
        .pendiente { color: #d9534f; font-weight: bold; }
        nav { margin-bottom: 20px; border-bottom: 1px solid #ccc; padding-bottom: 10px; }
    </style>
</head>
<body>
    <div class="container">
        <nav>
            <strong>ShadowRoot07 Code</strong> | <a href="/api/tareas/">Inicio</a>
        </nav>

        {% block content %}
        {% endblock %}
    </div>
</body>
</html>

```

**tasks/templates/tasks/lista.html:**

```html
{% extends "tasks/base.html" %}

{% block title %}Lista de Mis Tareas{% endblock %}

{% block content %}
    <h1>Mis Tareas Pendientes</h1>

    <ul>
        {% for tarea in mis_tareas %}
            <li>
                {% if tarea.completada %}
                    <span class="completada">{{ tarea.titulo }}</span> ✅
                {% else %}
                    <span class="pendiente">{{ tarea.titulo }}</span> ⏳
                {% endif %}

                <small>(ID: {{ tarea.id }})</small>
            </li>
        {% empty %}
            <p>No hay tareas registradas. ¡Hora de descansar!</p>
        {% endfor %}
    </ul>
{% endblock %}
```

Y por ultimo, definicion en el View:

tasks/views.py:

```python
from django.shortcuts import render
from django.http import HttpResponse, JsonResponse
from .models import Tarea
from django.shortcuts import get_object_or_404

# Añadimos 'nombre' como parámetro
def hola_mundo(request, nombre):
    # Usamos una f-string para insertar el nombre en el HTML
    return HttpResponse(f"<h1>¡Hola {nombre}!</h1><p>Bienvenido al calabozo de Django.</p>")


def lista_tareas(request):
    tareas = Tarea.objects.all()

    # Convertimos el QuerySet a una lista de diccionarios
    data = []
    for t in tareas:
        data.append({
            'id': t.id,
            'titulo': t.titulo,
            'completada': t.completada,
            'categoria': t.categoria.nombre if t.categoria else None
        })

    return render(request, 'tasks/lista.html', {'mis_tareas': tareas})



def detalle_tarea(request, id):
    print(f"DEBUG: Buscando ID {id} en la base de datos...")
    # Intentamos obtener la tarea. Si falla, Django lanza Http404 automáticamente.
    tarea = get_object_or_404(Tarea, id=id)

    data = {
        'id': tarea.id,
        'titulo': tarea.titulo,
        'descripcion': tarea.descripcion,
        'completada': tarea.completada,
        'categoria': tarea.categoria.nombre if tarea.categoria else "Sin categoría"
    }
    return JsonResponse(data)
```
