# clase 2

## ejemplos SQL usados en la clase

Imaginemos en una primera instancia un plan de gatos, algunos gastos tienen un momento determinado, otros no.

```sql
create table plan(
  momento date,
  detalle text,
  monto numeric
);

insert into plan (momento, detalle, monto) values 
  (null, 'Alimentos', 20000),
  (null, 'Bebidas'  , 10000),
  (null, 'Limpieza' , 15000),
  ('2017-03-01', 'Adaptación del lugar', 40000);
  
create table presupuesto(
  concepto text,
  importe numeric
);
```

Imaginemos luego que tenemos un modo de detallar los gastos haciendo un reparto en base a una proporción `w` que indica cuánto se gastará cada `mes`:

```sql  
create table modo_detallar(
  mes date,
  w numeric
);

insert into modo_detallar (mes, w) values
  ('2017-04-01', 0.2),
  ('2017-05-01', 0.3),
  ('2017-06-01', 0.5);
```

Luego transformaremos los registros cuyo plan no tenía momento por otros registros repartidos según el `modo_detallar`:

Para poder experimentar con la bifurcación del tiempo en la base de datos (coherencia y atomicidad) 
hay que ejecutar estas instrucciones desde la línea de comandos con `psql`

```sh
cd c:\Program Files\PostgreSQL\9.5\bin
psql clase2_db postgres
```

```sql
BEGIN TRANSACTION; -- no en pgAdminIII ni en SQLFiddle 
INSERT INTO plan
  SELECT mes, detalle, monto*w
    FROM plan, modo_detallar
    WHERE momento IS NULL;
DELETE FROM plan
  WHERE momento IS NULL;
COMMIT; -- no en pgAdminIII ni en SQLFiddle 
select * from plan;
```

¿Qué pasaría si no existiesen transacciones y se ejecutara la inclusión en el presupuesto durante la transformación?


```sql
INSERT INTO presupuesto
  SELECT 'plan', sum(monto)
    FROM plan;
select * from presupuesto;
```


