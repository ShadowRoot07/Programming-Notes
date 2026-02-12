# SubConsultas

## Que es una sub-consulta ?

Una sub-consulta es simplemente una consulta SELECT dentro de otra consulta.

**Explicacion de niveles:**

Las sub-consultas son como muniecas rusas, se almacenan dentro de ellas mismas.

* *Nivel externo (Outer Query):* Es la consulta que devuelve el resultado final que vez en tu pantalla.

* *Nivel intermedio (Inner Query / SubQuery):* Es la consulta que se ejecuta primero para darle el resultado a la consulta externa.

*Regla de oro:* La sub-consulta debe de is siempre entre parentesis.

**Clasificacion de las SubConsultas**

No todas las sub-consultas son iguales, se clasifican por lo que devuelven:


1. Escaleras (un solo valor):

Devuelven una sola fila y una sola columna.

2. De columna (una lista de valores):

Devuelve una sola columna pero muchas filas, se usa mucho con el operador `IN`.

3. De tabla (Un conjunto de datos):

Devuelve varias filas fulas y varias columnas. Estas actuan como una tabla temporal.

## SubConsultas en el FROM.

Hacer esto no es leer solo tabla, es leer un conjunto de datos que el mismo programador que esta haciendo la consulta acaba de inventar.

**Para que sirve?** 

Para haver pre-filtros o calculos complejos antes de la consulta final. Son llamaddas tablas derivadas.

**Sintaxis y requisito del Alias**

En la mayoria de motores SQL, cuando haces el uso de esta practica debes de usar un alias a la sub-consulta, por que de lo contrario, SQL no sabe como llamar a esta tabla temporal.

Ejemplo practico:

```sql 
SELECT
AVG(ResumenAlbum.DuracionTotal) AS PromedioGeneral
FROM (
    -- esta es nuestra sub consulta que actua como tabla
    SELECT AlbumId, SUM(Milliseconds) AS DuracionTotal
    FROM Track
    GROUP BY AlbumId
) AS ResumenAlbum;
```

*Analizis del codigo:* 

1. La sub-consulta interna creo una tabla con dos columnas.

2. La consulta externa tomo esa tabla y aplico el promedio a la columna que creamos dentro.

**Mas Ejemplos**

```sql
SELECT Name 
FROM Artist AS a 
WHERE EXISTS (
    SELECT 1
    FROM Album as alb 
    WHERE alb.ArtistId = a.ArtistId
);
```

```sql
SELECT FristName, LastName
FROM Employee AS e1
WHERE EXISTS (
    SELECT 1
    FROM Employee AS e2
    WHERE e2.ReportsTo = e1.EmployeeID
);
```

Eso es todo con respecto a este tema.
