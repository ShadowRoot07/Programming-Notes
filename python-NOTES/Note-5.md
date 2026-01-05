# Condicionales en Python.

Pues, cre que muchoa sabemos que es un condicional, yo solo voy a coiar la sintaxis.

```python 
energia= 15

if energia > 80:
    print("El poekmon esta lleno de energia, A atacar!")
elif energia > 20:
    print("El poekmon esta cansado, pero puede luchar")
else:
    print("Cuidado, el pokemon debe ir al centro pokemon")
```

## Operadores de comparacion en Python:
* **==**: Igual a ...
* **!=**: Diferente de ...
* **> / <**: Mayor / Menor que ...
* **>= / <=**: Mayor o igual / Menor o igual que ...

## Operadores Logicos:

para combinar combinar varias condiciones en una sola linea. En Python se escriben con palabras, lo que lo hace muy legible:

```python
tiene_pocion= True
hp= 10

#AND: ambas deben de ser ciertas
if hp < 20 and tiene_pocion:
    print("usando la pocion automaticamente")

#OR: al menos una debe de ser cierta 
if hp==0 or tiene_pocion== False:
    print("Estas en problemas")

#NOT: invierte el valor
esta_vivo = True 
if not esta_vivo:
    print("Game over")
```

## Operador ternario: Condicionales en una linea.

Sirve para hacer mensajes cortos, es como un if / else pero muy abreviado:

```python
nivel=20
estado= "Evolucionado" if nivel >= 16 else "basico" #es como: ?: de JacmvaScript

print(estado) #Salida: Evolucionado
```

## El operador de pertenencia "in"

Este es muy util para hacer condiciones en estructuras de datos:

```python
equipo= ["Pikachu", "Eevee", "Ditto"]

if "Pikachu" in equipo:
    print("Pikachu esta listo para la batalla")
```

### Tema culminado
