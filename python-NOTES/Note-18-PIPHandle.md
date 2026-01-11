# Manejo de **pip**

*pip* es el manejador de paquetes de python, es esencial saber su manejo y como funciona particularmente en tu maquina, en mi caso, yo uso termux, a si que solo veremos como manejarlo en termux y similares (linux...).

```bash
#Confirmar instalacion de python
pkg install python

#asegurarse de pip este actualizado
pip install --upgrade pip
```

## Sintaxis Basica de *pip*

|Comando|Descripcion|
|:--:|:---|
|pip install nombre|Instala una libreria|
|pip uninstall nombre|Desinstala una libreria|
|pip list|Muestra todo lo que tienes instalado|
|pip freeze > requirements.txt|Guarda tu lista de librerias en un archivo (estandar de proyectos)|
|pip show nombre|Muestra detalles de una linreria especifica|

## Librerias que yo voy a instalar

### Desarrollo web:

Para hacer sitios web robustos desde termux:

* **django**: El framework mas completo:  `pip install django`.

* **gunicorn**: Un servidor para poner tu web en produccion.

* **python-dotenv**: para manejar contrasenias y claves secretas de forma segura en archivos .env.

### Bases de datos

* **sqlite3**: (ya viene con python). Es perfecta para empezar, por que no requiere un servidor aparte (es un archivo .db)

* **psycopg2-binary**: si decides usar PostgreSQL.

* **mysql-connector-python**: Si prefieres MySQL.

### Inteligencia artificial (lo mas ligero para termux).

Hacer IAs en celular es dificil, pero estas librerias sirven para hacer modelos pequenios:

* **numpy**: la base de todo el calculo cientifico.

* **pandas**: para el analisis de datos y tablas.

* **scikit-learn**: Es la mejor para Machine Learning clasico (prediccione, clasificaciones), sin ser tan pesada como Deep Learning.

* **scipy**: Para calculos matematico avanzados.

*Nota de termux*: Algunas librerias IA como numpy, podrian tal vez fallar, usar el siguiente comando o consultar con tu IA de confianza: `pkg install build-essential python-numpy`.

## Entornoa virtuales, el mejor amigo de un programador.

A veces lo mejor es usar entornos virtuales en vez de intalar todo de forma global, esto se hace para que cada proyecto tenga su libreria sin mezclarse.

como crear uno en termux:

* 1. crear entorno: `python -m venv mi_entorno`

* 2. Activarlo: `source mi_entorno/bin/activate`

* 3. (notaras que el *prompt* cambia), ahora todo lo que intales en pip se queda solo en esa carpeta.

* 4. Salir: `deactivate`


