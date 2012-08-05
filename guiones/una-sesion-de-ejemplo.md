
Un ejemplo de sesión de R
=========================

### Outline:

* exresion y una asignacion
* crear, borrar y reescribir objetos
* lista de objetos activos (ver si un objeto existe)
* cálculos matemáticos simples con estos objetos
* concatenar/combinar comandos
* ejemplo de calculo de gastos para un proyecto:
  - nombres a las variables (sueldos, gastos etc...)
  - un script para repetirlo cambiando detalles
  - una funcioncita para hacerlo incluso más rápido
* Ejemplo de un loop automatizando una tarea, para mostrar su utilidad
  (con la historia del proyecto?)
* Ejemplos de los gráficos más comunes: hist, plot y points/lines (abline?)
* terminar la sesión
* Resumen y final remarks (el mensaje!)

Inicio
------

Una vez iniciado el R, lo que vemos es un command prompt y el cursor, indicando que se espera el input del usuario.

Podemos empezar por una expresión sencilla:

```
10 / 3
```

Lo que ocurre luego de dar enter, es que R devuelve el resultado de esta cuenta matemática simple. En este caso se trata de una expresion sencilla. En este caso R imprime el resultado en la consola y dicho valor no es guardado para su uso futuro, simplemente se descarta. Por suerte para nosotros, podemos muy facilmente repetir este comando, sin siquiera escribirlo de vuelta, ya que simplemente usando la tecla 'arriba' se puede volver a ver el comando y con enter lo ejecutamos de vuelta.

Pero esta no es la forma más común de guardar un resultado: las asignaciones sirven para este propósito y se ven como esto:
```
x <- 10 / 3
```

En este caso R no imprime el resultado, pero hace algo mejor, lo guarda en un objeto llamado x, el cual existirá hasta el momento en que cerremos nuestra sesión de R. Por lo tanto, podemos utilizar nuevamente este objeto cuando querramos, por ejemplo:
```
x
```

Esto nos devuelve el valor calculado.
```
x * 3
```

Aquí calculamos el valor de x multiplicado por 3.
```
y <- x * 3
```

Aquí calculamos el valor de x multiplicado por 3 y lo guardamos en un nuevo objeto llamado y

Nota: cada comando que ejecutamos debe ir en una línea, no se pueden poner dos comandos en la misma línea. Para poner un ejemplo:
```
y <- x * 3 w <- y + 15
```

Esto da error. Se puede solucionar:
```
y <- x * 3; w <- y + 15
```

Sin embargo esta es una opción a la que no queremos recurrir, ya que es considerada desprolija y efectivamente dificulta la lectura de quien está tratando de entender el código.

Nota: los nombres de los objetos no tienen porqué ser de una sola letra, y de hecho es recomendable usar palabras que den un indicio:
```
cat.ad <- 4
cat.op <- 5
hipot <- sqrt(cat.ad ** 2 + cat.op ** 2)
```

Hay algunas reglas para hacer estos nombres. En general, no usar espacios ni números al inicio de las mismas es suficiente.

Como habrá notado, eventualmente la sesión se llena de objetos con varios nombres de objetos. La función ls (abrebiación de "list") sirve para ver estos nombres:
```
ls()
```

La función objects hace exactamente lo mismo.

Para el extra: alternativamente, sobre todo si estamos usando R de forma interactiva, podemos usar exists para ver si está presente algún objeto en particular.

Es muy recomendable tratar de tener una sesión de R "limpia" para no perderse en un mar de objetos que no están siendo usados. La función rm sirve para borrar objetos de la memoria.
```
rm(x)
```

Matemática
----------

Crear una secuencia de números:
```
1:5
seq(1, 20, by=3)
```

Dado que R es un software enfocado a estadística, tiene muchas funciones matemáticas (muchas con mismos nombres que en excel, c, etc...):
```
sin(pi)
exp(10)
1000 ** 2
log(1)
rnorm(10)
mean(rnorm(10))
```

Incluso tiene algunas herramientas que son simples de usar que pueden ser muy útiles. La función curve sirve para graficar una función matemática (en números reales) cualquiera:
```
curve(cos(x), from=-tau / 2, to=tau / 2)
```

