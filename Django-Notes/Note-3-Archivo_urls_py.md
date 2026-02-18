# El aechivo urls.py: Sistena de navegacion.

Si el `settings.py` es el ADN, el urls.py es el mapa de carreteras. Django usa un mapa de rutas jerarquicas.

**Estructura de una ruta**

La funcion path() recibe:

1. Ruta (string): El camino en la url.

2. Vista: La funcion o clase que se ejecuta.

3. Nombre (name): (Opcional pero vital) Un alias para la ruta para no hardcodear URLS en tus templates.

**Concepto de "include" (Practica recomebdada)**

No es bueno llenar urls.py del core con cientos de rutas. Lo ideal es que cada app solicite sus propieas rutas.

En `core/urls.py`:

```python
from django.urls import path, include

urlpatterns =[
    path('admin/', admin.site.urls),
    path('stasks/', include('tasks.urls')),
]
```

El tasks/url.py lo creas tu:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('lista/', views.lista_tareas, name= 'tasks_list'),
]
```

Forma de crear el views.py:

```python
from django.http import HttpResponse

def hola_mundo(request, nombre):
    return HttpResponse(f"<h1>Hola {nombre}!</h1><p>Bienvenido al calabozo</p>")
```

Forma de modificar el `core/urls.py`:

```python
from django.contrib import admin
from django.urls import path
from tasks.views import hola_mundo

urlpatterns= [
    path('admin/', admin.site.urls),
    path('hola/<str:nombre>/', hola_mundo),
]
```

Se probaria con este enlace: link[/hola/ShadowRoot/]
