# Creacion y manejo de Servidor de nube MongoDB Atlas.

Aqui esta una Guia paso a paso:

## Como crear tu sevidor gratis:

Seguir los sigientes pasos desde el navegador:

1. **Registro:** Ve a [mongodb.com/atlas](url) y registrate.

2. **Crear proyecto:** Dale un nombre (ejemplo: proyecto-fastapi).

3. **Desplegar Cluster:** 

    * Elige el plan **MO** (FREE). Es gratis para siempre.

    * Selecciona un proveedor (AWS es el mas comun) y una region cercana a ti.

4.**Seguridad (IMPORTANTE)**: 
    
    * **Usuario:** Crea un usuario y contrasenia.

    * **IP Access List:** Para que puedas conectar desde termux o linux, se debe de agregar el IP actual. Si quiere que funcione desde cualquier red; agrega el IP 0.0.0.0/0 (aunque esto es menos seguro, es lo mas practico para pruebas).

## Conectar la nube a tu codigo local (en mi caso termux).

Una vez creado el Cluster, haz click en el boton "Connect" y elige "Drivers". Te dara una "Conection String" cadena de conexion, parecida a esta: 

    * mongodb_srv://tu_usiario:tu_password@cluster0.abcde.mongodb.net/?retryWrites=true&w=majority

**En tu codigo de Python:**

Ya no necesitas tener mongod corriendo en otra pestania de termux, por que ahora el servidor esta en la nube. Solo cambia la url en tu archivo en la base de datos:

```python
from pymongo import MongoClient

#Remplaza con la cadena que te dio MongoDB Atlas.
# 'OJO' Recuerda poner tu password rela en la url
URL_NUBE= "mongodb_srv://tu_usiario:tu_password@cluster0.abcde.mongodb.net/?retryWrites=true&w=majority "

cliente= MongoClient(URL_NUBE)

# A partir de aqui, todo lo demas es find, insert, ect... es exactamente IGUAL.

db= cliente["mi_base_de_datos"]
usuarios_col= db["usuarios"]
```

## Truco profecional:

Usa un archivo ".env" Para no estar cambiando la url a mano cada vez que hagas una interacion en python con el codigo:

```text
# Archivo .env
#Enlace para la nube
DATA_BASE_URL_CLOUD= mongodb_srv://tu_usiario:tu_password@cluster0.abcde.mongodb.net/?retryWrites=true&w=majority

#Enlace para la local
DATA_BASE_URL= mongodb://localhost:27017/
```

Y en tu codigo usas la libreria python-dotenv para leerla.


## Como visualizar los datos de la Nube:

Como en mi caso que solo tengo termux y este no tiene interfaz grafica, tengo dos opciones:


1. **MongoDB Atlas DashBoard**: En la misma pagina donde creaste el cluster, ve a la pestania "Browse Collections", ahi veras tis datos en tiempo real, con la interfaz bonita.

2. **Desde Termux:** Puede desde ahi usar el mismo comando mongosh pero pasando la url de la nube:

```bash
mongosh "mongodb_srv://tu_usiario:tu_password@cluster0. ..."
```

## Resumen de ventajas de usar la nube.

* **Adios al almacenamiento:** Ya no gastas almacenamiento en tu celular.

* **Disponibilidad:** Si borras termux o piedes tu celular, tus datos siguen vivos en Atlas.

* **Escalabilidad:** Si tu app crece, Atlas puede manejar millones de datos.
