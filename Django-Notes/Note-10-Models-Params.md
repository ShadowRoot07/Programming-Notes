# Models && Params.

Aunque ambos manejan datos, su proposito de ubicacion son totalmente distintos.

## Models **Persistencia**.

Es la representacion de una tabla en la base de datos de Python. Su funcion es la *persistencia*: que los datos sobrevivan cuando apagas el servidor.

* Se definen en models.py.
* Manejan: tipos de datos SQL (VARCHAR, INT, TEXT, ect.).

## Params **Comunicacion**.

Son los datos que viajan en una URL o en el cuerpo de una solicitud (Request). Su funcion es la *Comunicacion*: decirle al servidor que recurso especifico quiere el usuario.

* Se definen en: urls.py y se reciben en views.py.

* Tipos comunes en Django:

    * Path Params: api/tareas/<int: id>/ (para recursos especificos)
    * Query Params: api/tareas/?prioridad=Alta (para filtrar o buscar).
