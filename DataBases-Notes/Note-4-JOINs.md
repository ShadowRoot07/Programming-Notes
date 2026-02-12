# JOINs

## Explicacion Teorica y tecnica:

* **Teorica:** Un JOIN es una operaciom que vinvula dos o mas tablas basandose en datos de una columna comun de esta (usualmente una llavr forania), sirve para reconstruir la informacion que normalizamos previamente.

* **Tecnica:** El motor de la base de datos toma las filas de la "tabla A" y busca oincidencias de esta en la "tabla B" segun un predicado. Si vay una coincidencia, combina las columnas de ambas en una fila de resutado.

## Los 4 tipos de JOINs + CROSS.

1. `INNER JOIN` el mas comun.

Solo devuelve filas cuando hay una coincidencia en ambas tablas.

    * Ejemplo basico:

```sql
SELECT Artist.Name, Album.Title
FROM Artist 
INNER JOIN Album ON 
Artist.ArtistId = Album.ArtistId
```

2. `LEFT JOIN` (o `LEFT OUTER JOIN`).

Devuelve todas las filas de la tabla de la izquierda, y devuelve las coincidencias de la derecha, si no existen coincidencias pone NULL.

    * Ejemplo:

```sql
SELECT Artist.Name, Album.Title
FROM Artist 
LEFT JOIN Album ON 
Artist.ArtistId = Album.ArtistId;
```

3. `RIGTH JOIN` y `FULL JOIN` (RIGTH JOIN no existe en SQLite).

    * `RIGTH JOIN`: Es lo opuesto a LEFT.
    * `FULL JOIN`: Devuelve todo de ambas, con NULLs si no hay cruce.

**Detalles...** 
    * para RIGTH JOIN simplemente inviertes el orden de las tablas en un LEFT JOIN.
    
    * Para FULL JOIN se usa UNION.

4. `CROSS JOIN` (Producto cartesiano).

Combina cada fila debla "tabla A" con las filas de la "tabla B". Si tienes 10 artistas y 10 Generos, el resultado son 100 filas.

```sql
SELECT Artist.Name, Genre.Name 
FROM Artist CROSS JOIN Genre;
```

## Formas de hacerlo: Implicita vs Explicita.

Es vital conocer ambas, aunque la Explicita es el estandar definido actualmente.

**Forma Implicita (Old School)**

No usa la palabra JOIN, si no que lista las tablas `FROM` y filtra el `WHERE`.

```sql
SELECT Artist.Name, Album.Title
FROM Artist, Album
WHERE Artist.ArtistId = Album.ArtistId;
```

    * Desventaja: Es facil olvidar el WHERE y crear un CROSS JOIN accidental que bloquee el servidor.

**Forma Explicita (Recomendada)**

Usa la palabra clave `JOIN` y la clausula `ON`.

```sql
SELECT Artist.Name, Album.Title
FROM Artist 
JOIN Album ON Artist.ArtistId = Album.ArtistId;
```

    * Ventaja: Mas legible, separa la logica de union (ON), de la logica de filtrado(WHERE).

## Ejemplos complejos:

```sql
SELECT
    ar.Name AS Artist,
    al.Title AS Disco,
    t.Name As Cancion,
FROM Artist As ar JOIN Album AS al ON 
ar.ArtistId = al.ArtistId
JOIN Track AS t ON al.AlbumId = t.AlbumId
WHERE t.UnitPrice > 0.99 
ORDER BY ar.Name ASC;
```

```sql
SELECT Artist.Name, Album.Title
FROM Artist LEFT JOIN Album ON 
Artist.ArtistId = Album.ArtistId
UNION 
SELECT Artist.Name, Album.Title 
FROM Album LEFT JOIN Artist ON 
Album.ArtistId = Artist.ArtistId;
```
