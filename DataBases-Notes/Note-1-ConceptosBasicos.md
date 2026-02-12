# Conceptos basicos

* **columna (campo):** Es la categoria de informacion.

* **Fila (Registro):** es un objeto completo.

* **Valor de dato:** El dato especifico.

## 2. Tipos de campos:

1. INTEGER.

2. TEXT.

3. REAL.

4. BLOB.

5. NULL.

## 3. Crear una tabla, campos y default.

```sql
CREATE TABLE usuarios (
    id INTEGER PRIMARY KEY AUTOINCREMENT, -- Clave unica que crece sola
    nombre TEXT NOT NULL, -- No puede estar vacio
    alias TEXT DEFAULT 'sin alias', -- si no pones alias, sale esto
    puntos INTEGER DEFAULT 0, -- default numerico
    fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- fecha automatica
);
```

## 4. Insertar y ver Registros (Filas).

Ver como finciona default.

```sql
-- Insertando un registro completo
INSERT INTO usuarios (nombre, alias, puntos) VALUES ('ShadowRoot', 'Shadow', 100);
-- Insertando un registro usando DEFAULTS (solo ponemos el nombre)
INSERT INTO usuarios (nombre) VALUES ('Pepito');
```

Para el resultado:

```sql
SELECT * FROM usuarios
```

## 5. Para verlo en VistaData:

```bash
vd tienda.db
```

## 6. Como Insertar valores insert:

* A. Insertar un solo registro:

```sql
INSERT INTO productos (nombre, precio, stock)
VALUES ('Teclado mecanico', 45.50, 10)
```

* B. Insertar varios datos a la vez:

```sql
INSERT INTO productos (nombre, precio, stock) VALUES
('mause gamer', 25.00, 15),
('Monitor 4K', 300.99, 5),
('Cable HDMI', 12.00, 50),
('Laptop Pro', 1200.00, 2)
```

## 7. Como hacer una consulta. (El famoso SELECT)

```sql
SELECT * FROM productos
-- consulta basica
-- trae todas las columnas = "*"
```


## 8. Como hacer una consukta SELECT precisa?...

* A. Seleccionar columnas especificas.

```sql
SELECT nombre, precio FROM productos;
```

* B. Filtrar con la clausula "WHERE".

```sql
-- por igualdad:
SELECT * FROM productos WHERE precio = 12.00;

-- por comparacion
SELECT * FROM productos WHERE precio > 100;

-- por texto parcial (LIKE)
SELECT * FROM productos WHERE nombre LIKE 'M%';
```

*(el '%' es un comodin que significa "cualquier cosa despues de la M").*

* C. Combunar filtros con "AND" y "OR".

```sql
SELECT * FROM productos WHERE precio > 20 AND stock > 10;

SELECT * FROM productos WHERE precio > 20 or stock > 10;
```

* D. Ordenar los resultados (ORDER BY).

```sql
SELECT * FROM productos ORDER BY precio ASC;

-- ASC es de menor a mayor, DESC es de mayor a menor.
```

## 9. Identificadores.

* 1. Naturales

* 2. Artificiales

**AutoIncrement:** Para que sirve?, como ponerlo?

Sirve para que el programador no tenga que insertar el numero ID manualmente cada vez que registre algo o a alguien.

```sql
id INTEGER PRIMARY KEY AUTOINCREMENT
```

## 10. Claves Foraneas (Foren Keys) y relaciones.

Aqui es donde ocurre la magio de relacionar.
Una clave foranea es simplemente una columna en una tabla que hace referencia a la clave primaria de otra tabla.

**Como relacionar tablas?**

1. Crear la tabla principal:

```sql
CREATE TABLE clientes (
    id_cliente INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT

);
```

2. Crear tabla relacionada:

```sql
CREATE TABLE pedidos (
    id_pedido INTEGER PRIMARY KEY AUTOINCREMENT,
    producto TEXT,
    cliente_id INTEGER, -- Este es el campo que guarda el ID del cliente 
    FOREING KEY (id_cliente) REFERENCES clientes(id_cliente)
);
```

**Regla de oro:** Por defecto SQLlite tiene las llaves foraneas apagadas por velocidad.

```sql
PRAGMA foreing_keys = ON; 
```

## 11. Como eliminar todos los registros?

Hay dos formas:

**A. Borrar todo el contenido (pero dejar la tabla vacia)**

```sql
DELETE FROM nombre_de_la_tabla;
```

*Ojo: esto borra todos los datos, pero si el ultimo ID era 10, es siguiente que metas sera el ID 11.*

**B. Borrar todo y rastrear el contador (Truncate)**

En otros SQLs existen TRUNCATE, pero SQLlite lo mas limpio es borrar y luego limpiar la tabla de secuencias.

```sql
DELETE FROM nombre_de_la_tabla;
DELETE FROM sqlite_sequence WHERE name='nombre_de_la_tabla';
```

**C. Borrar la tabla por completo**

Si te equivocas diseniandola y quieres empezar desde cero:

```sql
DROP TABLE nombre_de_la_tabla
```

## 12. Exploracion inicial (Comandos del sistema).

* *.tables*: Muestra todas las tablas disponibles.

* *.schema Track*: Te muestra como se creo la tabla "Track" (Su columna y sus tipos de datos).

* *.header on*: Activa los nombres de las columnas en los resultados.

* *.mode colum*: Alinea los datos en las columnas para que se vean alineados en la terminal.
