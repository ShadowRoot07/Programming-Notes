# Funciones

## Gramatica correcta

En python seguimos la convencion snake_case, todo en minusculas y separado por giones bajos.

## Parametros por defecto

Aveces quieres que una funcion funcione aunque no le pases datos de parametro, por eso le creamos un valor por defecto, ejemplo:

```python
def saludar_entrenador(nombre, saludo="Hola"):
    print(f"{saludo}, {nombre}")

saludar_entrenador("Pepe") #Salida: Hola, Pepe
saludar_entrenador("Pepe", "Buenas") #Salida" Buenas, Pepe
```

**importante**: Los parametro con valores pro;defecto deben ir al final de la lista de Parametros

## Parametros arbitrarios (*args, **kwargs).

Estos tipos de paranetros se usan cuando no sabes que tipos de datos te va enviar el ususario.

* **args*: Argumentos posicionales.
    El asterisco empaqueta todos los valores en una tupla.

* ***kwargs*: 
    Los dos asteriscos empaquetan los valores de un dicionario.

**Ejemplo**:

```python
def enlistar_capturas(*nombres):
    #nombres actua como una tupla
    for pokemon in nombres:
        print(f"Has capturado a {pokemon}")

enlistar_capturas("Pikachu", "Charmander", "Mewtwo")
#puedes pasarle 3, 10 o ningun nombre

def crear_perfil(**detalles):
    #detalles actua como diccionario
    for clave, valor in detalles.items():
        print(f"{clave.capitalize()}:{valor}")

crear_perfil(nombre="mr-papu", nivle=5, rango=maestro)

#no confundir print con return

def calcular_danio(ataque, defensa):
    return ataque - defensa

total= calcular_danio(100, 40) # total vale 60
```


