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

### desarrollo

Agreguemos ahora más casos de prueba y probemos ¿faltó probar algo?

## Triángulo Isoscéles

En este ejercicio vamos a probar una función que recibe 3 parámetros y contesta 

  * *true* si los 3 parámetros corresponden a longitudes de un triángulo isosceles
  * *false* en cualquier otro casos
  
Se define que un triángulo es un *triángulo isosceles* cuando tiene un par de lados de igual longitud y otro distinto. 

Empecemos viendo la función que está en el repositorio, al tipear

```sh
cd\hecho\npm\pruebas\isosceles
node probar.js
```

Vemos:
```sh
c:\hecho\npm\pruebas\isosceles>node probar.js
Error en [ '3', '4', '4' ] se obtuvo false y se esperaba true
Error en [ '4', '4', '4' ] se obtuvo true y se esperaba false
Ok en no;3;4;5
Cantidad de pruebas 3
Cantidad de errores 2

c:\hecho\npm\pruebas\isosceles>
```

La función está mal porque está reconociendo como isósceles un triangulo equilátero (con todos sus lados de igual longitud) y no reconoce un triángulo isosceles.

  * Corrijamos el programa y volvamos a probar. Cambiemos el && por || (operador *and* por operador *or*). ¿Qué pasó? reconoce el isósceles pero no rechaza el equilátero. 
  * Pidamos que haya un par distintos:
  
```js
function isosceles(a,b,c){
    return (a==b || b==c) && a != c;
}

module.exports = isosceles;
```
  
Con las pruebas que pusimos funciona. Pero no está bien. Pongamos más casos de prueba
  
## solución

```js
function esPositivo(numero){
	return !isNaN(numero) && numero>0
}

function isosceles(a,b,c){
	a=Number(a)
	b=Number(b)
	c=Number(c)
	return esPositivo(a) &&
	       esPositivo(b) &&
	       esPositivo(c) &&
	       (a<b+c) &&
	       (b<a+c) &&
		   (c<b+a) &&
           (a==b || b==c || a==c ) && 
		   (a!=b || b!=c || a!=c );
}

module.exports = isosceles;
```
