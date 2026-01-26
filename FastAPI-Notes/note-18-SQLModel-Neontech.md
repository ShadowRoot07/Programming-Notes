# SQLModel & Neon.tech

## 1. **El stack tecnologico:**

|Libreria|Proposito|
|:---|:---|
|SQLModel|El nucleo del proyecto, combina Pydantic (validacion de datos) con SQLAlchemy (interaccion con SQL). Permite usar clases de Python con tablas.|
|SQLAlchemy|El motor bajo el capo que traduce el codigo Python a lenguaje SQL puro.|
|psycopg2-binary|El "driver" o el traductor especifico que permite que Python hable el dialecto de PostgreSQL|
|python-dotenv|Gestiona la carga de variables sensibles desde un archivo .env hacia el entorno del sistema.|
|Uvicorn|El servidor ASGI que pone a correr nuestra API de FastAPI|

## 2. **Arquitectura de archivos**(estandar profecional)

Para mantener el codigo limpio y escalable, dividimos las responsabilidades:

* **models.py:** Divide las estructuras, contiene las clases que heredan de SQLModel. Aqui se definen las relaciones (RelationShip) y las llaves foraneas (foreign_key).

* **database.py:** Define la conexion. Configura el engine (la tuberia) y el get_session (el flujo de datos).

* **main.py:** Define la logica de negocio. Contiene los endpoint de FastAPI y la inyeccion de dependencias.

* **.env / .gitignore: El bunker de seguridad.** primero guarda las llaves, segundo evita que esas llaves se suban a la red.

## 3. **Estandares de codificacion y variables**

1. **Nonmeclatura (PEP8):**

    * Clases (Modelos): PascalCase (Ej: HeroeEquipo).

    * Funciones y variables: snake_case (ej: def get_session(), heroe_id).

    * Constantes de entorno: UPPER_CASE (DATABASE_URL).

2. **Variables de entorno:** nunca harcodera credenciales. Se usa os.getenv("VAR") para recuperarlas.

3. **Inyecion de dependencias:** Usar Depends(get_session) en FastAPI asegura que cada peticion tenga su propia conexcion y que eata cierre correctamente al terminar, evitando fugas de memoria.

## 4. Neon.tech: PostgreSQL Serverless en la nube.

Hoy se aprendio que manejar una base de datos en la nube implica:

* **Conection strings:** una url que contiene un enlace que es la llave maestra dela conexion.

* **Seguridad de credenciales:** Si la url se filtra en un commit de git, la splucion es el Password Reset inmediato en el dashboard de Neon para invalidar la anterior.

* **Persistencia:** A diferencia de las pruebas locales, los datos de Neon viven es servidores de AWS, lo que permite que tu API sea consultada desde cualquier lugar del mundo.

* **SSL Mode:** Neon requiere conexiones seguras (?sslmode=require), lo que garantiza que los datos viajen cifrados.

## 5. Referencia de comandos CRUD.

Para interactuar con la API desde la terminal de Termux:

* **POST (crear):** `http POST :8000/heroes/ nombre="ShadowRoot" nombre_secreto="Admin" edad=25 equipo_id=1`

* **GET (leer todo):** `http GET :8000/heroes/`

* **PUT (actualizar):** `http PUT :8000/heroes/1 nombre="ShadowRoot" nombre_secreto="Admin" edad=25 equipo_id=1`

* **DELETE (Borrar):** `http DELETE :8000/heroes/1`

## 6. Logica de CRUD en SQLModel.

1. Create: session.add(objeto) -> session.commit() -> session.refresh(objeto).

2. Read: session.get(Modelo, id) para busqueda directa o session.exec(select(Modelo)) para listas.

3. Update: Buscar el objeto -> Modificar sus atributos en Python -> session.add(objeto) -> session.commit().

4. DELETE:;Buscar el objeto -> session.delete(objeto) -> session.commit.


