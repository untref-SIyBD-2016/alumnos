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
  
  
