´´´sql
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
```
