# Tipos de Errores comunes en Python

Entender todos los tipos de errores comunes de un lenguaje de programacion y saberlos manejar, es una abilidad indispemsable para el manejo y la creacion y construccion de una app o programa.

## 1. Tipos de Errores comunes:

| **Error** | **Cuando aparece?** | **Ejemplo rapido** |
|:---:|:---|:---|
| SyntaxError | Cuando escribes mal la estructura de un elemento del lenguaje, cuando falta un ":", un "(", ect. | `if tru
    print("hola")
    #falta el ":"`|
| IndentationError | Un error de espacios o tabulaciones, Python es muy estricto con esto | olvidar sangrar codigo dentro de un "def" |
| NameError | Intentas usar una variable o una funcion que no ha sido definida. | `print(variable_que_no_existe)` |
| TypeError | Operacion entre tipos incompatibles. | `"hola"+5 #sumar texto mas numeros` |
| IndexError | Intentas acceder al indice de una lista que esta fuera de rango. | `lista=[1, 2]
print(lista[5])` |
| KeyError | Intentas acceder a la clave de un diccionario que no existe. | `dict["clave_falsa"]` |
| ValueError | El tipo de dato es correcto, peroel valor es inapropiado. | `int("perro")`
en verdad espera texto con numeros |
| AttributeError | Intentas usar un metodo que no tiene. | `numero= 10;
numero.upper()`|
| ZeroDivisionError | Cuando intentas dividir cualquier numeros entre 0. | `10 / 0` |
| ImportError | Cuando intentas importar un modulo que no esta instalado o no existe. | `import modulo_fantasma` |

## 2. Como leer un Error (Traceback)

Cuando Python te lanza un error te devuelve un "Traceback"(seguimiento), hay que leerlo de abajo hacia arriba:

* 1. La ultima linea: te dice el tipo de error y una breve descripcion.

* 2. La linea anterior: te dice en que archivo y en que linea ocurrio el problema.

## 3. Detalles tecnicos:

* **raise:**

comunesPuedes provocar un error manualmente usando `raise NameError("Mensaje personalizado")`, es util para validar datos y detener el programa usando logica propia.

* **Jerarquia:**

Todos los errores heredan de una clase llamada `BaseException`
