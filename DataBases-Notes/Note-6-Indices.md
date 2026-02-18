# Indices

## Definion tecnica de indices.

Un indice es una estructira de datos fisica,m (normalmente B-Tree) que la base de datos mantiene de forma separada a la tabla para acelerar la busqueda de registros. Sin un indice, SQL tiene qie hacer un Full Table Scan (Leer fila por fila).

## CREATE INDEX y UNIQUE

Forma de usarlo:

```sql 
CREATE INDEX idx_post_titulo ON Posts(Ittulo);
```

**Le indice UNIQUE**

Cuando creas una restuccion, un indice o un PRIMARY KEY, SQLite crea un indice automaticamente. pero tambien se puede crear manualmente.

```sql
CREATE UNIQUE INDEX idx_autores_email ON Autores(Email);
```

* UNIQUE no solo acelera la busqueda, si no que este tampoco permite valores duplicados.

## En que momento usar Idices:

* En columnas que aparecen frecuentemente en el WHERE.

* Columnas que usas para hacer JOINs (claves foraneas).

* Columnasque se usan en el ORDER BY.

## Mantenimiento y Cardinalidad.

**Como saber si un indice tiene alta Cardinalidad?:**

La cardinalidad en indices se refiere a la cantidad de valores unicos en esa columna.

    * Alta cardinalidad: Muchos valores unicos, aqui los indices son mu efectivos.

    * Baja cardinalidad: Pocos valores repetidos muchas veces. Aqui los indices suelen ser inutiles, por que SQL prefiere leer la tabla completa.

**Mantenimiento:**

Se usa:

```sql
ANALYZE;
```

Ayuda al optimizador de busqueda a saber si los indices presentes siguen siendo utiles.

## Nombre de referencia (convenciones).

No hay reglas fijas, pero la convencion estandar dela industria es:

```sql
idx_[nombre_tabla]_[nombre_columna]
```

    * Ejemplo: `idx_posts_fecha`
    * Si es unico: `uidx_autores_email`

## Uso de DROP

El comando DROP se usa para eliminar estructuras permanentemente. No hay papelera de recilaje aqui, hay que tener precaucion.

    * Borrar indice:
```sql
DROP INDEX idx_posts_titulo;
```

    * Borrar tabla completa:

```sql 
DROP TABLE Posts;
```

    * Para evitar errores en Scripts usa **IF EXISTS** por seguridad.

```sql 
DROP TABLE IF EXISTS Autores;
```


