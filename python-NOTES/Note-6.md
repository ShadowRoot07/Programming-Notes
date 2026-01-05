# Bucles / Loops

## for

El bucle for es uno de los mas usados enprogramacion para lista y iterar datos de algo de forma constante:

*codigo de ejemplo*:

```python
pokemones=["Bulbasaur", "Charmander", "Squirtle"]

for p in pokemones:
    print(f"Yo te elijo {p}!")
```

* Funcion Range:

```python
#range(inicio, fin) -> el fin no se incluye

for i in range(1, 6):
    print(f"Vuelta al numero {i}")
```

## Bucle while

Esta se mantiene activa hasta que su condicion de activacion cambie, en mi opinion no sirve para iterar, si no mas bien para mantener algo funcionando mientras su condicion sea True o equivalente a su respectiva tarea:

*ejemplo*:

```python
hp=100

while hp > 0:
    print(f"HP actual: {hp}")
    hp-=20
    #es vital para cambiar la condicion para que no exista un bucle infinito

print("El pokemon se ha debilitado")
```

## Control de bucles: break, continue

A veces tu codigo requiere saltarese algo o parar algo en cierto momento, por eso existe estos controles de bucles:

* **break**: rompe el bucle por completo y sale de el.
* **continue**: se salta el resto de la vuelta actual y se pasa a la siguiente.

```python
#ejemplo de busqueda
numeros=[1, 2, 99, 4, 5]

for n in numeros:
    if n == 99:
        print("Encontrando el 99, deteniendo la busqueda")
        break #sale del for
    print(f"revisado {n}")
```

## Nested Loops (Bucles anidados).

es poner un bucle dentro de otro, se usa mucho para matrizes y coordenadas.

```python
for x in range(3): #Filas
    for x in range(3): #columnas
        print(f"Cordenada ({x},{y})")
```


