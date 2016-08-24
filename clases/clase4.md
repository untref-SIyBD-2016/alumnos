# clase 4

## preparación

Hoy vamos a trabajar con casos de prueba. Abrimos un intérprete de comandos:

Presionar simultáneamente las teclas de windows ![win](imagenes/win-key.png) y ![tecla R](imagenes/R-key.png), escribir **`CMD`** y dar *Enter*.
Se abrirá una pantalla negra llamada intérprete de comandos. Escribir ahí:

```sh
CD\
MKDIR hecho
CD hecho
MKDIR npm
CD npm
git clone https://github.com/untref-SIyBD-2016/pruebas.git
CD pruebas
```

Se debe ver en la pantalla: `C:\hecho\npm\pruebas`

## Casos de Prueba

Para los ejercicios vamos a usar funciones en `Javascript` y casos de prueba escritos en `.txt`

Cada línea del `.txt` contiene una prueba, las columnas están separadas por ';', el primer dato es 'si' o 'no' o sea el valor esparado la función que estamos probando.
Luego separdo por ';' estarán los datos para pasarle a la función. 

## prueba con palíndromos

Escribamos una función que indique si un texto es un palíndromo. 
*Un texto es palíndromo si al invertir el orden de las letras que lo componen se obtienen las mismas letras*

En el intérpete de comandos pongamos

```sh
CD palindromo
node probar.js
```

Debe mostrar:
```txt
c:\hecho\npm\pruebas\palindromo>node probar.js
Ok en si=arenera
Ok en si=oso
Ok en no=tamaño
Cantidad de pruebas 3
Cantidad de errores 0

c:\hecho\npm\pruebas\palindromo>
```
