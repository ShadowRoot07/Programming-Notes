# Notas extras de SQL y Neon.tech con Alambic.

* **Revisiones vacias:** Si ejecutas --autogenerate y no hiciste cambios en los modelos, se creara un archivo casi vacio. Si te pasa, simplemente borra ese archivo de la carpeta versions/ para no ensuciar el historial.

* **Naming:** Intenta que los mensajes -m sean claros, como "add_phone_to_user" en lugar de "cambio_2". Te salvara la vida cuando tengas 50 archivos de migracion.

* **Sincronizacion:** Recuerda que si borras una mogracion manualmente de la carpeta versions/ pero ya la habias aplicado a la base de datos, Alembic se confundira. El historial de la carpeta y el debla db debe de coincidir.


