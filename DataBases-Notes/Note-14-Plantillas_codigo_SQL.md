# Plantillas de codigo SQL.


## SQLite.

* Actualizar una columna de una tabla:

```sql 
ALTER TABLE nombre_de_la_tabla ADD COLUMN nombre_de_la_columna tipo_de_dato
```

* Uso de la clausula `WHERE`:

```sql
UPDATE nombre_de_la_tabla SET columna_modificar = nuevo_valor
WHERE id = valor_del_id;
```

* Pasos para modificar una tabla sin perder sus valores:

```sql
ALTER TABLE cursos RENAME TO cursos_viejos;
```

* Crear la tabla con la estructura deseada:

```sql
CREATE TABLE cursos (
    id INTEGER PRIMARY KEY,
    nombre_curso TEXT,
    estudiante_id INTEGER,
    FOREING KEY (estudiante_id) REFERENCES estudiantes(id)
);
```

* Pasar los datos de la vieja a la nueva tabla:

```sql
INSERT INTO cursos (id, nombre_curso) SELECT id, nombre_curso FROM cursos_viejos;
```

* Borrar la tabla vieja:

```sql
DROP TABLE cursos_viejos;
```

* Forma de borrar un registro especifico:

```sql
DELETE FROM nombre_de_la_tabla WHERE id= 10;
```

* Forma de insertar nuevos registros:

```sql
INSERT INTO cursos (nombre_curso, estudiante_id) VALUES ('Python Pro', 1);
```

* forma de cambiar un registro con el id:

```sql
UPDATE cursos SET estudiante_id = 3 WHERE id =5;
```

* Comando para encender los FOREING Keys en SQLite:

```sql
PRAGMA foreign_keys= ON;
```

* Forma de crear validacion dentro de la creacion de una columna:

```sql
CREATE TABLE productos (
    id INTEGER PRIMARY KEY,
    nombre TEXT,
    precio REAL CHECK(precio > 0) -- El precio debe de ser mayor a cero
);
```

* Otro tipo de condicion:

```sql
CREATE TABLE empleados (
    id INTEGER PRIMARY KEY,
    nombre TEXT,
    salario REAL,
    bono REAL,
    CHECK(bono < salario)
);
```

* Creacion de tablas temporales:

```sql
CREATE TEMP TABLE nombre_tabla (
    id INTEGER PRIMARY KEY,
    dato_importado TEXT
);
```

* Creacion de una tabla temporal a partir de una tabla exiatente:

```sql
CREATE TEMP TABLE temp_estudiantes AS SELECT * FROM estudiantes WHERE 0;
```

* Forma de importar la base de datos a csv:

```bash
.mode csv
.import datos_prueba.csv mi_tabla_temporal
```

* Manera de borrar una columna de una tabla:

```sql
ALTER TABLE nombre_de_la_tabla DROP COLUMN nombre_de_la_columna;
```

* Forma de crear un indice:

```sql 
CREATE UNIQUE INDEX idx_nombre On nombre_tabla(nombre_de_la_columna);
```

* Ver que columnas tiene el indice:

```sql
PRAGMA index_info('idx_nombre');
```

* Listar todos los indices de una tabla:

```sql
PRAGMA index_list(nombre_tabla);
```

* Manera de saber si el indice se esta usando:

```sql
EXPLAIN QUERY PLAN SELECT * FROM nombre_tabla WHERE nombre_de_la_columna = 'valor';
```

* Vaciar y resetear los IDs de una tabla:

```sql
DELETE FROM nombre_de_la_tabla;

-- paso dos 

DELETE FROM sqlite_sequence WHERE name ='nombre_de_la_tabla';
```

* Forma de apagar los foreign_keys:

```sql
PRAGMA foreign_keys= OFF;
```

* Forma de crear una tabla de foreign_keys para que si se borra el padre, los datos de conexioon de los hijos se borren automaticamente:

```sql
CREATE TABLE nombre_de_la_tabla (
    id INTEGER PRIMARY KEY,
    text_ej TEXT,
    key_conn INTEGER,
    FOREING KEY (key_conn) REFERENCES otra_tabla(id) ON DELETE CASCADE
);
```

* Manera de filtrar datos por rango:

```sql
SELECT id, nombre, precio
FROM productos
WHERE precio BETWEN 10 AND 50;
```

* Manera de ponerle limites a una conaulta:

```sql
SELECT columnas
FROM tabla
ORDER BY columna -- Es vital ordenat para que el salto sea coherente
LIMIT cantidad_a_mostrar
OFFSET cantidad_a_saltar;
```

