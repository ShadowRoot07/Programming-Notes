# Resumen de lo aprendido desde el 1 al 3.

1. **Entorno y estructura**: creacion de un env, entendimiento del proydcto y la app.

2. **Ciclo de vida (MVT)**: 

    * *URLs*: creacion de rutas estaticas y dinamicas con parametros.
    * *Views*: creacion de logica que devyelve texto simple a datos estructurados.

3. **Persistencia**:

    * Ejecucion de Migrate que transformo el codigo de Django a tablas SQL legibles.
    * Creacion de SuperUser, lo que significa que hay un registro real guardado en la base de datos.


* **Querysets (Tareas.objects.all())**: Es la forma en que Django hace un "SELECT" tipo consulta, es SQL escrito en python.

* **Serializacion Manual**: Transmormar un objeto completo de la base de datos a un objeto simple que se pueda convertir a texto JSON.

* **Formateo de fechas (strftime)**: Como JSON no sabe que es un objeto de fecha de python, se convierte a un string bonito.
