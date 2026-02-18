# Ciclo de vida de las migraciones.

Las migraciones son el sistema de control de versiones de Django en la base de datos, es como alembic de FastAPI.

**makemigrations vs migrate**:

* makemigrations: lee tus archivos models.py, los compara con la version anterior y genera un archivo de intrucciones en su carpeta migrations/. No toca la base de datos todavia.

* migrate: lee los archivos y los ejecuta a SQL para que esten el la base de datos real.

## Cuando ejecutar "migrate"?

1. Al inicio del proyecto: para crear las tablas del sistema de Django.

2. Despues de un makemigrations: para que los cambios en el codigo se reflejen en la DB.

3. Al cambiar de rama en GIT: si un companiero aniado un modelo, debes de migrar para cambiar esa tabla.

4. A desplegar a produccion: Para que el servidor real tenga la misma estructura que tu local.
