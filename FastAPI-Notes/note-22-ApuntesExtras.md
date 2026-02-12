# Apuntes extras.

## 1. El archivo requirements.txt

Render necesita saber que librerias instalaste en tu termux. Se crea con este comando en el terminal:

```bash
pip freeze > requirements.txt 
```

## Background Tasks:

Permite delegar procesos que requieren respuesta inmediata al cliente, mejorando la latencia percibida (Response Time). Se ejecutan despues de que la respueata ha sido enviada.

## Resumen:

* **Background Tasks** es una clase de fastAPI que maneja para nosotros.

* **.add_task()** recibe el nombre de la funcion (sin parentesis) y luego los argumentos que eata funcion necesita.

* **Independencia**: si la funcion de fondo falla (por ejemplo, si el disco esta lleno), el usuario no recibe el error 500, por que su peticion ya se cerro con exito.

## Vocabulario tecnico:

* **Enviroment Variables: (variables de entorno):** Son los datos sencibles (como la clave de la base de datos) qur nunca subes a GitHub. Se configuran directamente en el panel del servidor (Render). **Nuca subir el .env a GitHub**

* **Deployment (Despliegue):** El proceso de llevar tu codigo de "mi maquina" a "una maquina de internet".

* **EndPoint URL:** la direccion publica de tu API.

* **Continuous Deployment (CD):** Cada vez que se haga "git push", Render actualiza tu API automaticamente.

## Que hacer si se queda pegado el render?

Si el log del render dice "Port 10000 not reached" o algo similar:

* Revisa que en tu codigo no tengas puesto el puerto fijo. Render le asigna automaticamente a travez de una variable interna. Por eso usamos ugicorn, para que el se encarge de eacuchar donde el Render la diga.