* Manera de concatemar informacion de dos o mas columnas:

```sql 
SELECT columna1 || columna2 AS alias
FROM table;
```

* UPDATE con CASE, interacion para actualizar cierto atributo a la columna de una fila:

```sql 
UPDATE nombre_de_la_tabla 
SET nombre_de_la_columna = CASE id 
    WHEN 1 THEN '0123456'
    WHEN 2 THEN '6543210'
    WHEN 3 THEN '4561230'
END 
WHERE in IN (1, 2, 3);
```

* Otra manera:

```sql
UPDATE nombre_de_la_tabla SET
columna = '09922233' WHERE id =1;
```

* Forma de reemplazar el ID de una tabla:

```sql
INSERT OR REPLACE INTO nombre_de_la_tabla (id, nombre) 
VALUES (1, 'dato-insertar');
```

* Forma para que un atributo no se repita con el efecto UNIQUE:

```sql
CREATE TABLE usuarios (
    id INTEGER PRIMARY KEY,
    email TEXT UNIQUE,
    puntos INTEGER
);

-- si el correo ya existe, reemplaza el registro con los nuevos puntos
INSERT OR REPLACE INTO usuarios (emails, puntos) 
VALUES ('shadowRoot@example.com', 100);
```

* Manera de buacar registros vacios:

```sql
SELECT nombre_de_la_columna
FROM nombre_tabla
WHERE nombre_valor IS NULL;
```

* Manera de buacar registros si tienen datos:

```sql
SELECT nombre_de_la_columna
FROM nombre_de_la_tabla
WHERE nombre_valor IS NOT NULL;
```

* Manera de buscar palabras por longitud exacta:

```sql
SELECT nombre 
FROM nombre_de_la_tabla 
WHERE columna LIKE '_____'; -- 5 guienos bajos
```

    * froma de filtrar estas busquedas:

```sql
WHERE nombre LIKE 'A___';
```

```sql
WHERE nombre LIKE '__%s';
```

```sql
WHERE nombre LIKE '_%a';
```

* Alternativa usando la funcion `LENGTH()`:

```sql
SELECT columna 
FROM nombre_tabla 
WHERE LENGTH(columna) > 8;
```

* Manera de obtener la longitud de una columna numerica:

```sql
SELECT ABS(columna_numerica) AS valor_positivo FROM nombre_tabla;
```

    * Otro uso filtrando y convirtiendo un valor a positivo, ejemplo:

```sql
SELECT nombre, ABS(saldo) AS deuda_positiva
FROM estudiantes 
WHERE saldo < 0 
ORDER BY deuda_positiva DESC;
```

* Uso de `GLOB`:

```sql
SELECT nombre 
FROM estudiantes
WHERE nombre GLOB 'S*';
```

```sql
SELECT nombre 
FROM estudiantes 
WHERE nombre GLOB '[ABC]*';
```

```sql
SELECT nombre FROM estudiantes
WHERE nombre GLOB '*[0-9]';
```

* Forma de usar INNER JOIN:

```sql
SELECT estudiantes.nombre,
cursos.nombre_curso
FROM estudiantes
JOIN cursos ON estudiantes.id = cursos.estudiante_id;
```

* Ejemplo de JOINs con 3 tablas:

```sql
SELECT estudiantes.nombre AS Estudiante,
cursos.nombre_curso AS Curso,
inscripciones.fecha_inscripcion AS Fecha 
FROM estudiantes 
JOIN inscripciones 
ON estudiantes.id = inscripciones.estudiante_id
JOIN cursos ON 
inscripciones.curso_id= cursos.id;
```

* Ejemplo de LEFT JOIN:

```sql
SELECT 
    cursos.nombre_curso,
    COUNT(inscripciones.estudiante_id) AS cantidad_estudiantes
FROM cursos 
LEFT JOIN inscripciones ON 
cursos.id= inscripciones.curso_id
GROUP BY cursos.id;
```

* LEFT JOIN con WHERE:

```sql
SELECT cursos.nombre_curso
FROM cursos
LEFT JOIN inscripciones ON 
cursos.id = inscripciones.curso_id
WHERE inscripciones.curso_id IS NULL;
```

* Ejemplo pequenio de uso de subconsulta:

```sql
SELECT nombre_curso
FROM cursos
WHERE id NOT IN (SELECT curso_id FROM inscripciones);
```

* Uso de USING:

```sql
SELECT * FROM tabla_1
JOIN tabla_2 USING (id);
```

