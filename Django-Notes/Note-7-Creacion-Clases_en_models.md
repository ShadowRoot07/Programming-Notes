# Creacion de clases en models.py

Cada clase es una tabla y cada atributo es una columna. Django te obliga a heredar de models.Model.

**tasks/models.py:**

```python
from django.db import models

class Categoria(models.Model):
    nombre = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre

class Tarea(models.Model):
    titulo = models.CharField(max_length=200)
    descripcion = models.TextField(blank=True)
    completada = models.BooleanField(default=False)
    fecha_creacion = models.DateTimeField(auto_now_add=True)

    categoria = models.ForeignKey(
        Categoria,
        on_delete=models.CASCADE,
        null=True,
        blank=True
    )

    PRIORIDAD_CHOICES = [
        ('B', 'Baja'),
        ('M', 'Media'),
        ('A', 'Alta'),
    ]
    prioridad = models.CharField(
        max_length=1,
        choices=PRIORIDAD_CHOICES,
        default='M',
    )

    class Meta:
        ordering = ['-fecha_creacion'] # El '-' significa descendente (la más nueva primero)
        verbose_name_plural = "Mis Tareas" # Para que el Admin no diga "Tareas" sino algo personalizado

    def __str__(self):
        return self.titulo

```

## El manejo de on_delete (Integridad referencial).

Cuando usas un ForeingKey (una relacion 1 a muchos), hay que crear una accion si Django le dice al objeto padre si se borra. Es el equivalente a ON_DELETE de SQL.

**Casos mas comunes**:

* CASCADE: Si borras el autor, se borran todos lo libros automaticamente. (Peligroso pero comun).
* PROTECT : No permite borrar el autor si todavia tiene libros asociados. Lanza un error de Integridad.
* SET_NULL: Si borras el autor, el campo en el libro queda como NULL (esto requiere que el campo acepte nulos).
* SET_DEFAULTT: pone un valor por defecto que tu definas.

```python
autor= models.ForeingKey('Autor', on_delete= models.CASCADE)
```
