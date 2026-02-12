# Normalizacion.

La normalizacion es el proceso de organizar los datos para evitar redundancia y proteger la integridad. Existen muchas forma normales, pero an la vuda real es solo necesaria dominar las primeras 3 leyes.

## Primera forma Normal (1FN): Automocidad.

    * Regla: Cada celda debe de tener un solo valor. No puede haber columnas repetidas.

    * Ejemplo de error: una cplumna llamada telefonos que guarde "9293-643-245".

    * Solucion: crear una fila dostinta para cada telefono o una tabla aparte.

## Segunda forma normal (2FN): Depebdencia total.

    * Regla: debe de cumplir la 1FN y todos los datos deben de depender de la Llave primaria completa.

    * Ejemplo de error: En una tabla de pedidos, poner el nombre del cliente. El nobre del cliente depende del ID del cliente, no del ID del pedido.

    * Solucion: separar clientes de pedidos.
    
## Tercera forma normal (3FN): No dependencias transitivas.

    * Regla: Debe de cumplir la 2FN y no deben devhaber columnas que dependan de otras columnas que no sean la llave primaria.

    * Ejemplo de error: En una tabla devempleados, tener Ciudad y Codigo postal. la ciudad eepende del codigo postal, no directamente del empleado.

    * Solucion: Crear uma tabla de codigos postales.


**Las formas normales HARDS**

 A partir de aqui entramos a un terreno tecnico, en la mayoria de aplicaciones es suficiente solo tener las 3 primeras leyes, con llegar a 3FN ea suficiente. Pero para cosas mas complejas y sistematicas, estan estas otras 2.

## Cuarta forma normal (4FN): Dependencias Multivaluadas.

    * Regla: Una tabla no puede tener dos o mas relaciones de muchos a muchos que sean independientes entre si.

    * Ejemplo de error: Una tabla Chef qie tiene especialidad y idioma:
        
        * Podria haber confuciones al guardar los datos de estas filas y colimnas.

    * Solucion: Separar en dos las tablas: Chef_especialidad y Chef_idioma.

## Quinta forma normal (5FN): Forma normal de proyeccion de union.

    * Regla: Se alcanza cuando la informacion no puede ser dividida en tablas mas pequenias sin perder los datos o crear rwdundancias al intentar unirlas de nuevo. Este es el nivel maximo de la Automatizacion.

    * Cuando se usa?: Se usa cuando hay reglas de negcio cruzadas.

    * Casi nadie llega, a menos qie trabaje en sistemas criticos (banca, aeroespacial).
