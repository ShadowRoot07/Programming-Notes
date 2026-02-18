# Vistas

Una vista es una tabla virtual, no almacena datos fisicamente, Cada vez que llamas una vista, SQL ejecuta la consulta guardada.

la convenciin es usar `vw_` o `v_`.

```sql 
CREATE VIEW IF NOT EXISTS vw_resumen_posts AS 
SELECT 
    p.PostId,
    p.Titulo,
    a.Nombre AS Autor
FROM Posts p 
JOIN Autores a ON p.AutorId = a.AutorId;
```
