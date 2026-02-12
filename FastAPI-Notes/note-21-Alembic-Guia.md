# Guia maestra de Alembic.

## 1. El concepto fundamental.

Alembic registra la evolucion de la base de datos. Cada cambio es un "commit" de la estructura, permitiendo avanzar (upgrade) o retroceder (downgrade) sin destruir los datos existentes.

## 2. Configuracion critica en env.py

Para que Alembic reconozca a SQLModel y use variables de entorno:

* Path: sys.path.insert(0, ...) para encontrar models.py.

* Metadata: target_metadata= SQLModel.metadata.

* Modelos: Se deben de importar las clases (ej: from models import Heroes, Equipo) para Alembic.

* Seguridad: Usar config.set_main_option("sqlalchemy.url"), os.getenv("DATABASE_URL")) para evitar credenciales en el .ini.

## 3. Comandos de supervivencia.

|Comando|Accion|Cuando usarlo|
|:---:|:---|:---|
|alembic revision --autogenerate -m ".."|crea un script de cambio|Al modificar models.py|
|alembic upgrade head|Aplica todos los cambios|Para sincronizar la DB con el codigo|
|alembic downgrade -1|Deshace el ultimo cambio|Si el cambio rompio algo o ocurrio un error|
|alembic current|Muestra la version actual|Para saber donde esta parada la DB|
|alembic history --verbose|Lista de todos los cambios|Para revisar la histpria del proyecto|

## 4. El fix de SQLModel.

Si alembic de error NameError: name 'sqlmodel' is not defined:

1. Abrir el script en migrators/versions/.

2. Aniadir import sqlmodel manualmente.

3. (Opcional) Aniadir import sqlmodel a migrators/script.py.mako para automatizarlo.

## 5. Regla de Oro de la soncronizacion.

* **Si haces upgrade:** El codigo ya tiene cambio, ahora la DB tambien.

* **Si haces donwgrade:** La DB ya No tiene el cambio, debes borrarlo manualmente del codigo (models.py) la coherencia.


