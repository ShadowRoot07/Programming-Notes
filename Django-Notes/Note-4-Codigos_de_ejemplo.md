# Codigos de ejemplo.

**tasks/views.py:**

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

**core/urls.py:**

```python
from django.contrib import admin
from django.urls import path, include
from tasks.views import hola_mundo, lista_tareas
from tasks import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tasks.urls')),
    path('hola/<str:nombre>/', hola_mundo),
    path('api/tareas/', lista_tareas),
    path('test/<int:id>/', views.detalle_tarea),
]
```


