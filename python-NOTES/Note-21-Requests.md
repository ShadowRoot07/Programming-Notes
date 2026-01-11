# Request: conexion a internet (APIs)

Request sirve para interactuar con sitios web y servidores de forma automatica. Se usa para consumir APIs (como pedir clima, noticias, datos web) o para web scrping.

codigo de ejemplo:

```python
import request

respuesta= request.get("http://api.github.com/users/octocat")

#Si la comexcion es exitosa, saldra 200
if respuesta.status_code== 200:
    datos_usuario= respuesta.json() #convertir la respuesta a diccionario
    print(f"Nombre de usuario: {datos_usuario['login']}")
    print(f"Bio: {datos_usuario['bio']}")
```

{Esto es todo lo que se escribira de este tema por ahora}
