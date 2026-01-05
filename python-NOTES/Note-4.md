# Diccionarios

A mi parecer, los diccionarios son como los objetos en JavaScript, se usan recurentemente para lo que es mi objetivo, para crear datos json o objetos similares para la creacion de APIs en backend.

### Regla de oro: 
las llaves deben ser unicas y inmutables

```python
#Ejemplo de un pokemon representado en un diccionario 
pokemon{
    "nombre": "Pikachu",
    "tipo":"electrico",
    "nivel": 25,
    "es_legendario": false
}

#Acceder a un valor
print(pokemon["nombre"]) #Salida: Pikachu
```

## Operaciones esenciales

* Agregar o modificar:
    Es tan facil como asignar un valos a una llave:

```python
#Modificar
pokemon["nivel"]=26

#Agregar una nueva llave
pokemon["habilidad"]="Static"
```

En Django muchas veces recibiras diccionarios de la base de datos y querras mostrarlos.

```python
stats={"HP": 35, "Ataque": 55, "Defensa": 40}

#solo las llaves
for clave in stats.keys():
    print(clave)

#Solo los valores
for valor in stats.values():
    print(valor)

#Llave y valoral mismo tiempo (lo mas usado)
for clave, valor in stats.items():
    print(f"{clave}: {valor}")
```

## Diccionarios anidados (Estructura de APIs)

Como ya usaste la PokeAPI recordaras que los datos vienen unos dentro de otros.
En Python se ve asi:

```python
entrenador={
    "nombre":"pepe"
    "equipo":[
        {"nombre": "Bulbasaur", "nivel": 10}
        {"nombre": "Charmander", "nivel": 12}
    ]
}

#Como accedo al nivel del pokemon?
print(entrenador["equipo"][1]["nivel"]) #Salida: 12
```

### Tambien existe el metodo .get():
Muy importante.