Nótese que la variable por convención es x, pero este x no tiene nada que ver con el x que creamos antes. Podemos ver cuánto vale el cos de x cuando x vale pi/2 (90 grados)
```
abline(v=tau / 4, col='red')
```

Quiero ver los ejes:
```
abline(v=0, h=0, col='blue')
```

Ahora quiero saber el área bajo la gráfica de esta función en el intervalo 0, tau / 4, o en otras palabras, la integral:
```
integrate(cos, 0, tau / 4)
```

Esto nos da una respuesta con un pequeño error numérico (ya que R no hace cálculo de la solución analítica).

De forma opuesta la funcin deriv hace calculos de derivadas.

Sin dudas este tipo de funciones tienen gran utilidad y pueden ser utilizadas como ayuda en nuestros trabajos o estudios. Acá están solo para ilustrar las cosas básicas que R puede hacer.


Combinación de comandos
-----------------------

Una de las grandes virtudes que tiene R es que favorece naturalmente la combinación de diferentes comandos para crear cálculos sofisticados. Esta es la clave de la enorme flexibilidad de este programa.

La combinación de comandos en su forma más básica se ve algo así:
```
mean(rnorm(100))
```

Aquí estamos ejecutando la función rnorm, que genera números aleatorios con distribución gaussiana o normal, para crear 100 de estos números. La salida de este comando es inmediatamente tomada por la función mean, quien calcula el promedio de estos 100 números. En términos matemáticos esto es una composición de funciones. Anteriormente vimos otro ejemplo, el de la hipotenusa.

Pero generalmente las instrucciones que son creadas son mucho más elaboradas y permiten realizar cualquier cálculo imaginable.

Veremos un caso de ejemplo ahora para ilustrar esta idea y otras que son fundamentales para sacar provecho a R.


Presupuesto de un proyecto
--------------------------

Muchas veces en nuestras vidas nos veremos obligados a calcular el presupuesto de un proyecto, nuestra casa, etc. Vamos a suponer que trabajamos en un proyecto de investigación de la universidad en el que necesitamos calcular el costo de equipos de trabajo, materiales, viáticos y sueldos. Este tipo de tareas suele volverse compleja debido a que muchas variables están en juego, como diferentes modelos y costos para los equipos, cantidad de viajes y medios de transporte, estadías, cantidad de técnicos contratados, sueldos de dichos técnicos, etc.

Un ejemplo sencillo sería este:

```
dineroTotal <- 450000
microscopio <- 50000
salida <- 20000
tecnicoMes <- 6000
ntecnicos <- 2
meses <- 3
nsalidas <- 2

margen <- dineroTotal - microscopio - nsalidas * salida - tecnicoMes * meses *
ntecnicos
```

Si queremos jugar cambiando el número de meses o el número de salidas, entonces cambiamos el valor de alguna de estas (uso la flecha hacia arriba para modificar uno de los valores y veo el nuevo margen). 

Como veran, puedo cambiar varias cosas, como el valor del microscopio o de la salida, para seguir estimando el margen de dinero que voy a tener. Pero no es muy práctico hacer esto en la consola. Es aquí cuando entra en juego lo que se conoce como script de R. Para hacer mi script de R, voy a abrir un editor de texto; en el GUI de R de windows hay un editor de texto por defecto (abro ese).

Como me interesa reciclar los últimos comandos que hice, uso history() y hago un copiar y pegar en el editor.
```
history()
```

Una vez aquí, borro los comandos que sobran. Luego modifico aquellos que me interesa (meses, microscopio, etc). Luego salvo y hago un source en R:
```
source('presupuesto.R')
```

Esto es lo que se conoce como R no interactivo!

Cada vez que cambimos el script en el editor, tenemos que salvar antes de llamar a la función source, ya que si no la versión en el disco duro no se va a actualizar y por lo tanto el R va a seguir interetando la versión anterior.

Así puedo jugar con cualquiera de mis variables y determinar cuál será mi margen. Notarán que el proceso se vuelve un poco engorroso al cabo de un tiempo.

Aquí es cuando encontramos la primer razón para usar RStudio (paso al RStudio).

En RStudio trabajamos con el script y podemos enviar comandos a la consola para que R ejecute, sin necesidad de estar cambiando de ventana. Incluso podemos enviar el documento completo sin necesidad de salvarlo.

Mostrar ejemplos

