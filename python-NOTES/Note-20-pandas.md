# pandas: Analisis de datos y tablas.

Pandas se basa en el objeto dataframe, que es una tabla con listas y columnas, es ideal para elanejo de archivos csv y manejo de datos.

```python
import pandas as pd 

#crear un dataframe (una tabla)
datos={
    'Producto': ['camisa', 'pantalon', 'zapatos'],
    'precio': [25, 40, 60]
}

df= pd.DataFrame(datos)

#Filtrar datos facilmente
caros= df[df['Precio']> 30]
print(df)
print("\nProductos caros:")
print(caros)
```

{por ahora este archivo que se trata de Pandas se dejara asi hasta nuevo aviso}
