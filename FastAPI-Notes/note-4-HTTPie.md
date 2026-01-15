# HTTPie.

Es una fantanstica herramienta para depurar desde comandos, te da herramientas estandar como:

* 1. **El codigo de estado**: (200 Ok, 404 Not Found, 500 Error).

* 2. **Los encabezados (Headers)**: Para ver el contenido es application/json o si los tokens de seguridad son correctos.

* 3. **El cuerpo (Body)**: Ver el json que devuelve tu API.

*HTTPie* te muestra todo eso *de forma organizada*.

## Ejemplos Practicos con HTTPie:

Primero hay que asegurarse de tenerlo instalado:

```bash
pip install httpie 
```

Supongamos que tienes tu FastAPI corriendo en tu localhost de tu maquina.

* 1. Peticion GET(consuktar datos): para ver que responde tu ruta principal.

```bash
http GET http://127.0.0.1:8000/
```

(Incluso al onmitir el metodo GET ya es el metodo por defecto).

* 2. Peticion POST (Enviar datos para crear algo): Imagina que ya tienes una ruta para crear un usuario que recibe un nombre y una edad. Con HTTPie no tienes que escribir el JSON, el lo construlle por ti:

```bash
http POST http://127.0.0.1:8000/usuarios nombre="Gohan" edad:=20 
```

**Nota**: Fijate en el ":=", esto se usa para numeros y booleanos. Para texto normal "string" es solo "=".

* 3. Peticion con autenticacion (Tokens): Cuando se llega a la parte de seguridad, se tiene que enviar un token encriptado, con HTTPie es super limpio:

```bash
http GET http://127.0.0.1:8000/perfil "Authorization: Bearer TU_TOKEN_AQUI"
```

* 4. Ver los encabezados (modo de Depuracion **Pro**): Si quieres ver que esta pasando *detras de camaras* (los headers de la peticion y la respuesta) aniade `-v` (verbose, estandar de comandos):

```bash
http -v GET http://127.0.0.1:8000/items/1 
```

## Comparacion rapida:

* **Navegador:** Solo sirve para GET, no se pueden enviar JSON facilmente.

* **Swagger (/docs):** Genial por que es visual, pero a veces es lento hacer click en tantos botones.

* **HTTPie:** Es instantaneo, escribes el comando, das Enter y vez el resultado. ideal cuando estas cambiando el codigo y probando rapido en Termux.
