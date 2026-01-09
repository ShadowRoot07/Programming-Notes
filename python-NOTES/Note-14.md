# Funciones de orden superior

## Que son las funciones de orden superior

En Python, las funciones son ciudadanos de primera clase, esto quiere decir:

* 1. Puede ser asignada a una variable.
* 2. Pararse como argumento a ptra funcion.
* 3. Puede ser retornada por otra funcion.

Una funcion que hace el punto 2 o el punto 3, se le puede llamar que es una funcion de prden superior.

## Closures (clausuras).

Un Closure ocurre cuando una fincion interna "recuerda" el estado de su entorno superior, incluso despues de que esa funcion exterior haya terminado de ejecutarse.

```python
def exterior(x):
    def interior(y):
        return x+y
    return interior

sumar_cinco= exterior(5)
print(sumar_cinco(10))
# El resultado sera 15, el cinco estara guardado en el Closure 
```

## Funciones deorden superior propias de python.

Python tiene 3 funciones reinas para procesar datos de forma eficiente y tangible.

* 1. **map()**

aplica una funcion a cada elemento de;un iterable, y devuelve uno nuevo con;los resultados.

    * *Sintaxis*: map(funcion, iterable)

```python
numeros=[1, 2, 3, 4, 5]
#Elevar todos al cuadrado
al_cuadrado= list(map(lambda x: x ** 2, numeros))
#resultados: 1, 4, 9, 16, 25
```

* 2. **filter()**

Filtra los elementos de la lista segun su condicion (La funcion debe devolver un booleano "True" o "False").

    * *Sintaxis*: filter(funcion_condicion, iterable)

```python
edades=[12, 25, 17, 18, 30]
#solo mayores de edad 
adultos= list(filter(lambda x: x >= 18, edades))
#resultado: [18, 25, 30]
```

* 3. **reduce()**

A diferencia de las anteriores, esta no devuelve una lista, si no un unico valor. Va operando los elementos en parejas.

    * Nota: debes importarla de *functools*.

    * *Sintaxis*: reduce(funcion, iterable)

```python
from functools import reduce

numeros= [1, 2, 3, 4]
#sumar todos los numeros (((1+2)+3)4)
total= reduce(lambda a, b: a + b, numeros)
#El resultado es: 10
```

## Otras funciones importantes

| Funcion | Descripcion |
|:---:|:---|
| `sorted()` | Permite ordenar una *lista* usando el parametro *key* (ese parametro acepta una lambda) |
| `any()` | Retorna *True* si al menos uno de los elementos retorna una condicion. |
| `all()` | Retorna *True* si todos los elementos cumplen con la condicion. |

**Pequenio ejemplo de sorted con lambda:**

```python
usuarios[("Ana", 25),("Juan", 19),("Pedro", 30)]

#Ordenar por edad, el segundo elemento de la tupla.
ordenados= sorted(usuarios, key= lambda x: x[1])
```

## Importante:

*map* y *filter* devuelven "iteradores", por eso siempre los envolvemos en *list()* si queremos ver el resultado como una lista inmediatamente.
