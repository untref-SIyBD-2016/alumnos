# clase 6

## preparación

Hoy vamos a intentar poner en línea una mini aplicación web basada en base de datos. 

Presionar simultáneamente las teclas de windows ![win](imagenes/win-key.png) y ![tecla R](imagenes/R-key.png), escribir **`CMD`** y dar *Enter*.
Se abrirá una pantalla negra llamada intérprete de comandos. Escribir ahí:

```sh
CD\
MKDIR hecho
```

Puede pasar que el `MKDIR` diga que el directorio ya existía, eso no es problema, seguimos adelante.

```sh
CD hecho
MKDIR npm
CD npm
git clone https://github.com/untref-SIyBD-2016/ejemplo-t.git
CD ejemplo-t
COPY example-config-db.json local-config-db.json
```

Se debe ver en la pantalla: `C:\hecho\npm\ejemplo-t`

Instalamos los paquetes de node y ponemos a correr el servidor:

```sh
npm install
npm start
```

Luego revisamos (editamos) el archivo de configuración llamado `local-config-db` para que coincidan el nombre de la base de datos y demás parámetros.

## Alternativas de SQL

```sql
select comuna,
       sum(case when e2=3 then fexp else 0 end) as numerador,
       sum(fexp) as denominador
  from miembros
  where edad>=3
  group by comuna
  order by comuna;

select comuna, sum(fexp) as numerador,
  (select sum(fexp) from miembros z where edad>=3 and z.comuna=x.comuna) as denominador
  from miembros as x
  where edad>=3 and e2=3
  group by comuna
  order by comuna;
```
