# Metodos Http.

para qur una API funcione en un cliente que sea un movil, computadora, navegador, ect; debe decirle al servidor no solo a donde debe ir, sino que intencion tiene. Los metodos Http son los que miden esa intencion.

Aqui se hablaran de los mas basico y importante, lamados metodos **CRUD** *(Create, Read, Update, Delete)*.

## GET (Leer).

Es el mas comun, se usa unicamente para pedirle informacion al servidor, no modifica nada, solo consulta.

* Ejemplo: ver tu perfil de instagram o buscar un producto en amazon.

* En FastAPI: @app.get("/ruta/{id}").

## POST (Crear).

Se usa para enviarles datos nuevos al servidor para que este cree un registro. Los datos no van en la url, sino en el "cuerpo" (body) de la peticion por seguridad.

* Ejemplo: Registrar un nuevo usuario o crear un comentario.

* En FastAPI: @app.get("/usuarios/").

## PUT/PATCH (Actualizar).

Estos se usan para modificar algo que ya existe.

* PUT: se usa para reemplazar el recurso por completo.

* PATCH: Se usan para cambios parciales (Solo cambiar la foto de perfil).

* En FastAPI: @app.put("/item/{id}").

## DELETE (Borrar).

Su nombre lo dice todo, se usa para borrar informacion especifica.

* Ejemplo: Borrar un correo electronico o eliminar una foto.

* En FastAPI: @app.delete("/items/{id}").


## Resumen Rapido:

|**Metodo**|**Accion CRUD**|**Proposito**|
|:---|:---|:---|
|GET|Read|Obtener datos.|
|POST|Create|Enviar datos para crear algo nuevo.|
|PUT|Update|Actualizar un registro existente|
|DELETE|Delete|Eliminar un registro especifico por completo.|

## Como Probarlos todos?

Como el navegador hace peticiones GET por defecto cuando escribes una url, ahi entra una pagina magica, Swagger UI.

Los pasos son: entrar al enlace local del servidor y al final del enlace escribir "/docs", alli se veran botones de colores; Verde para POST, Azul para GET, Rojo para DELETE. FastAPI te permite darle al boton "Try it out" Para provar cada tipo de peticion sin salir del navegador. Es una practica recomendada con bajos recursos que proporciona FastAPI.
