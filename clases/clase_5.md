```sql
SELECT nieto.id, nieto.nhogar
  FROM miembros as jefe 
    inner join miembros as nieto 
       on jefe.id=nieto.id and jefe.nhogar=nieto.nhogar
  WHERE jefe.parentes_2=1 
    and nieto.parentes_2=5
  ORDER BY nieto.id, nieto.nhogar
  
  
SELECT nieto.id, nieto.nhogar
  FROM miembros as nieto 
  WHERE nieto.parentes_2=5
  ORDER BY nieto.id, nieto.nhogar

SELECT hijo.id, hijo.nhogar
  FROM miembros as padre 
    inner join miembros as hijo 
       on padre.id=hijo.id and padre.nhogar=hijo.nhogar
  WHERE padre.parentes_2=6 
    and hijo.parentes_2=3
  ORDER BY hijo.id, hijo.nhogar  

-- hay repetidos

SELECT DISTINCT hijo.id, hijo.nhogar
  FROM miembros as padre 
    inner join miembros as hijo 
       on padre.id=hijo.id and padre.nhogar=hijo.nhogar
  WHERE padre.parentes_2=6 
    and hijo.parentes_2=3
  ORDER BY hijo.id, hijo.nhogar  

WITH hogares_AN as (
SELECT DISTINCT hijo.id, hijo.nhogar
  FROM miembros as padre 
    inner join miembros as hijo 
       on padre.id=hijo.id and padre.nhogar=hijo.nhogar
  WHERE padre.parentes_2=6 
    and hijo.parentes_2=3
UNION 
SELECT DISTINCT nieto.id, nieto.nhogar
  FROM miembros as nieto 
  WHERE nieto.parentes_2=5
ORDER BY id, nhogar) 
SELECT sum(fexp)*100.0/
     (SELECT SUM(fexp) FROM hogares AS h INNER JOIN viviendas AS v ON v.id=h.id)
  FROM hogares_AN AS h inner join viviendas AS v ON v.id=h.id
  
  
-- por comuna

WITH hogares_AN as (
SELECT DISTINCT hijo.id, hijo.nhogar
  FROM miembros as padre 
    inner join miembros as hijo 
       on padre.id=hijo.id and padre.nhogar=hijo.nhogar
  WHERE padre.parentes_2=6 
    and hijo.parentes_2=3
UNION 
SELECT DISTINCT nieto.id, nieto.nhogar
  FROM miembros as nieto 
  WHERE nieto.parentes_2=5
ORDER BY id, nhogar) 
SELECT comuna, sum(fexp)*100.0/
     (SELECT SUM(fexp) 
        FROM hogares AS h 
        INNER JOIN viviendas AS v2 ON v2.id=h.id
        WHERE v1.comuna=v2.comuna
      )
  FROM hogares_AN AS h inner join viviendas AS v1 ON v1.id=h.id
  GROUP BY comuna
  ORDER BY comuna
  
  
CREATE TABLE comunas
  AS SELECT DISTINCT comuna 
       FROM viviendas
       ORDER BY comuna;
       
ALTER TABLE comunas
  ADD PRIMARY KEY (comuna);
  
ALTER TABLE comunas
  ADD zona INTEGER;  
  
UPDATE comunas SET zona = 
  CASE WHEN comuna in (2,13,14) THEN 1
     WHEN comuna in (4,8,9,10) THEN 3
     ELSE 2 END;
     
WITH hogares_AN as (
SELECT DISTINCT hijo.id, hijo.nhogar
  FROM miembros as padre 
    inner join miembros as hijo 
       on padre.id=hijo.id and padre.nhogar=hijo.nhogar
  WHERE padre.parentes_2=6 
    and hijo.parentes_2=3
UNION 
SELECT DISTINCT nieto.id, nieto.nhogar
  FROM miembros as nieto 
  WHERE nieto.parentes_2=5
ORDER BY id, nhogar) 
, vivZ AS (
  select v.*, zona 
    FROM viviendas AS v 
    INNER JOIN comunas AS c ON v.comuna=c.comuna
)
SELECT zona, sum(fexp)*100.0/
     (SELECT SUM(fexp) 
        FROM hogares AS h 
        INNER JOIN vivZ AS v2 ON v2.id=h.id
        WHERE v1.zona=v2.zona
      )
  FROM hogares_AN AS h inner join vivZ AS v1 ON v1.id=h.id
  GROUP BY zona
  ORDER BY zona     
```



