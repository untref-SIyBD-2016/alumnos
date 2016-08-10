# clase 2

## ejemplos SQL usados en la clase

### transacciones

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


### select

Primero nos bajamos los datos de la [EAH](https://www.estadisticaciudad.gob.ar/eyc/?p=54071) y
usamos [txt-to-sql](http://codenautas.com/txt-to-sql) para pasarlo a SQL. Luego metemos todo en la base de datos

Vamos a buscar qué hogares están en viviendas tipo 5. 

```sql
SELECT * 
  FROM eah2015_usuarios_hog
  WHERE v2_2=5
  ORDER BY id;
```

Vamos a contar las viviendas de cada tipo en la muestra

```sql
SELECT v2_2, count(*) as cantidad_viviendas
  FROM eah2015_usuarios_hog
  WHERE nhogar=1
  GROUP BY v2_2
  ORDER BY v2_2;
```

Cuando no hay cláusula `GROUP BY` se obtiene una fila por cada registro de la tabla que cumpla la condición del `WHERE` 
(cuando no hay `WHERE` son todos los registros de la tabla).

La presencia del `GROUP BY` determina que se obtendrán datos agrupados. 
Las columnas que aparecen listadas en `GROUP BY` son las únicas que pueden estar libres en la lista de campos del `SELECT`, 
el resto tiene que estar dentro de una función de agregación `SUM`, `COUNT`, `AVG`, etc

### select con subqueries

Ahora vamos a obtener el % del total, además queremos reclasificar los tipos de vivienda en 1=casa, 2=dept y resto=otros
```sql
WITH viviendas AS (
  SELECT CASE v2_2 WHEN 1 THEN 'casa' 
                   WHEN 2 THEN 'dept' 
                   ELSE 'otros' 
         END AS tipo_viv,
         fexp
    FROM eah2015_usuarios_hog
    WHERE nhogar=1)
SELECT tipo_viv, 
       SUM(fexp)*100.0/(SELECT SUM(fexp) FROM viviendas)
  FROM viviendas
  GROUP BY tipo_viv
  ORDER BY tipo_viv;
```

### select con joins

Ahora vamos a obtener el % de personas que viven en casa o departamento
```sql
WITH personas AS (
  SELECT CASE v2_2 WHEN 1 THEN 'casa' 
                   WHEN 2 THEN 'dept' 
                   ELSE 'otros' 
         END AS tipo_viv,
         i.fexp
    FROM eah2015_usuarios_hog h
      INNER JOIN eah2015_usuarios_ind i ON h.id=i.id
)
SELECT tipo_viv, 
       SUM(fexp)*100.0/(SELECT SUM(fexp) FROM personas),
       SUM(fexp)
  FROM personas
  GROUP BY tipo_viv
  ORDER BY tipo_viv;
```

Siempre hay que verificar con los totales:

```sql
SELECT sum(i.fexp)
  FROM eah2015_usuarios_ind
```
