# El modulo *datetime*.

Pera usarlo, hay que importarlo, la forma de hacerlo es la siguiente:

```python
from datetime import datetime

#obtener fecha y hora excacta de este momento
ahora= datetime.now()

print(ahora) #El resultado es la fecha actual

print(ahora.year) #anio
print(ahora.month) #mes
print(ahora.day) # dia
print(ahora.hour) #hora 
print(ahora.minute) #minuto
print(ahora.second) #segundo
```

## Diferencia entre datetime(), date(), time().

Aunque parezcan iguales, tienen diferentes propositos:

| Funcion / clase | Para que sirve? | Ejemplo de uso |
|:---|:---|:---|
| datetime() | Fecha y hora completas | datetime(2026, 1, 15, 8, 30) |
| date() | Solo fecha (anio, mes, dia) | date(2026, 1, 8) |
| time() | Solo la hora (hora, minutos, segundos) | time(15, 30, 0) |

Par usar date y time de forma independiente, debes de importarlo asi:

```python
from datetime import date, time
```

## Operacipnes con timedelta()

Esta funcion es una de las mas utiles, noa sirve para representar la cantidad de distancia de una fecha de otra.

**Para que sirve?**: para representar la suma y resta de la distancia de una fecha de otra.

```python
from datetime import datetime, timedelta

hoy= datetime.now()

#crear un lapso de 5 dias
cinco_dias= timedelta(days=5)

#calcular el futuro y el pasado
fecha_futura= hoy + cinco_dias
fecha_pasada= hoy - cinco_dias
print(f"En cinco dias sera: {fecha_futura}")
```

## Formatear fechas

Para formatear las fechas a tu gusto, o de la forma estandar de las redes o calendarios se hace con strftime (string format time):

* ahora.strftime("%d/%m/%Y") -> 08/01/2026
* ahora.strftime("%H:%M") -> 16:50
