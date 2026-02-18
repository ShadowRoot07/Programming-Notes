# SQLs, diferencias clave.

1. **PostgreSQL:**

```sql 
-- 1. Estructura y CRUD
CREATE TABLE productos (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    precio NUMERIC(10, 2) CHECK(precio > 0),
    stock INTEGER DEFAULT 0
);

-- Insertar
INSERT INTO productos (nombre, precio, stock) VALUES ('laptop', 1200.50, 10), ('Mouse', 25.00, 50), ('Teclado', 80.00, 15);

-- Actualizar
UPDATE productos SET precio = precio * 1.10 WHERE nombre = 'laptop';

-- Consulta completa
SELECT 
    nombre,
    stock,
    (precio * stock) * AS valor_total,
    CASE 
        WHEN stock < 20 THEN 'REABASTECER'
        ELSE 'OK'
    END AS estado_stock
FROM productos 
WHERE nombre LIKE 'L%' OR nombre IN ('Mouse', 'Teclado') 
GROUP BY nombre, precio, stock HAVING (precio * stock) > 50 
ORDER BY valor_total DESC;

-- SubQuery y filtrado avanzado
SELECT * FROM productos WHERE precio > (SELECT AVG(precio) FROM productos);
```

2. **MySQL:**

```sql
-- CRUD completo
CREATE TABLE clientes(
    cliente_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    registro DATE
);

CREATE TABLE pedidos(
    pedido_id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    monto DECIMAL(10, 2),
    FOREIGN KEY (cliente_id) REFERENCES clientes(cliente_id)
);

-- Inserciones rapidas

INSERT INTO clientes (nombre, registro) VALUES ('ShadowRoot', CURDATE()), ('Admin', '2024-01-15');
INSERT INTO pedidos (cliente_id, monto) VALUES (1, 500.00),
(1, 150.00),
(2, 50.00);

-- JOINs y agregaciones
SELECT
    c.nombre,
    SUM(p.monto) as total_gastado,
    COUNT(p.pedido_id) as num_pedidosFROM clientes c 
LEFT JOIN pedidos p ON c.cliente_id = p.cliente_id WHERE c.nombre NOT LIKE 'Admin%' -- LIKE
GROUP BY c.cliente_id 
HAVING total_gastado > 100 -- HAVING
ORDER BY total_gastado DESC;

-- Condicionales y calculos matematicos
SELECT 
    monto,
    CEIL(monto) as redondeo_arriba,
    FLOOR(monto) as redondeo_abajo,
    IF(monto, > 200, 'Premium', 'Regular') as categoria_venta
FROM pedidos;
```