Una funcioncita para facilitar
------------------------------

Para facilitar más aún el proceso, R tiene una herramienta que sale en nuestro auxilio: la función

Tomando los mismos comandos que tenía antes, pero cambiando sutil y estratégicamente la ubicación de los mismos voy a hacer mi funcion prp:
```
prp <- function(nsalidas = 2, ntecnicos = 2, microscopio = 50000, salida = 20000,
                tecnicoMes = 6000, meses = 3) {
  dineroTotal <- 450000
  margen <- dineroTotal - microscopio - nsalidas * salida - tecnicoMes * meses
  margen
}
```
Entonces si yo ejecuto
```
prp()
```

Obtengo el cálculo que teníamos antes. Para esto el R usó los valores que puse en la primer línea del script. Estos son los valores "por defecto" es decir aquellos que tomará la función si es que el usuario no puso ningún valor propio.

La ventaja de esta aproximacion es que podemos cambiar rápidamente de valores para nuestras variables y así encontrar cómo cambia el margen de nuestro presupuesto.

En combinación (no importa el orden de las variables)
```
prp(nsalidas = 3, ntecnicos = 3)
```

Ejemplo de un loop
------------------

A veces lo que queremos es simplemente ver todos los valores posibles. Para este tipo de tareas repetitivas existen los loops. Concretamente, podríamos hacer algo como esto:
```
prp(nsalidas = 1)
prp(nsalidas = 2)
prp(nsalidas = 3)
prp(nsalidas = 4)
prp(nsalidas = 5)
prp(nsalidas = 6)
prp(nsalidas = 7)
prp(nsalidas = 8)
prp(nsalidas = 9)
```
etc

Pero con un loop esto se reduce a:

```
for (i in 1:20) {
  cat('No. de salidas =', i, '\n')
  print(prp(nsalidas = i))
}
```

Si además queremos ver cómo se combina con el número de técnicos, podemos hacer otro loop dentro de este:
```
for (i in 1:20) {
  for (j in 1:6) {
    cat('No. salidas =', i, 'No. técnicos =', j, '\n')
    print(prp(nsalidas = i, ntecnicos=j))
  }
}
```

La tercer línea fue agregada para poder ver en la salida cuántas salidas y técnicos corresponden.

Nota: esto se puede hacer de formas más fáciles: el primer ejemplo con `prp(nsalidas=1:20)` y el segundo con la función 'outer':
```
outer(1:20, 1:6, prp)
```

En este ejemplo de acercamiento, hemos combinado varias utilidades de R para hacer un análisis relativamente sofisticado del presupuesto de nuestro proyecto ficticio. Empezando incialmente con una cuenta sencilla, pasamos a crear una función y luego utilizamos loops para ver todo el rango de combinaciones posibles.

El objetivo de hacer este ejercicio es mostrar someramente cómo se puede pasar con facilidad de un análisis simple y limitado a otro más elaborado y abarcativo, a través de la secuenciación de pasos. Nótese que el uso de scripts en este proceso es fundamental, ya que permite construir sobre lo ya hecho y mantener ordenados y bien comentados los pasos dados para llegar a un resultado en particular.

Esta posibilidad no sólo permite que un usuario pueda volver a repetir su análisis en el futuro, si no que también facilita el compartir tareas entre varios usuarios, incluyendo el 'reproducible research' (investigación reproducible), tema que se ha vuelto muy caliente en los últimos años. 

Mensaje final
-------------

En esta primer introducción a las capacidades de R no nos hemos metido demasiado en temas específicos de análisis de datos, el cual suele ser el motivo principal por la que R es utilizado. Sin embargo hemos visto herramientas de uso general que pueden y son constantemente utilizadas en cualquier análisis de datos (funciones matemáticas, gráficos, funciones personalizadas, loops).

El objetivo de este video es mostrar rápidamente algunas de las funcionalidades básicas que tenemos en R y, con suerte, entusiasmarlos en el aprendizaje de este lenguaje. Todas las cosas que fueron tocadas en este video, y más, se van a profundizar a medida que avanza el curso, de forma que no hay que preocuparse demasiado por aquello que no se entendió claramente. Animamos a que cada uno en su casa juegue con los ejemplos dados aquí y así pueda ir descubriendo por cuenta propia el R.