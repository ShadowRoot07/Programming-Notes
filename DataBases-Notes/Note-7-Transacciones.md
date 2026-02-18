# Transacciones (ACID).

Una transaccion es un conjunto de;operaciones que se ejecutan como una unidad unica. O se hacen todas, o no se hace ninguna.

(Atomicidad, Consistencia, Aislamiento y Durabilidad).

**USO:**

* Resta dinero dela cuenta A.
* Sumar dinero a la cuenta B.

*Si el sistema falla en alguno de estos datos, el dinero desaparece. La trasaccion evita esto*

**COMANDOS:**

* BEGIN TRANSACTION: Inicia el bloque protegido.

* COMMIT: Guarda los cambios permanentemente.

* ROLLBACK: Cancela todo y vuelve al estado inicial.

```sql
BEGIN TRANSACTION;
    INSERT INTO Posts (Titulo, AutorId) VALUES ('Post importante', 1);
    -- Si ocurre un error, nada se Guarda
COMMIT;
```
