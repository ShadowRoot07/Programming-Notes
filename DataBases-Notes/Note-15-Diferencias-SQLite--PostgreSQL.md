# Diferencia y sintaxis de PostgreSQL.

**Comandos**

1. \conninfo: Te dira a que base de dqtps esta conectado, bajp que usuario y en qie host.

2. \dn: Lista los esquemas. En PostgreSQL las tablas viven dentro de los esquemas.

3. \dt: Lista las tablas actuales.

4. \q: Para salir de la consola cuando termines.

**Cosas nuevas**

* `SERIAL`: Auto-incremental inteligente.

* `VARCHAR(100)`: Mas eficiente que el TEXT, generico para strings con limite.

* `DECIMAL(10, 20)`: Precision excata para calucuolos o dinero.

* `CHECK(precio > 0)`: una restriccion de integridad que impide meter precios negativos por error.

* `TIMESTAMPTZ`: Da la fecha, hora y zona horaria.

**Codigo**

* Creacion de una tabla:

```sql 
CREATE TABLE vemtas_pro(
    id SERIAL PRIMARY KEY,
    producto VARCHAR(100) NOT NULL,
    precio DECIMAL(10, 2) CHECK(precio > 0),
    cantidad INTEGER DEFAULT 1,
    fecha_venta TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);
```

* Insertar datos:

```sql
INSERT INTO ventas_pro (producto, precio, cantidad) VALUES 
('Teclado Mecanico', 85.50, 10),
RETURNING id, fecha_venta;
```

* Forma de agregar una nueva columna a una tabla:

```sql
ALTER TABLE ventas_pro ADD COLUMN en_stock BOLEAN DEFAULT true; 
```

* Manera de crear una reatriccion de unicidad:

```sql
ALTER TABLE ventas_pro ADD CONSTRAINT producto_unico UNIQUE (producto);
```

* Uso de ON CONFLICT:

```sql
INSERT INTO ventas_pro (producto, precio, cantidad) VALUES ('Teclado Mecanico', 85.50, 5)
ON CONFLICT (producto)
DO UPDATE SET cantidad = ventas_pro.cantidad = EXCLUDED.cantidad;
```

* Busqueda de texto con ILIKE:

```sql
SELECT * FROM ventas_pro 
WHERE producto ILIKE '%TECLADO%';
```

* Forma de crear un nuevo esquema:

```sql
CREATE SCHEMA rrhh;

CREATE TABLE rrhh.empleados(
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(50),
    puesto VARCHAR(50)
);
```

* Forma de configurar la configurar la sesion para usar un esquema por defecto:

```sql
SET search_path TO rrhh , public;

SELECT * FROM empleados;
```

* Uso de WHERE y maneras de filtrar c-nsultas:

```sql
SELECT producto, fecha_venta FROM public.ventas_pro 
WHERE fecha_venta > CURRENT_TIMESTAMP - INTERVAL '30 minutes';
```

* Forma de manejar y crear vistas:

```sql 
CREATE VIEW reporte_stock AS 
SELECT
    producto,
    cantidad,
    (precio * cantidad) AS valor_inventatio
FROM public.ventas_pro
WHERE en_stock =true;

-- para consultarla:
SELECT * FROM reporte_stock;
```

* Consulta de fecha pero con un formato determinado por el programador:

```sql
SELECT 
    producto,
    TO_CHAR(fecha_venta, 'DD/MM/YYYY - HH24:MI') AS fecha_formateada 
FROM public.vemtas_pro;
```

* Practica para guardar datos grandes usando JSONB:

```sql
ALTER TABLE ventas_pro ADD COLUMN detalles JSONB;

-- Insertar el registro con datos JSON 
UPDATE ventas_pro 
SET detalles= '{"color": "negro", "swicht": "blue", "wireless": true}'
WHERE producto ILIKE '%Teclado%';
```

* manera de Consultar datos JSONB:

```sql
SELECT producto, detalles->>'color' AS color 
FROM ventas_pro 
WHERE detalles->>'wireless' ='true';
```

* manera de crear funciones personalizadas:

```sql
CREATE OR REPLACE FUNCTION calcular_iva(precio_decimal) RETURNS DECIMAL AS $$ BEGIN 
    RETURN precio * 1.16;
END;
$$ LANGUAGE plpgsql;

SELECT producto, precio, calcular_iva(precio) AS precio_con_iva FROM ventas_pro;
```

* Manera de crear Triggers:

```sql
CREATE TABLE auditoria_ventas (
    id SERIAL PRIMARY KEY,
    mensaje TEXT,
    fecha TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,
);

CREATE OR REPLACE FUNCTION log_nueva_venta() RETURNS TRIGGER AS $$ BEGIN
    INSERT INTO auditoria_ventas(mensaje)
    VALUES ('Se inserto el producto'|| NEW.producto);
    RETURN NEW;
END;

$$ LANGUAGE plpgsql;
```

* Creacion de indices:

```sql
CREATE INDEX idx_producto_nombre ON
ventas_pro(producto);

-- para verlos indices
-- \d ventas_pro
```

* Manera de crear ub indice para JSONB l:

```sql
CREATE INDEX idx_detalle_jsonb ON 
ventas_pro USING GIN (detalles);
```

* Restricciones de dominio, para crear un tipo de datos personalizado:

```sql
CREATE DOMAIN email_valido AS TEXT CHECK (VALUE ~* '^[A-Za-z0-9._%-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2, 4}$');

CREATE TABLE clientes_pro(
    id SERIAL PRIMARY KEY,
    nombre TEXT,
    correo email_valido
);

-- INSERT INTO clientes_pro (nombre, correo) VALUES ('Shadow', 'correo falso')
```

* Forma de usar EXPLAIN ANALYZE, es la forma de saber cuanto realmente a tardado tu consulta y si realmante usa los indices que has indicado.


```sql
EXPLAIN ANALYZE
SELECT * FROM ventas_pro WHERE producto= 'Monitor 4K';
```
