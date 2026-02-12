# Schemas avanzados y seguridad.

## 1. Modelado inteligente (Pydantic + SQLModel).

Implementemos la estructura de 4 capas para para evitar fugas de datos de validaciones y error:

* **Base:** Campos comunes.

* **Create:** Para escribir datos sin ID.

* **Read:** Para enviar datos con ID.

* **Relational (ReadWith ...):** Para respuestas anidadas (Heroe + Equipo).

## 2. Validacion automatica.

Usamos `Field(gt=0, lt=1000)` y min_length para que la API rechace datos basura antes de que lleguen la base de datos de Neon.

## 3. Seguridad con API keys.

Protegimos los endpoints sensibles (POST, DELETE, PATH) mediante un Header Security.

* **Header:** X-API-KEY

* **Mecanismo:** Dependencias de FastAPI (Depends).

* **Estado:** Solo usuarios autorizados pieden modificar la base de datos.

## 4. Documwntacion interactiva.

Configuramos **Swagger UI** (/docs) para probar la API de forma visual, incluyendo la autenticacion mediante el boton "Authorize".
