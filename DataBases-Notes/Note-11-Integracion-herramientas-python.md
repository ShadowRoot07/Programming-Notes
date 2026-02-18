# Integracion de SQLite, Mathplotlib y Pandas.

## Repaso de puntos clave:

1. **Pandas**

* DataFrame: es la estructura principal (una tabla con filas y columnas).

* Lectura: `pd.read_sql_query()`  es el puente directo con SQL.

* Filtrado: `df[df['columna'] valor]` para manipular datos sin escribir mas SQL.

2. **Mathplotlib**

* `plt.plot()`: Para lineas, graficar tendencias.
* `plt.bar()`: Para comparar categorias.
* `plt.show()`: Para renderizar la imagen.
* `plt.savefig()`: para guardar el archivo como png.

## Conexion total:


**Ejemplo con SQLite y Pandas:**

```python
import sqlite3 
import pandas as pd 

DB_NAME="mi_practica.db"

def analizar_datos():
# creamos la Conexion
    conn=sqlite3.connect(DB_NAME)

# Usamos pandas para leer la consulta SQL
    query = "SELECT estado, COUNT(*) AS cantidad FROM tareas GROUP BY estado"
    
    df= pd.read_sql_query(query, conn)

# Cerrar la conexion
    conn.close()

# Mostrar el resumen en consola
    print("Resumen de tareas por estado:")
    print(df)

    return df 

if __name__== "__main__":
    analizar_datos()
```

**Ejemplo con SQLite, Pandas y Mathplotlib:**

```python
import sqlite3
import pandas as pd 
import matplotlib.pyplot as plt

def generar_grafico():
    conn = sqlite3.connect("mi_practica.db")

# leemos los datos 
    df = pd.read_sql_query("SELECT estado , COUNT(*) AS cantidad FROM tareas GROUP BY estado", conn)
    conn.close()

# configuracion de Matplotlib 
    plt.figure(figsize(8,5))
    plt.bar(df['estado'], df['cantidad'], color='skybule')

# Etiquetas
    plt.title('Distribucion de mis tareas')
    plt.xlabrl('Estado de la tarea')
    plt.ylabel('Numero de tareas')
    plt.savefig('grafico_tareas.png')


if __name__== "__main__":
    analizar_datos()
```

## Puntos adicionales

**Pandas**

* `df.head()`: Muestra las primeras 5 filas.
* `df.groupby('columna').sum()`: Agrupa datos.
* `df.deacribe()`: Te da estadisticas exactas, media, maximo, minimo.

**Mathplotlib**

* `df.figure(figsize=(w, h))`: Define el tamanio de tu lienzo.
