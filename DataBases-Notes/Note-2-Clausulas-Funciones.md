# Note 2.

AS sirve para ponerle un nombre temporal a una columna o tabla.

**Uso en operaciones matematicas**

```sql
SELECT 
    Name,
    Milliseconds / 60000.0 AS Minutos
FROM Track
LIMIT 5
```

* "*" multiplicacion.
* "+" suma.
* "-" resta.
* "/" division.

## 2. Uso de ORDER BY

Sirve para ordenar tus reaultados. Por defecto si no pones nada, SQL ordena de forma ascendente.

* ASC (ascendente).

```sql
SELECT Name FROM Artist ORDER BY Name ASC;
```

* DESC (desendente).

```sql
SELECT Name, Milliseconds FROM Track ORDER BY Milliseconds DESC;
```

* NULLS FIRST / NULLS LAST:

Es para decidir si los valores vacios van al comienzo o al final;

```sql
SELECT Name, Composer FROM Track ORDER BY Composer ASC NULLS LAST;
```

* RANDOM():

Util para sacar una informacion filtrada al azar.

```sql
SELECT Name FROM Track ORDER BY RANDOM() LIMIT 1;
```

## Uso de "DISTINCT"

DISTINCT se usa para eliminar duplicados en tus resultados. Solo te devuelven los valores unicos.

**Ejemplo:** De que paises son mis clientes?...

```sql
SELECT DISTINCT Country
FROM Custtomer ORDER BY Country ASC;
```

## Operador BETWEEN (rangos).

```sql
SELECT Name, UnitPrice
FROM Track WHERE UnitPrice
BETWEEN 0.99 AND 1.99;
```

## Operador IN.

En lugar de usar muchos OR uss un IN.

```sql
SELECT * FROM Custtomer
WHERE City IN ('Oslo', 'Paris', 'Prage', 'Vienne');
```

## valores Nulos: IS NULL e NOT IS NULL

Hay que tener cuidado al usar esto, ya que SQL toma NULL como un valor.

## Las 5 funciones de agregacion (Las "Big Five").

Estas funciones toman un monton de valores y devuelven un solo valor.

1. COUNT() - Contador.

```sql
SELECT COUNT(*) AS TotalCanciones AS TotalCanciones FROM Track
```

2. SUM() - La sumadora.

```sql
SELECT SUM(Total) AS IngresosTotales FROM Invoice;
```

3. AVG() - El promedio (Average)

Calcula la media aritmetica.

```sql
SELECT AVG(Milliseconds) / 60000.0 As DurationPromedioMinutos FROM Track;
```

4. MAX() - El valor maximo.

Encuentra el valor mas alto, o la fecha mas reciente.

```sql
SELECT MAX(UnitPrice) FROM Track;
```

5. MIN() - El valor minimo.

Encuentra el valor mas bajo o la fecha mas antigua.

```sql
SELECT MAX(BirthDate) AS EmpleadoMasViejo FROM Employe;
```

## Regla de oro: GROUP BY

Las funciones de agregacion cobran vida cuando usas GROUP BY.

## Filtro de las agregaciones "HAVING".

Filtra el recurso despues de haber sido calculado.

```sql
SELECT ArtistId, COUNT(AlbumId) AS TotalAlbunes
FROM Album 
GROUP BY ArtistId 
HAVING TotalAlbunes > 10;
```
