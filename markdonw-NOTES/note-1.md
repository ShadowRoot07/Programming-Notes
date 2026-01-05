#Sintaxis de markdonw

##1-Titulos (Jerarquia)

Usas el simbolo "#", cuantos mas pongas, mas pequenio es el titulo:

```markdonw
#titulo principal
##subtitulo (modulo de clases)
###Nota importante (atributos)
```

##2-Formato de texto

para resaltar ideas sin gastar tinta ni esfuerzo:

```markdonw
- **Negrita**: para conceptos clave.
- *Cursiva*: para terminos tecnicos.
- ~~Tachado~~: para cosas qie ya corregiste.
```

##3-Listas (ideales para apuntes)

para organizar tus ideas rapido:

```markdonw
* Concepto A
* Concepto B
    * Subelemento (un tabulador de espacio).
```

##4- Bloques de codigo

En lugar de copiar codigo en medio del texto, lo encierras en "backticks" (tilde invertida ```). En NeoVim es genial por que muchos plugins le dan color al codigo dentro del md:

```markdonw
(```python
def hola_mundo():
    print('Hello world')
```)
```

##5- Links y Tareas

* [Texto del link](url) para guardar enlaces a cosas importantes.

* - [ ] Tarea pendiente.

* - [x] Tarea tachada (Esto da mucha saticfaccion verlo lleno).
