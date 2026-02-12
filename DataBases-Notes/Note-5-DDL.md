# DDL (Data Definition Lenguage).

En los anteriores apuntes vimos las leyes de como pensar las tablas, ahora aplicaremos esas leyes, esto se hacen en un proyecto cuando se use un ORM.

1. La sentencia `CREATE TABLE`.

    El comando para definir la estructura, aqui defines nombre, tipos de datos y restricciones.

2. Restricciones (Constraints) - Las leyes de tu tabla.

    * `PRIMARY KEY`: El identificador unico.
    * `NOT NULL`: El campo no puede quedar vacio.
    * `UNIQUE`: No se puede repetir el valor
    * `DEFAULT`: Si no se pone nada, se pone un valor por defecto.
    * `CHECK`: una regka perszonalizada, ej: CHECK (Edad >= 18).
    * `FOREIGN KEY`: El vinculo con otra tabla.

```sql
-- 1. Crear la tabla de autores
CREATE TABLE Autores (
    AutorId INTEGER PRIMARY KEY AUTOINCREMENT,
    Nombre TEXT NOT NULL,
    Email TEXT UNIQUE NOT NULL 
);

-- 2. Crear la tabla de Posts
CREATE TABLE Posts (
    PostId INTEGER PRIMARY KEY AUTOINCREMENT,
    Titulo TEXT NOT NULL,
    Contenido TEXT,
    FechaCreacion DATETIME DEFAULT CURRENT_TIMESTAMP,
    AutorId INTEGER,
    FOREIGN KEY (AutorId) REFERENCES Autores(AutorId)
);
```


