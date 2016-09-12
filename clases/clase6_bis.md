Ejemplos vistos en la clase de consulta:

```sql
ALTER TABLE EAH2015_usuarios_IND ADD COLUMN rango_e integer;

UPDATE EAH2015_usuarios_IND SET rango_e = CASE
  WHEN edad<18 THEN 1
  WHEN edad<65 THEN 2
  WHEN edad<80 THEN 3
  ELSE 4
END;
*/

SELECT rango_e, sum(fexp), sum(sum(fexp)) over ()
  FROM EAH2015_usuarios_IND 
  GROUP BY rango_e
  ORDER BY rango_e;

SELECT rango_e, sum(fexp), 
       (select sum(fexp) FROM EAH2015_usuarios_IND ) as total
  FROM EAH2015_usuarios_IND 
  GROUP BY rango_e
  ORDER BY rango_e;

select comuna, 
       sum(case when sexo=1 then fexp else 0 end) as varones, 
       sum(case when sexo=1 then 0 else fexp end) as mujeres, 
       sum(fexp)
  FROM EAH2015_usuarios_IND 
  GROUP BY comuna
  ORDER BY comuna;

select comuna, 
       sum(case when sexo=1 then fexp else 0 end)*100.0/sum(fexp) as varones, 
       sum(case when sexo=1 then 0 else fexp end)*100.0/sum(fexp) as mujeres
  FROM EAH2015_usuarios_IND 
  GROUP BY comuna
  ORDER BY comuna;

select comuna, religion, sum(n.fexp)*100.0
      /(select sum(d.fexp) 
          from EAH2015_usuarios_IND d 
          WHERE d.comuna=n.comuna
       )
  FROM EAH2015_usuarios_IND n
  GROUP BY comuna, sexo
  ORDER BY comuna, sexo;

```
