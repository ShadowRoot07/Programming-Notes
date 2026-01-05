# Tuplas

## Que es una **Tupla**?

Es una coleccion de elementos, que una vez creada, no se puede modificar. No aniadir, cambiar o eliminar elementos. En Python se define usando parentesis "()" en lugar de corchetes.

```Python
#creando una tupla 
punto_geografico= (10.48, -66.90)
pokemon_fijo= ("New", "Psiquico", 151)
```

## Operaciones con Tuplas:

Como no puedes modificarlas, solo tienes herramientas de lectura:

```python
colores_rgb= ("Rojo","Verde","Azul")

#Acceso igual a las listas
print(colores_rgb[0]) #Salida: Rojo

#Contar cuantas veces aparece un elemento
print(colores_rgb.count("Verde")) #Salida: 1

#Saber el indice de un elemento
print(colores_rgb.index("Azul")) #Salida: 2
```

## Desempaquetando (Unpacking)

Este es el "superpoder" de las tuplas que veras en mucho codigo avanzado en Python:

```python
#Tenemos una tupla con datos de un pokemon 
datos = ("Pikachu", "Electrico", 25)

#podemos asignar cada valor a una variable de una sola linea
nombre, tipo, nivel = datos

print(nombre) #Salida: Pikachu
print(nivel) #Salida: 25
```

### Eso es todo con el tema de las tuplas.
