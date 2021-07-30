# Herramientas de programación adicionales {#Herramientasdeprogramaciónadicionales}

<hr style="background-color:#03193b;height:2px">

Palabras clave:
- Estimación de máxima verosimilitud 
- Votante promedio 
- Análisis de Monte Carlo 
- Experimento de Montecarlo 
- Posición de emisión 

Como se ha mostrado en los últimos capítulos, R  ofrece a los usuarios flexibilidad y oportunidades para análisis de datos avanzados que no se encuentran en muchos programas. En este capítulo final, exploraremos las herramientas de programación de R , que permiten al usuario crear código que aborde cualquier problema único que enfrente.

Tenga en cuenta que muchas de las herramientas relevantes para la programación ya se han presentado anteriormente en este libro. En el Cap. 10 ,  se introdujeron las herramientas para el álgebra matricial en R y muchos programas requieren procesamiento de matrices. Además, las declaraciones lógicas (o booleanas) son esenciales para la programación. Los operadores lógicos de R se introdujeron en el Cap. 2 , por lo tanto, consulte la Tabla  2.1 para recordar cuál es la función de cada operador.   

Las secciones siguientes presentarán varias otras herramientas que son importantes para la programación: distribuciones de probabilidad, definición de nuevas funciones, bucles, ramificaciones y optimización (que es particularmente útil para la estimación de máxima verosimilitud). El capítulo terminará con dos grandes ejemplos aplicados. El primero, basado en Monogan (2013b), introduce la programación orientada a objetos en R  y aplica varias herramientas de programación de este capítulo para encontrar soluciones a un problema de teoría de juegos insoluble. El segundo, dibujo de Signorino (1999), Es un ejemplo de análisis de Monte Carlo y la estimación de máxima verosimilitud más avanzada en R . 1 Juntas, las dos aplicaciones deben mostrar cómo todas las herramientas de programación pueden unirse para resolver un problema complejo.

11.1 Distribuciones de probabilidad
R le permite utilizar una amplia variedad de distribuciones para cuatro propósitos. Para cada distribución, R le permite llamar a la función de distribución acumulativa (CDF), la función de densidad de probabilidad (PDF), la función de cuantiles y las extracciones aleatorias de la distribución. Todos los comandos de distribución de probabilidad constan de un prefijo y un sufijo. La tabla  11.1 presenta los cuatro prefijos y su uso, así como los sufijos para algunas distribuciones de probabilidad de uso común. Las funciones de cada distribución toman argumentos únicos para los parámetros de esa distribución de probabilidad. Para ver cómo éstos se especifican, los archivos de uso de ayuda (por ejemplo, ? Punif , ? PEXP , o ? Pnorm ). 2
Cuadro 11.1
Usando distribuciones de probabilidad en R

Prefijo

Uso

Sufijo

Distribución

 
pag

Función de distribución acumulativa

norma

Normal

 
D

Función de densidad de probabilidad

logis

Logístico

 
q

Función cuantil

t

t

 
r

Sorteo aleatorio de la distribución

F

F

 
 	 	
unif

Uniforme

 
 	 	
pois

Poisson

 
 	 	
Exp

Exponencial

 
 	 	
chisq

Chi-cuadrado

 
 	 	
binom

Binomio

 
Si desea conocer la probabilidad de que una observación normal estándar sea menor que 1.645, use el comando pnorm de la función de distribución acumulativa (CDF) :

normal (1.645)

Suponga que desea dibujar un escalar de la distribución normal estándar: para dibujar   a ∼ N( 0 , 1 ) , use el comando de dibujo aleatorio rnorm :

a <- normal (1)

Para dibujar un vector con diez valores de una distribución χ 2 con cuatro grados de libertad, use el comando de dibujo aleatorio:

c <- rchisq (10, gl = 4)

Recuerde del Cap. 10 que el comando de ejemplo también nos permite simular valores, siempre que proporcionemos un vector de valores posibles. Por tanto, R  ofrece una amplia gama de comandos de simulación de datos. 

Suponga que tenemos una probabilidad dada, 0.9, y queremos saber el valor de una distribución χ 2 con cuatro grados de libertad en la cual la probabilidad de ser menor o igual a ese valor es 0.9. Esto requiere la función de cuantiles :

qchisq (.9, gl = 4)

Podemos calcular la probabilidad de un cierto valor a partir de la función de masa de probabilidad (PMF) para una distribución discreta. De una distribución de Poisson con parámetro de intensidad 9, ¿cuál es la probabilidad de un conteo de 5?

dpois (5, lambda = 9)

Aunque suele ser de menor interés, para una distribución continua, podemos calcular el valor de la función de densidad de probabilidad (PDF) de un valor particular. Esto no tiene un significado inherente, pero ocasionalmente es necesario. Para una distribución normal con media 4 y desviación estándar 2, la densidad en el valor 1 viene dada por:

dnorm (1, media = 4, sd = 2)

11.2 Funciones
R le permite crear sus propias funciones con el comando de función . El comando de función utiliza la siguiente sintaxis básica:

function.name <- function (ENTRADAS) {BODY}

Observe que el comando de función primero espera que las entradas se enumeren entre paréntesis, mientras que el cuerpo de la función se enumera entre llaves. Como se puede ver, hay pocas restricciones en lo que el usuario elige poner en la función, por lo que una función se puede diseñar para lograr lo que el usuario desee.

Por ejemplo, supongamos que estamos interesados ​​en la ecuación   y= 2 +1X2 . Podríamos definir esta función fácilmente en R . Todo lo que tenemos que hacer es especificar que nuestra variable de entrada es x y nuestro cuerpo es el lado derecho de la ecuación. Esto creará una función que devuelve valores para y . Definimos nuestra función, llamada first.fun , de la siguiente manera:

first.fun <-función (x) {

     y <-2 + x ^ {- 2}

     retorno (y)

     }

Aunque está dividido en unas pocas líneas, todo esto es un gran comando, ya que las llaves ( {} ) abarcan varias líneas. Con el comando de función , comenzamos declarando que x es el nombre de nuestra única entrada, basado en el hecho de que nuestra ecuación de interés tiene x como el nombre de la variable de entrada. A continuación, dentro de las llaves, asignamos la salida y como la función exacta de x que nos interesa. Como último paso antes de cerrar nuestras llaves, usamos el comando return , que le dice a la función cuál es el resultado de salida después de que se llame. El regresoEl comando es útil para determinar la salida de una función por un par de razones: Primero, si definimos varios objetos dentro de un comando, esto nos permite especificar lo que debería imprimirse en la salida versus lo que era estrictamente interno a la función. En segundo lugar, return nos permite informar una lista de elementos como salida, si lo deseamos. Alternativamente, podríamos haber sustituido el comando invisible , que funciona de la misma manera que return , excepto que no imprime la salida en la pantalla, sino que simplemente almacena la salida.

Con esta función, si queremos saber qué valor toma y cuando x  = 1, solo necesitamos escribir: first.fun (1) . Como usamos return en lugar de invisible , la impresión simplemente dice:

[1] 3

Por supuesto, el resultado de que y  = 3 aquí se puede verificar fácilmente a mano. De manera similar, sabemos que si x  = 3, entonces  y= 219 . Para verificar este hecho, podemos escribir: first.fun (3) . R  nos dará la impresión correspondiente:

[1] 2.111111

Como tarea final con esta función, podemos graficar cómo se ve insertando un vector de valores x en ella. Considere el código:

my.x <-seq (-4,4, por = .01)

plot (y = first.fun (my.x), x = my.x, type = "l",

     xlab = "x", ylab = "y", ylim = c (0,10))

Esto producirá el gráfico que se muestra en la Fig.  11.1 .
Abrir imagen en nueva ventanaFigura 11.1
Figura 11.1
Gráfico de la función  y= 2 +1X2 

Como ejemplo más complicado, considere una función que usaremos como parte de un ejercicio más amplio en la Secta. 11.6 . La siguiente es una función de utilidad esperada para un partido político en un modelo de cómo los partidos elegirán una posición de tema cuando compitan en elecciones secuenciales (Monogan 2013b, Eq. (6)):
miUA(θA,θD)=Λ { - (metro1-θA)2+ (metro1-θD)2+ V}+ δΛ { - (metro2-θA)2+ (metro2-θD)2}
(11,1)
En esta ecuación, la utilidad esperada para un partido político (partido A ) es la suma de dos funciones de distribución logística acumuladas (las funciones Λ ), con la segunda ponderada por un término de descuento (0 ≤  δ  ≤ 1). La utilidad depende de las posiciones tomadas por el partido A y el partido D ( θ A y θ D ), una ventaja de valencia para el partido A ( V ) y la posición de emisión del votante mediano en la primera y segunda elección ( m 1 y m 2 ). Esta es ahora una función de varias variables y requiere que usemos el CDF de la distribución logística. Para ingresar esta función en R, escribimos:
Función cuadrática.A <(m.1, m.2, p, delta, theta.A, theta.D) {

    util.a <-plogis (- (m.1-theta.A) ^ 2 +

         (m.1-theta.D) ^ 2 + p) +

       delta * plogis (- (m.2-theta.A) ^ 2 +

         (m.2-theta.D) ^ 2)

    volver (util.a)

    }

El plogis comando calcula cada relevante p robabilidad del Logis tic CDF, y todos los demás términos se denominan auto-evidente de la ecuación. ( 11.1 ) (excepto que p se refiere al término de valencia V ). Aunque esta función era más complicada, todo lo que teníamos que hacer era asegurarnos de nombrar cada entrada y copiar Eq. ( 11.1 ) completamente en el cuerpo de la función.

Esta función más compleja, Quadratic.A , todavía se comporta como nuestra función simple. Por ejemplo, podríamos seguir adelante y proporcionar un valor numérico para cada argumento de la función como este:

Cuadrático.A (m.1 = .7, m.2 = -. 1, p = .1, delta = 0, theta.A = .7, theta.D = .7)

Al hacerlo, se imprime una salida de:

[1] 0.5249792

Por lo tanto, ahora sabemos que 0.52 es la utilidad esperada para la parte A cuando los términos toman estos valores numéricos. De forma aislada, este resultado no significa mucho. En teoría de juegos, normalmente estamos interesados ​​en cómo el partido A (o cualquier jugador) puede maximizar su utilidad. Por lo tanto, si tomamos todos los parámetros como fijos en los valores anteriores, excepto que permitimos que la parte A elija θ A como mejor le parezca, podemos visualizar cuál es la mejor opción de A. El siguiente código produce el gráfico que se ve en la figura  11.2 :
Abrir imagen en nueva ventanaFigura 11.2
Figura 11.2
Gráfico de la utilidad esperada de un partido aventajado durante dos elecciones dependiendo de la posición del asunto

posiciones <-seq (-1,1, .01)

util.A <-Quadratic.A (m.1 = .7, m.2 = -. 1, p = .1, delta = 0,

     theta.A = posiciones, theta.D = .7)

plot (x = posiciones, y = util.A, type = "l",

     xlab = "Posición del Partido A", ylab = "Utilidad")

En la primera línea, generamos una serie de posiciones del partido Un podrá optar, para θ A . En la segunda línea, calculamos un vector de utilidades esperadas para la parte A en cada una de las posiciones de emisión que consideramos y establecemos los otros parámetros en su valor mencionado anteriormente. Por último, usamos la función plot para dibujar un gráfico lineal de estas utilidades. Resulta que el máximo de esta función está en θ A  = 0. 7, por lo que nuestra utilidad de 0.52 mencionada anteriormente es la mejor fiesta que A puede hacer después de todo.

11.3 Bucles
Los bucles son fáciles de escribir en R  y se pueden usar para repetir cálculos que son idénticos o varían solo en unos pocos parámetros. La estructura básica de un bucle que utiliza el comando for es:

para (i en 1: M) {COMANDOS}

En este caso, M es el número de veces que se ejecutarán los comandos. R  también admite bucles con el comando while que sigue una estructura de comando similar:

j <- 1

mientras que (j <M) {

     COMANDOS

     j <- j + 1

     }

Si una de bucle o un tiempo de bucle funciona mejor puede variar según la situación, por lo que el usuario debe usar su propio juicio al elegir una configuración. En general, mientras que los bucles tienden a ser mejor para los problemas en los que se tendría que hacer algo hasta que se cumpla un criterio (como un criterio de convergencia), y para los bucles son generalmente mejores para las cosas que desea repetir un número fijo de veces. Bajo cualquier estructura, R  permitirá que se incluya una amplia gama de comandos en un bucle; la tarea del usuario es cómo administrar la entrada y salida del bucle de manera eficiente.

Como demostración simple de cómo funcionan los bucles, considere un ejemplo que ilustra la ley de los grandes números. Para hacer esto, podemos simular observaciones r andom a partir del comando de distribución normal estándar (fácil con el comando rnorm ). Dado que la normal estándar tiene una media poblacional de cero, esperaríamos que la media muestral de nuestros valores simulados sea cercana a cero. A medida que el tamaño de nuestra muestra aumenta, la media muestral debería estar más cerca de la media poblacional de cero. Un bucle es perfecto para este ejercicio: queremos repetir los cálculos de la simulación a partir de la distribución normal estándar y luego tomar la media de las simulaciones. Lo que difiere de una iteración a otra es que queremos que el tamaño de nuestra muestra aumente.

Podemos configurar esto fácilmente con el siguiente código:

set.seed (271828183)

almacenar <- matriz (NA, 1000,1)

para (i en 1: 1000) {

     a <- norma (i)

     almacenar [i] <- media (a)

     }

plot (store, type = "h", ylab = "Sample Mean",

     xlab = "Número de observaciones")

abline (h = 0, col = 'rojo', lwd = 2)

En la primera línea de este código, llamamos al comando set.seed , para que nuestro experimento de simulación sea replicable. Cuando R  extrae números aleatoriamente de alguna manera, utiliza un generador de números pseudoaleatorios, que es una lista de 2,1 mil millones de números que se asemejan a extracciones aleatorios. 3 Al elegir cualquier número del 1 al 2,147,483,647, otros deberían poder reproducir nuestros resultados usando los mismos números en su simulación. Nuestra elección de 271,828,183 fue en gran medida arbitraria. En la segunda línea de código, creamos un vector en blanco llamado store de longitud 1000. Este vector es donde almacenaremos nuestra salida del ciclo. En las siguientes cuatro líneas de código, definimos nuestro ciclo. El ciclo va de 1 a 1000, y el índice de cada iteración se denominayo . En cada iteración, nuestro tamaño de muestra es simplemente el valor de i , por lo que la primera iteración simula una observación y la milésima iteración simula 1000 observaciones. Por tanto, el tamaño de la muestra aumenta con cada pasada del bucle. En cada pasada del programa, R  toma muestras de un  norte( 0 , 1 ) distribución y luego toma la media de esa muestra. Cada media se registra en la i- ésima celda de almacenamiento . Una vez que se cierra el ciclo, graficamos nuestras medias muestrales contra el tamaño de la muestra y usamos abline para dibujar una línea roja en la media poblacional de cero. El resultado se muestra en la figura  11.3 . De hecho, nuestro gráfico muestra la media de la muestra que converge a la media real de cero a medida que aumenta el tamaño de la muestra.
Abrir imagen en nueva ventanaFigura 11.3
Figura 11.3
La ley de los grandes números y los bucles en acción.

Los bucles son necesarios para muchos tipos de programas, por ejemplo, si desea hacer un análisis de Monte Carlo. En algunos casos (pero no en todos), sin embargo, los bucles pueden ser más lentos que las versiones vectorizadas de los comandos. Puede valer la pena probar el comando apply , por ejemplo, si puede lograr el mismo objetivo que un bucle. Cuál es más rápido a menudo depende de la complejidad de la función y la sobrecarga de memoria para calcular y almacenar resultados. Entonces, si un enfoque es demasiado lento o demasiado complicado para que su computadora lo maneje, puede valer la pena probar el otro.

11.4 Ramificación
Los usuarios de R tienen la opción de ejecutar comandos condicionados a una declaración booleana usando el comando if . Esto puede ser útil siempre que el usuario solo quiera implementar algo para ciertos casos, o si los diferentes tipos de datos deben tratarse de manera diferente. La sintaxis básica de una instrucción if es:

si (expresión_lógica) {

     expresión_1

     ...

}

En este marco, la expresión_lógica es una declaración booleana, y la expresión_1 representa los comandos que el usuario quisiera aplicar siempre que la expresión lógica sea verdadera.

Como un ejemplo de juguete de cómo funciona esto, supongamos que quisiéramos simular un proceso en el que dibujamos dos números del 1 al 10 (con números repetidos permitidos). El comando de muestra nos permite simular este proceso fácilmente, y si lo incrustamos en un bucle for podemos repetir el experimento varias veces (digamos, 100 intentos). Si quisiéramos saber en cuántos ensayos aparecieron ambos números, podríamos determinar esto con una declaración if . Nuestro código se junta así:

even.count <-0

para (i en 1: 100) {

     a <-muestra (c (1:10), 2, reemplazar = VERDADERO)

     si (suma (a %% 2) == 0) {

          even.count <-even.count + 1

          }

}

even.count

La primera línea crea un escalar llamado even.count y lo establece en cero. La siguiente línea inicia el ciclo que nos da 100 pruebas. La tercera línea crea nuestra muestra de dos números del 1 al 10 y nombra la muestra a . La cuarta línea define nuestra declaración if : usamos la función módulo para encontrar el resto cuando cada término de nuestra muestra se divide por dos. 4 Si el resto de cada término es cero, entonces todos los números son pares y la suma es cero. En ese caso, hemos extraído una muestra en la que los números son pares. Por lo tanto, cuando sum (a %% 2) == 0es cierto, entonces queremos agregar uno a un recuento continuo de cuántas muestras de dos números pares tenemos. Por lo tanto, la quinta línea de nuestro código se suma a nuestro recuento, pero solo en los casos que cumplen con nuestra condición. (Observe que una asignación recursiva para even.count es aceptable. R  tomará el valor anterior, le agregará uno y actualizará el valor guardado). Pruebe este experimento usted mismo. Como sugerencia, la teoría de la probabilidad diría que el 25% de los ensayos producirán dos números pares, en promedio.

Los usuarios también pueden hacer declaraciones if ... else de bifurcación de modo que un conjunto de operaciones se aplique siempre que una expresión sea verdadera, y otro conjunto de operaciones se aplique cuando la expresión sea falsa. Esta estructura básica se ve así:

si (expresión_lógica) {

     expresión_1

     ...

} demás {

     expresión_2

     ...

}

En este caso, expression_1 solo se aplicará en los casos en que la expresión lógica sea verdadera, y expression_2 solo se aplicará en los casos en que la expresión lógica sea falsa.

Finalmente, los usuarios tienen la opción de ramificarse aún más. En los casos en los que la primera expresión lógica es falsa, por lo que se llama a las expresiones que siguen a else , estos casos se pueden ramificar de nuevo con otra instrucción if ... else . De hecho, el programador puede anidar tantas declaraciones if ... else como desee. Para ilustrar esto, considere nuevamente el caso cuando simulamos dos números aleatorios del 1 al 10. Imagine que esta vez queremos saber no solo con qué frecuencia sacamos dos números pares, sino también con qué frecuencia sacamos dos números impares y con qué frecuencia sacamos un número par y uno impar. Una forma en que podríamos abordar esto es manteniendo tres recuentos en ejecución (a continuación, denominados even.count ,odd.count y split.count ) y agregando declaraciones de bifurcación adicionales:

even.count <-0

cuenta impar <-0

split.count <-0

para (i en 1: 100) {

     a <-muestra (c (1:10), 2, reemplazar = VERDADERO)

     si (suma (a %% 2) == 0) {

          even.count <-even.count + 1

          } más si (suma (a %% 2) == 2) {

               recuento.imor <-cuenta.odd + 1

                    } demás{

                         split.count <-split.count + 1

                         }

}

even.count

cuenta impar

split.count

Nuestro ciclo for comienza igual que antes, pero después de nuestra primera instrucción if , seguimos con una instrucción else . Cualquier muestra que no consista en dos números pares ahora está sujeta a los comandos debajo de else , y el primer comando debajo de else es ... otra instrucción if . El siguiente enunciado if observa que si ambos términos de la muestra son impares, la suma de los residuos después de dividir por dos será dos. Por lo tanto, todas las muestras con dos entradas impares ahora están sujetas a los comandos de esta nueva instrucción if , donde vemos que nuestro índice de cuenta impar aumentará en uno. Por último, tenemos un resto finalinstrucción: todas las muestras que no constan de dos números pares o impares ahora estarán sujetas a este conjunto final de comandos. Dado que estas muestras constan de un número par y uno impar, el índice split.count aumentará en uno. Pruébelo usted mismo. Nuevamente, como sugerencia, la teoría de la probabilidad indica que, en promedio, el 50% de las muestras debe constar de un número par y uno impar, el 25% debe constar de dos números pares y el 25% debe constar de dos números impares.

11.5 Optimización y estimación de máxima verosimilitud
El comando optim en R  permite a los usuarios encontrar el mínimo o máximo de una función usando una variedad de métodos de optimización numérica . 5 En otras palabras, optim tiene varios algoritmos que buscan de manera eficiente el valor más alto o más bajo de una función, varios de los cuales permiten al usuario restringir los valores posibles de las variables de entrada. Si bien hay muchos usos de la optimización en la investigación de ciencias políticas, el más común es la estimación de máxima verosimilitud (MLE). 6

El comando optim , junto con la definición de función simple de R (discutida en la Sección  11.2 ), le permite a R  usar fácilmente la flexibilidad total de MLE. Si un modelo enlatado como los descritos en los Cap. 1 –7 no se adapta a su investigación y desea derivar su propio estimador utilizando MLE, entonces R  puede adaptarse a usted. 

Para ilustrar cómo funciona la programación de un MLE en R , considere un ejemplo simple: el estimador del parámetro de probabilidad ( π ) en una distribución binomial. A modo de recordatorio, la motivación detrás de una distribución binomial es que estamos interesados ​​en una prueba que toma uno de dos resultados cada vez, y repetimos esa prueba varias veces. El ejemplo más simple es que lanzamos una moneda, que saldrá cara o cruz en cada lanzamiento. Usando este ejemplo, suponga que nuestros datos consisten en 100 lanzamientos de moneda y 43 de los lanzamientos salieron cara. ¿Cuál es la estimación MLE de la probabilidad de que salga cara? Por intuición o derivación, debes saber que  π^=43100= 0,43 . Sin embargo, seguiremos estimando esta probabilidad en R  para practicar en casos más complicados.

Para definir más formalmente nuestro problema, una distribución binomial se motiva al completar n ensayos de Bernoulli, o ensayos que pueden terminar en un éxito o un fracaso. Registramos el número de veces que tenemos éxito en nuestras n pruebas como y . Además, por definición, nuestro parámetro de probabilidad ( π ) debe ser mayor o igual a cero y menor o igual a uno. Nuestra función de verosimilitud es:
L ( π| n , y) =πy( 1 - π)n - y
(11,2)
Para facilitar el cálculo y el cálculo, podemos obtener un resultado equivalente maximizando nuestra función logarítmica de verosimilitud:
ℓ ( π| n , y) = registroL ( π| n , y) = y⋅ registro( π) + ( n - y) registro( 1 - π)
(11,3)
Podemos definir fácilmente nuestra función de probabilidad logarítmica en R  con el comando de función :
binomial.loglikelihood <- función (prob, y, n) {

   loglikelihood <- y * log (prob) + (ny) * log (1-prob)

   retorno (loglikelihood)

}

Ahora para estimar   π^ , necesitamos R para encontrar el valor de π que maximiza la función logarítmica de verosimilitud dados los datos. (Llamamos a este término prob en el código para evitar confusiones con el uso que hace R del comando pi para almacenar la constante geométrica). Podemos hacer esto con el comando optim . Calculamos:

test <- optim (c (.5), # valor inicial para prob

   binomial.loglikelihood, # la función logarítmica de verosimilitud

   method = "BFGS", # método de optimización

   arpillera = VERDADERO, # devuelve arpillera numérica

   control = lista (fnscale = -1), # maximizar en lugar de minimizar

   y = 43, n = 100) # los datos

imprimir (prueba)

Recuerda que todo lo que sigue a un signo de almohadilla ( # ) es un comentario que R  ignora, por lo que estas notas sirven para describir cada línea de código. Siempre comenzamos con un vector de valores iniciales con todos los parámetros para estimar (solo π en este caso), nombramos la función de probabilidad logarítmica que definimos en otro lugar, elegimos nuestro método de optimización ( B royden- F letcher- G oldfarb- S hanno es a menudo una buena elección) e indicar que queremos que R  devuelva el hessiano numérico para poder calcular los errores estándar más tarde. La quinta línea de código es de vital importancia: de forma predeterminada, optim es un minimizador , por lo que tenemos que especificarfnscale = -1 para convertirlo en un maximizador . Siempre que utilice optim para la estimación de máxima verosimilitud, deberá incluir esta línea. En la sexta línea, enumeramos nuestros datos. A menudo, aquí llamaremos una matriz o un marco de datos, pero en este caso solo necesitamos enumerar los valores de y y n .

Nuestro resultado de impresión (prueba) se ve así:

$ par

[1] 0,4300015

$ valor

[1] -68.33149

$ cuenta

gradiente de función

      13 4

$ convergencia

[1] 0

$ mensaje

NULO

$ arpillera

          [, 1]

[1,] -407,9996

Para interpretar nuestra salida: El término par enumera las estimaciones de los parámetros, por lo que nuestra estimación es  π^= 0,43 (como anticipamos). La probabilidad logarítmica de nuestra solución final es - 68. 33149, y se presenta bajo valor . El término cuenta nos dice con qué frecuencia optim tuvo que llamar a la función y al gradiente. El término convergencia se codificará como 0 si la optimización se completó con éxito; cualquier otro valor es un código de error. El elemento de mensaje puede devolver otra información necesaria del optimizador. Por último, la arpillera es nuestra matriz de segundas derivadas de la función logarítmica de verosimilitud. Con solo un parámetro aquí, es una matriz simple de 1 × 1. En general, si el usuario quiere errores estándar a partir de un modelo de máxima verosimilitud estimada, la siguiente línea los devolverá:

sqrt (diag (resolver (-prueba $ arpillera)))

Esta línea se basa en la fórmula para errores estándar en la estimación de máxima verosimilitud. Todo lo que el usuario necesitará cambiar es reemplazar la palabra prueba con el nombre asociado con la llamada a optim . En este caso, R  informa que el error estándar es  S E (π^) = 0.0495074 .

Finalmente, en este caso en el que tenemos un solo parámetro de interés, tenemos la opción de usar nuestra función de probabilidad logarítmica definida para dibujar una imagen del problema de optimización. Considere el siguiente código:

regla <- seq (0,1,0.01)

loglikelihood <- binomial.loglikelihood (regla, y = 43, n = 100)

plot (regla, loglikelihood, type = "l", lwd = 2, col = "blue",

   xlab = expresión (pi), ylab = "Log-Likelihood", ylim = c (-300, -70),

   main = "Log-Likelihood for Binomial Model")

abline (v = .43)

La primera línea define todos los valores que posiblemente pueda tomar π . La segunda línea inserta en la función logarítmica de verosimilitud el vector de valores posibles de π , más los valores verdaderos de y y n . Las líneas tercera a quinta son una llamada a graficar que nos da un gráfico lineal de la función logarítmica de verosimilitud. La última línea dibuja una línea vertical en nuestro valor estimado para  π^ . El resultado de esto se muestra en la Fig.  11.4 . Si bien las funciones log-verosimilitud con muchos parámetros generalmente no se pueden visualizar, esto nos recuerda que la función que hemos definido aún se puede usar para fines distintos a la optimización, si es necesario.
Abrir imagen en nueva ventanaFigura 11.4
Figura 11.4
Logaritmo de verosimilitud binomial en todos los valores posibles del parámetro de probabilidad π cuando los datos constan de 43 éxitos en 100 ensayos

11.6 Programación orientada a objetos
Esta sección da un giro hacia lo avanzado, al presentar una aplicación en programación orientada a objetos. Si bien esta aplicación sintetiza muchas de las otras herramientas discutidas en este capítulo, es menos probable que los usuarios novatos utilicen la programación orientada a objetos que las características discutidas hasta ahora. Dicho esto, para usos avanzados, el trabajo orientado a objetos puede ser beneficioso.

Como se mencionó en el Cap. 1 , R  es un entorno orientado a objetos. Esto significa que usted, como investigador, tiene la oportunidad de utilizar sofisticadas herramientas de programación en su propia investigación. En la programación orientada a objetos, crea una clase de elemento que tiene una variedad de características. Luego, puede crear elementos dentro de la clase (llamados objetos o variables ) que tendrán características únicas. En R , puede crear clases a partir de los sistemas de objetos S3 o S4 .  

Para ilustrar, un "modelo lineal" es una clase de objetos creados por el comando lm . Es una clase S3 . De vuelta en el Cap. 6 , estimamos un modelo que llamamos mod.hours , que era un objeto de la clase de modelo lineal. Este objeto tenía características que incluyen coeficientes , residuos y cov . Sin escala . En todos los objetos de la clase S3 , podemos llamar a una característica escribiendo el nombre del objeto, el signo de dólar y el nombre de la característica que queremos. Por ejemplo, mod.hours $ coefficients enumeraría el vector de coeficientes de ese modelo. ( T4 utiliza referencias ligeramente diferentes, que se describen más adelante.) Cuando defina sus propios objetos, puede utilizar el sistema de objetos S3 o S4 . El siguiente ejemplo guarda nuestras salidas en el sistema de objetos S3 , aunque las notas al pie ilustran cómo podría usar el sistema de objetos S4 si lo prefiere.

11.6.1 Simular un juego
Como ejemplo práctico de programación orientada a objetos, consideramos una versión algo simplificada del trabajo presentado en Monogan (2013b). 7 Tocamos este modelo en la Secta. 11.2 , presentando la función de utilidad de un actor. En resumen, este artículo desarrolla un modelo de teoría de juegos de cómo dos partes elegirán una posición sobre un tema de acuerdo con el modelo de proximidad espacial de la política. El modelo espacial, descrito algo en el Cap. 7 , asume que las posiciones de los problemas se pueden representar como ubicaciones en el espacio. Los votantes eligen partidos que adoptan posiciones de emisión más cercanas a su propia posición en el espacio de emisión, por lo que los partidos tratan de maximizar los votos eligiendo estratégicamente sus posiciones de emisión. 

En el juego que consideramos, la motivación sustantiva es el hecho de que la opinión pública muestra fuertes tendencias en ciertos temas de política debido a tendencias demográficas o diferencias generacionales. Temas como este incluyen la política ambiental, la política de inmigración y la protección de los derechos de los homosexuales. Suponemos que dos partidos competirán en dos elecciones. Sin embargo, para considerar el hecho de que los partidos deben tener en cuenta el futuro y pueden rendir cuentas por posiciones de asuntos pasados, la posición que toman en la primera elección es la posición en la que están atrapados en la segunda elección. También asumimos que la posición del votante mediano cambia de una elección a la siguiente de una manera que los partidos anticipan, ya que en estos temas las tendencias de la opinión pública suelen ser claras.

Otra característica es que un partido tiene ventaja en la primera elección (debido a la titularidad o algún otro factor), por lo que los jugadores son el partido A (un partido que no tiene ventaja de valencia en la primera elección) y el partido D (una parte desfavorecida). Se supone que la segunda elección es menos valiosa que la primera, porque la recompensa está más lejos. Finalmente, los votantes también consideran factores desconocidos para los partidos, por lo que el resultado de la elección es más probabilístico que seguro. En este juego, entonces, los partidos intentan maximizar su probabilidad de ganar cada una de las dos elecciones. ¿Cómo se posicionan ante un tema que cambia la opinión pública?

La característica engañosa de este juego es que no se puede resolver algebraicamente (Monogan 2013b, pag. 288). Por lo tanto, recurrimos a R  para ayudarnos a encontrar soluciones a través de la simulación. Lo primero que debemos hacer es limpiar y definir las funciones de utilidad esperadas para las partes A y D :

rm (lista = ls ())

Función cuadrática.A <(m.1, m.2, p, delta, theta.A, theta.D) {

    util.a <-plogis (- (m.1-theta.A) ^ 2 +

         (m.1-theta.D) ^ 2 + p) +

       delta * plogis (- (m.2-theta.A) ^ 2 +

         (m.2-theta.D) ^ 2)

    volver (util.a)

    }

D <-función cuadrática (m.1, m.2, p, delta, theta.A, theta.D) {

    util.d <- (1-plogis (- (m.1-theta.A) ^ 2 +

         (m.1-theta.D) ^ 2 + p)) +

       delta * (1-plogis (- (m.2-theta.A) ^ 2 +

         (m.2-theta.D) ^ 2))

    volver (util.d)

    }

Ya consideramos la función Quadratic.A en la secc. 11.2 , y está formalmente definido por la Ec. ( 11,1 ). Quadratic.D es similar en estructura, pero produce diferentes utilidades. Ambas funciones se utilizarán en las simulaciones que hagamos.

Nuestro siguiente paso es definir una función larga que usa nuestras funciones de utilidad para ejecutar una simulación y produce una salida con el formato que queremos. 8 Todo el siguiente código es una gran definición de función. Los comentarios después de los signos de almohadilla ( # ) se incluyen nuevamente para ayudar a distinguir cada parte del cuerpo de la función. La función, llamada simular , simula nuestro juego de interés y guarda nuestros resultados en un objeto de clase game.simulation :

simular <-función (v, delta, m.2, m.1 = 0.7, theta = seq (-1,1, .1)) {

    #definir parámetros internos, matrices y vectores

    precisión <-longitud (theta)

    resultA <-matrix (NA, precisión, precisión)

    resultD <-matrix (NA, precisión, precisión)

    bestResponseA <-rep (NA, precisión)

    bestResponseD <-rep (NA, precisión)

    equilibrioA <- 'NA'

    equilibrioD <- 'NA'

    #matrix atributos

    nombres de fila (resultA) <- colnames (resultA) <- nombres de fila (resultD) <-

         colnames (resultD) <- nombres (bestResponseA) <-

         nombres (bestResponseD) <- theta

    # Complete las utilidades para todas las estrategias para el partido A

    para (i en 1: precisión) {

            para (j en 1: precisión) {

                resultadoA [i, j] <- Cuadrático.A (m.1, m.2, v, delta,

                     theta [i], theta [j])

                }

        }

    #utilidades para la fiesta D

    para (i en 1: precisión) {

            para (j en 1: precisión) {

                resultadoD [i, j] <- Cuadrático.D (m.1, m.2, v, delta,

                     theta [i], theta [j])

                }

        }

    #mejores respuestas para la fiesta A

    para (i en 1: precisión) {

        bestResponseA [i] <- which.max (resultA [, i])

        }

    #mejores respuestas para la fiesta D

    para (i en 1: precisión) {

        bestResponseD [i] <- which.max (resultD [i,])

        }

    #encuentra los equilibrios

    para (i en 1: precisión) {

        if (bestResponseD [bestResponseA [i]] == i) {

            equilibriumA <-dimnames (resultA) [[1]] [

                 bestResponseA [i]]

            equilibriumD <-dimnames (resultD) [[2]] [

                 bestResponseD [bestResponseA [i]]]

                }

    }

    # guardar la salida

    resultado <-lista (resultadoA = resultadoA, resultadoD = resultadoD,

        bestResponseA = bestResponseA, bestResponseD = bestResponseD,

        equilibrio A = equilibrio A, equilibrio D = equilibrio D)

    clase (resultado) <- "juego.simulación"

    invisible (resultado)

}

Como consejo general, al escribir una función larga, es mejor probar el código fuera del contenedor de la función . Para obtener puntos de bonificación y tener una idea completa de este código, es posible que desee dividir esta función en sus componentes y ver cómo funciona cada pieza. Como muestran los comentarios a lo largo de todo, cada conjunto de comandos hace algo que normalmente haríamos en una función. Observe que cuando se definen los argumentos, tanto m.1 como thetase les dan valores predeterminados. Esto significa que en usos futuros, si no especificamos los valores de estas entradas, la función utilizará estos valores predeterminados, pero si los especificamos, los valores predeterminados se anularán. Pasando a los argumentos dentro de la función, comienza definiendo parámetros internos y estableciendo sus atributos. Cada conjunto de comandos a partir de entonces es un ciclo dentro de un ciclo para repetir ciertos comandos y llenar vectores y matrices de salida.

La adición clave aquí que es exclusiva de todo lo que se discutió anteriormente está en la definición del término de resultado al final de la función. Observe que al hacer esto, primero definimos la salida como una lista , en la que se nombra cada componente. En este caso, ya nombramos nuestros objetos de la misma manera que queremos etiquetarlos en la lista de salida, pero puede que no siempre sea así. 9 Luego usamos el comando class para declarar que nuestra salida es de la clase game.simulation , un concepto que estamos creando ahora. El comando de clase lo formatea como un objeto de la familia S3 . Al escribir invisible (resultado)al final de la función, sabemos que nuestra función devolverá este objeto game.simulation -class . 10

Ahora que esta función está definida, podemos ponerla en práctica. Refiriéndose a los términos de la Ec. ( 11.1 ), suponga que V  = 0, 1, δ  = 0, m 1  = 0, 7 y m 2  = −0. 1. En el siguiente código, creamos un nuevo objeto llamado tratamiento.1 usando la función de simulación cuando los parámetros toman estos valores, y luego le pedimos a R  que imprima la salida:

tratamiento.1 <-simular (v = 0.1, delta = 0.0, m.2 = -0.1)

tratamiento.1

Observe que no especificamos m.1 porque 0.7 ya es el valor predeterminado para ese parámetro. El resultado de la impresión de treatment.1 es demasiado extenso para reproducirlo aquí, pero en su propia pantalla verá que treatment.1 es un objeto de la clase game.simulation , y la impresión informa los valores de cada atributo.

Si solo estamos interesados ​​en una característica particular de este resultado, podemos pedirle a R  que devuelva solo el valor de una ranura específica. Para un objeto S3 , que es cómo guardamos nuestro resultado, podemos llamar a un atributo nombrando el objeto, usando el símbolo $ y luego nombrando el atributo que queremos usar. (Esto contrasta con los objetos S4 , que usan el símbolo @ para llamar ranuras). Por ejemplo, si solo quisiéramos ver cuáles eran las opciones de equilibrio para las partes A y D , simplemente podríamos escribir:

tratamiento.1 $ equilibrioA

tratamiento.1 $ equilibrioD

Cada uno de estos comandos devuelve el mismo resultado:

[1] "0,7"

Entonces, sustancialmente, podemos concluir que el equilibrio para el juego bajo estos parámetros es θ A  =  θ D  = 0. 7, que es el punto ideal del votante mediano en la primera elección. En esencia, esto debería tener sentido porque establecer δ  = 0 es un caso especial cuando los partidos no están en absoluto preocupados por ganar la segunda elección, por lo que se posicionan estrictamente para la primera elección. 11

Para dibujar un contraste, ¿qué pasa si realizamos una segunda simulación, pero esta vez aumentamos el valor de δ a 0.1? También podríamos usar valores más detallados de las posiciones que podrían tomar las partes, estableciendo su posición hasta las centésimas de decimal, en lugar de décimas. Podríamos hacer esto de la siguiente manera:

tratamiento.2 <-simular (v = 0.1, delta = 0.1, m.2 = -0.1,

     theta = seq (-1,1, .01))

tratamiento.2 $ equilibrioA

tratamiento.2 $ equilibrioD

Logramos la precisión más fina al sustituir nuestro propio vector por theta . Nuestros valores de salida de las dos ranuras de equilibrio son nuevamente los mismos:

[1] "0,63"

Entonces sabemos que bajo este segundo tratamiento, el equilibrio para el juego es θ A  =  θ D  = 0. 63. Sustancialmente, lo que sucedió aquí es que aumentamos el valor de ganar la segunda elección para los partidos. Como resultado, los partidos movieron su posición de tema un poco más cerca de la preferencia de tema del votante medio en la segunda elección.

11.7 Análisis de Monte Carlo: un ejemplo aplicado
Como ejemplo aplicado que sintetiza varias de las herramientas desarrolladas en este capítulo, ahora realizamos un análisis de Monte Carlo. La lógica básica del análisis de Monte Carlo es que el investigador genera datos conociendo el verdadero modelo de población. Luego, el investigador pregunta si las estimaciones muestrales de un determinado método tienen buenas propiedades, dadas las cantidades de población. Los tratamientos comunes en un experimento de Monte Carlo incluyen: elección de estimador, tamaño de muestra, distribución de predictores, varianza de errores y si el modelo tiene la forma funcional correcta (por ejemplo, ignorar una relación no lineal u omitir un predictor). Al probar varios tratamientos, un usuario puede tener una idea comparativa de qué tan bien funciona un estimador.

En este caso, presentaremos Signorino's (1999) modelo probit multinomial estratégico como estimador que usaremos en nuestro experimento de Monte Carlo. La idea detrás de este enfoque es que, cuando una situación política recurrente se puede representar con un juego (como las disputas internacionales militarizadas), podemos desarrollar un modelo empírico de los resultados que pueden ocurrir (como la guerra) basado en la estrategia del gobierno. juego. Cada resultado posible tiene una función de utilidadpara cada jugador, ya sea que el jugador sea un individuo, un país u otro actor. La utilidad representa cuánto beneficio obtiene el jugador del resultado. En esta configuración, utilizamos predictores observables para modelar las utilidades para cada resultado posible. Este tipo de modelo es interesante porque el proceso de generación de datos de población generalmente tiene no linealidades que no son capturadas por enfoques estándar. Por lo tanto, tendremos que programar nuestra propia función de probabilidad y usar optim para estimarla. 12

Para motivar cómo se establece el probit estratégico multinomial, considere un modelo sustantivo de comportamiento entre dos naciones, un agresor (1) y un objetivo (2). Este es un juego de dos etapas: primero, el agresor decide si atacar (A) o no (∼ A); entonces, el objetivo decide si defender (D) o no (∼ D). Pueden ocurrir tres consecuencias observables: si el agresor decide no atacar, el status quo se mantiene; si el agresor ataca, el objetivo puede optar por capitular ; finalmente, si el objetivo defiende, tenemos guerra . El árbol del juego se presenta en la Fig.  11.5 . Al final de cada rama están las utilidades para los jugadores (1) y (2) para el status quo (SQ), la capitulación (C) y la guerra.(W), respectivamente. Suponemos que estos jugadores son racionales y, por lo tanto, elegimos las acciones que maximizan la recompensa resultante. Por ejemplo, si el objetivo es débil, la recompensa de la guerra U 2 ( W ) sería terriblemente negativa y probablemente menor que la recompensa de la capitulación U 2 ( C ). Si el agresor lo sabe, puede deducir que en caso de ataque, el objetivo capitularía. Por lo tanto, el agresor sabría que si elige atacar, recibirá la recompensa U 1 ( C ) (y no U 1 ( W )). Si la recompensa de la capitulación U 1 (C ) es más grande que la recompensa del status quo U 1 (SQ) para el agresor, la decisión racional es atacar (y la decisión racional del objetivo es capitular). Por supuesto, el objetivo es tener una idea de lo que sucederá a medida que cambien las circunstancias en diferentes díadas de naciones.
Abrir imagen en nueva ventanaFigura 11.5
Figura 11.5
Modelo de disuasión estratégica

En general, asumimos que las funciones de utilidad responden a circunstancias específicas observables (el valor de los recursos en disputa, el precio diplomático esperado debido a las sanciones en caso de agresión, el poderío militar, etc.). También asumiremos que las utilidades para la elección de cada país son estocásticas. Esto introduce incertidumbre en nuestro modelo de pagos, lo que representa el hecho de que este modelo está incompleto y probablemente excluye algunos parámetros relevantes de la evaluación. En este ejercicio, consideraremos solo cuatro predictores X i para modelar las funciones de utilidad y asumiremos que los pagos son lineales en estas variables.

En particular, consideramos la siguiente forma paramétrica para los pagos:
U1( S Q )U1( C)U1( W)U2( C)U2( W)α=====∼0X1β1X2β20X3β3+X4β4norte( 0 , 0,5 )
(11,4)
Cada nación hace su elección basándose en qué decisión le dará una mayor utilidad. Esto se basa en la información conocida, más una perturbación privada desconocida ( α ) para cada elección. Esta perturbación privada agrega un elemento aleatorio a las decisiones de los actores. Nosotros, como investigadores, podemos mirar los conflictos pasados, medir los predictores ( X ), observar el resultado y nos gustaría inferir: ¿Qué importancia tienen X 1 , X 2 , X 3 y X 4 en el comportamiento de las díadas-nación? Tenemos que determinar esto basándonos en si cada punto de datos resultó en un status quo , una capitulación o una guerra .
11.7.1 Función log-verosimilitud de disuasión estratégica
Podemos determinar estimaciones de cada   β^ utilizando la estimación de máxima verosimilitud. Primero, tenemos que determinar la probabilidad ( p ) que el objetivo (2) defenderá si es atacado:
p = P( D )===PAG(U2( D ) +α2 D>U2( ∼ D ) +α2 ∼ D)PAG(U2( D ) -U2( ∼ D ) >α2 ∼ D-α2 D)Φ (X3β3+X4β4)
(11,5)
Donde Φ es la función de distribución acumulativa para una distribución normal estándar. Si conocemos p , podemos determinar la probabilidad ( q ) de que el agresor (1) ataque:
q= P( A )===PAG(U1( A ) +α1 A>Ua( ∼ A ) +α1 ∼ A)PAG(U1( A ) -U1( ∼ A ) >α1 ∼ A-α1 A)Φ ( pX2β2+ ( 1 - p )X1β1)
(11,6)
Observe que p está en la ecuación de q . Esta característica no lineal del modelo no se adapta a los modelos enlatados estándar.
Conocer las fórmulas para p y q , sabemos que las probabilidades de status quo, la capitulación, y la guerra. Por lo tanto, la función de verosimilitud es simplemente el producto de las probabilidades de cada evento, elevado a una variable ficticia de si el evento sucedió, multiplicado por todas las observaciones:
L ( β | y , X ) =∏i = 1norte( 1 - q)DS Q× ( q( 1 - p ))DC× ( p q)DW
(11,7)
Donde D SQ , D C y D W son variables ficticias iguales a 1 si el caso es status quo, capitulación o guerra, respectivamente, y 0 en caso contrario. La función logarítmica de verosimilitud es:
ℓ ( β | y , X ) =∑i = 1norteDS QIniciar sesión( 1 - q) +DCIniciar sesiónq( 1 - p ) +DWIniciar sesión( p q)
(11,8)
Ahora estamos listos para empezar a programar esto en R . Comenzamos limpiando y luego definiendo nuestra función log-verosimilitud como llik , que nuevamente incluye comentarios para describir partes del cuerpo de la función:

rm (lista = ls ())

llik = función (B, X, Y) {

  #Separe matrices de datos para variables individuales:

  sq = as.matrix (Y [, 1])

  cap = as.matrix (Y [, 2])

  war = as.matrix (Y [, 3])

  X13 = as.matrix (X [, 1])

  X14 = as.matrix (X [, 2])

  X24 = as.matrix (X [, 3: 4])

  # Coeficientes separados para cada ecuación:

  B13 = como.matriz (B [1])

  B14 = como.matriz (B [2])

  B24 = como.matriz (B [3: 4])

  # Definir utilidades como variables multiplicadas por coeficientes:

  U13 = X13% *% B13

  U14 = X14% *% B14

  U24 = X24% *% B24

  #Calcular la probabilidad de que 2 pelee (P4) o no (P3):

  P4 = pnorm (U24)

  P3 = 1-P4

  #Calcular la probabilidad de que 1 ataque (P2) o no (P1):

  P2 = norma ((P3 * U13 + P4 * U14))

  P1 = 1-P2

  # Definir y devolver la función de probabilidad logarítmica:

  lnpsq = log (P1)

  lnpcap = log (P2 * P3)

  lnpwar = log (P2 * P4)

  llik = sq * lnpsq + cap * lnpcap + war * lnpwar

  retorno (suma (llik))

}

Aunque sustancialmente más largo que la función de verosimilitud que definimos en la Sec. 11.5 , la idea es la misma. La función aún acepta valores de parámetros, variables independientes y variables dependientes, y aún devuelve un valor de probabilidad logarítmica. Sin embargo, con un modelo más complejo, la función debe dividirse en pasos de componentes. Primero, nuestros datos ahora son matrices, por lo que el primer lote de código separa las variables por ecuación del modelo. En segundo lugar, nuestros parámetros son todos coeficientes almacenados en el argumento B, por lo que necesitamos separar los coeficientes por ecuación del modelo. En tercer lugar, multiplicamos por matrices las variables por los coeficientes para crear los tres términos de utilidad. Cuarto, usamos esa información para calcular las probabilidades de que el objetivo se defienda o no. En quinto lugar, utilizamos las utilidades, más la probabilidad de las acciones del objetivo, para determinar la probabilidad de que el agresor ataque o no. Por último, las probabilidades de todos los resultados se utilizan para crear la función de probabilidad logarítmica.

11.7.2 Evaluación del estimador
Ahora que hemos definido nuestra función de verosimilitud, podemos usarla para simular datos y ajustar el modelo sobre nuestros datos simulados. Con cualquier experimento de Monte Carlo, debemos comenzar por definir el número de experimentos que realizaremos para un tratamiento determinado y el número de puntos de datos simulados en cada experimento. También necesitamos definir espacios vacíos para los resultados de cada experimento. Escribimos:

set.seed (3141593)

i <-100 # número de experimentos

n <-1000 # número de casos por experimento

beta.qre <-matriz (NA, i, 4)

stder.qre <-matriz (NA, i, 4)

Comenzamos usando set.seed para hacer que nuestros resultados de Monte Carlo sean más replicables. Aquí dejamos que i sea ​​nuestro número de experimentos, que establecemos en 100. (Aunque normalmente podríamos preferir un número mayor). Usamos n como nuestro número de casos. Esto nos permite definir beta.qre y stder.qre como las matrices de salida para nuestras estimaciones de los coeficientes y errores estándar de nuestros modelos, respectivamente.

Con esto en su lugar, ahora podemos ejecutar un gran ciclo que simulará repetidamente un conjunto de datos, estimará nuestro modelo probit estratégico multinomial y luego registrará los resultados. El bucle es el siguiente:

para (j en 1: i) {

     #Simular variables causales

     x1 <-rnorm (n)

     x2 <-rnorm (n)

     x3 <-rnorm (n)

     x4 <-rnorm (n)

     #Crear utilidades y términos de error

     u11 <-rnorm (n, sd = sqrt (.5))

     u13 <-x1

     u23 <-rnorm (n, sd = sqrt (.5))

     u14 <-x2

     u24 <-x3 + x4 + rnorm (n, sd = sqrt (.5))

     pR <-pnorm (x3 + x4)

     uA <- (pR * u14) + ((1-pR) * u13) + rnorm (n, sd = sqrt (.5))

     #Crear variables dependientes

     sq <-rep (0, n)

     capit <-rep (0, n)

     guerra <-rep (0, n)

     sq [u11> = uA] <- 1

     capit [u11 <uA & u23> = u24] <- 1

     guerra [u11 <uA & u23 <u24] <- 1

     Nsq <-abs (1 cuadrado)

     #Matrices para entrada

     stval <-rep (.1,4)

     depvar <-cbind (sq, capit, war)

     indvar <-cbind (x1, x2, x3, x4)

     #Modelo de ajuste

     strat.mle <-optim (stval, llik, hessian = TRUE, method = "BFGS",

          control = lista (maxit = 2000, fnscale = -1, trace = 1),

          X = indvar, Y = depvar)

     #Guardar resultados

     beta.qre [j,] <- estrat.mle $ par

     stder.qre [j,] <- sqrt (diag (solve (-strat.mle $ hessian)))

}

En este modelo, establecemos β  = 1 para cada coeficiente en el modelo de población. De lo contrario, Eq. ( 11.4 ) define completamente nuestro modelo de población. En el ciclo, los primeros tres lotes de código generan datos de acuerdo con este modelo. Primero, generamos cuatro variables independientes, cada una con una distribución normal estándar. En segundo lugar, definimos las utilidades de acuerdo con la ecuación. ( 11.4 ), agregando los términos de perturbación aleatoria ( α ) como se indica en la figura  11.5 . En tercer lugar, creamos los valores de las variables dependientes en función de los servicios públicos y las perturbaciones. Después de esto, el cuarto paso es limpiar nuestros datos simulados; definimos valores iniciales para optimy unir las variables dependientes e independientes en matrices. En quinto lugar, usamos optim para estimar realmente nuestro modelo, nombrando la estrategia de salida dentro de la iteración . Por último, los resultados del modelo se escriben en las matrices beta.qre y stder.qre .

Después de ejecutar este ciclo, podemos echar un vistazo rápido al valor promedio de nuestros coeficientes estimados (  β^ ) escribiendo:

aplicar (beta.qre, 2, media)

En esta convocatoria para postularse , estudiamos nuestra matriz de coeficientes de regresión en la que cada fila representa uno de los 100 experimentos de Monte Carlo, y cada columna representa uno de los cuatro coeficientes de regresión. Estamos interesados ​​en los coeficientes, por lo que escribimos 2 para estudiar las columnas y luego tomamos la media de las columnas. Si bien sus resultados pueden diferir un poco de lo que se imprime aquí, particularmente si no configuró la semilla para que sea la misma, la salida debería verse así:

[1] 1.0037491 1.0115165 1.0069188 0.9985754

En resumen, los cuatro coeficientes estimados están cerca de 1, lo cual es bueno porque 1 es el valor poblacional de cada uno. Si quisiéramos automatizar un poco más nuestros resultados para indicarnos el sesgo en los parámetros, podríamos escribir algo como esto:

desviarse <- barrido (beta.qre, 2, c (1,1,1,1))

colMeans (desviarse)

En la primera línea, usamos el comando de barrido , que barre (o resta) la estadística de resumen de nuestra elección de una matriz. En nuestro caso, beta.qre es nuestra matriz de estimaciones de coeficientes en 100 simulaciones. El argumento 2 que ingresamos por separado indica aplicar la estadística por columna (por ejemplo, por coeficiente) en lugar de por fila (lo que habría sido por experimento). Por último, en lugar de enumerar una estadística empírica que queremos restar, simplemente enumeramos los parámetros de población verdaderos para restar. En la segunda línea, calculamos el sesgo tomando la desviación media por column, o por parámetro. Nuevamente, la producción puede variar con diferentes semillas y generadores de números, pero en general debería indicar que los valores promedio no están lejos de los valores reales de la población. Todas las diferencias son pequeñas:

[1] 0,003749060 0,011516459 0,006918824 -0,001424579

Otra cantidad que vale la pena es el error absoluto medio, que nos dice cuánto difiere una estimación del valor de la población en promedio. Esto es un poco diferente en el sentido de que las sobreestimaciones y las subestimaciones no pueden desaparecer (como podría ocurrir con el cálculo del sesgo). Un estimador insesgado aún puede tener una gran varianza de error y un gran error medio absoluto. Como ya definimos desviarnos antes, ahora solo necesitamos escribir:

colMeans (abs (desviado))

Nuestra salida muestra pequeños errores absolutos promedio:

[1] 0.07875179 0.08059979 0.07169820 0.07127819

Para tener una idea de cuán bueno o malo es este rendimiento, deberíamos volver a ejecutar este experimento de Monte Carlo usando otro tratamiento para comparar. Para una comparación simple, podemos preguntar qué tan bien funciona este modelo si aumentamos el tamaño de la muestra. Presumiblemente, nuestro error absoluto medio disminuirá con una muestra más grande, por lo que en cada experimento podemos simular una muestra de 5000 en lugar de 1000. Para hacer esto, vuelva a ejecutar todo el código de esta sección del capítulo, pero reemplace una sola línea. Al definir el número de casos, la tercera línea después del inicio de la sección. 11.7.2 - en lugar de escribir n <-1000 , escriba en su lugar: n <-5000. Al final del programa, cuando se reportan los errores absolutos medios, verá que son más pequeños. Nuevamente, variarán de una sesión a otra, pero el resultado final debería verse así:

[1] 0.03306102 0.02934981 0.03535597 0.02974488

Como puede ver, los errores son más pequeños que con 1000 observaciones. Nuestro tamaño de muestra es considerablemente mayor, por lo que esperamos que este sea el caso. Esto nos da una buena comparación.

Con la finalización de este capítulo, ahora debería tener el kit de herramientas necesario para programar en R  siempre que su investigación tenga necesidades únicas que no puedan ser abordadas por comandos estándar, o incluso por paquetes aportados por el usuario. Con la finalización de este libro, debería tener una idea del amplio alcance de lo que R  puede hacer por los analistas políticos, desde la gestión de datos hasta los modelos básicos y la programación avanzada. Una verdadera fortaleza de R  es la flexibilidad que ofrece a los usuarios para abordar las complejidades y los problemas originales que puedan enfrentar. A medida que proceda a utilizar R  en su propia investigación, asegúrese de seguir consultando los recursos en línea para descubrir nuevas y prometedoras capacidades a medida que surjan.

11.8 Problemas de práctica
1.
Distribuciones de probabilidad: Calcule la probabilidad de cada uno de los siguientes eventos:
un.
Una variable estándar distribuida normalmente es mayor que 3.

 
B.
Una variable distribuida normalmente con media 35 y desviación estándar 6 es mayor que 42.

 
C.
X  <0,9 cuando x tiene la distribución uniforme estándar.

 
 
2.
Bucles: Let   h ( x , n ) = 1 + x +X2+ ⋯ +Xnorte=∑nortei = 0XI . Escriba un  programa en R para calcular h (0. 9, 25) usando un bucle for .

 
3.
Funciones: En la pregunta anterior, escribió un programa para calcular   h ( x , n ) =∑nortei = 0XI para x  = 0. 9 y n  = 25. Convierta este programa en una función más general que toma dos argumentos, x y n , y devuelve h ( x ,  n ). Usando la función, determine los valores de h (0. 8, 30), h (0. 7, 50) y h (0. 95, 20).

 
4.
Estimación de máxima verosimilitud. Considere un ejemplo aplicado de Signorino (1999) método probit estratégico multinomial. Descargue un subconjunto de disputas interestatales militarizadas del siglo XIX, el archivo war1800.dta en formato Stata , del Dataverse (consulte la página vii) o el contenido en línea de este capítulo (consulte la página 205). Estos datos provienen de fuentes como EUGene (Bueno de Mesquita y Lalman 1992) y el Proyecto Correlatos de Guerra (Jones et al. 1996). Programe una función de verosimilitud para un modelo como el que se muestra en la figura  11.5 y estime el modelo para estos datos reales. Los tres resultados son: guerra (codificado 1 si los países entraron en guerra), sq (codificado 1 si se mantuvo el status quo) y capit (codificado 1 si el país objetivo capituló). Suponga que U 1 (SQ) es impulsado por pacificadores (número de años desde que la díada estuvo en conflicto por última vez) y s_wt_re1 (puntaje S para la similitud política de los estados, ponderado por la región del agresor). U 1 ( W ) es una función de balanc(la capacidad militar del agresor en relación con la capacidad combinada de la díada). U 2 ( W ) es una función de una constante y un equilibrio . U 1 ( C ) es una constante y U 2 ( C ) es cero.
un.
Informe sus estimaciones y los errores estándar correspondientes.

 
B.
¿Qué coeficientes se distinguen estadísticamente de cero?

 
C.
Bonificación: Dibuje la probabilidad de guerra predicha si balanc se manipula desde su mínimo de 0 hasta su máximo de 1, mientras que pacifistas y s_wt_re1 se mantienen en sus mínimos teóricos de 0 y -1, respectivamente.
Simplemente cómico: si dibuja el mismo gráfico de probabilidades predichas, pero permite que balanc llegue hasta 5, realmente puede ilustrar el tipo de relación no monótona que permite este modelo. Sin embargo, recuerde que no desea interpretar resultados fuera del rango de sus datos. Esta es solo una ilustración divertida para practicar más.

 
 
5.
Análisis y optimización de Monte Carlo: Replica el experimento en Signorino's (1999) probit multinomial estratégico de la Sect. 11,7 . ¿Hay mucha diferencia entre sus valores promedio estimados y los valores de la población? ¿Obtiene errores medios absolutos similares a los informados en esa sección? ¿Cómo se comparan sus resultados cuando prueba los siguientes tratamientos?
un.
Suponga que disminuye la desviación estándar de x1 , x2 , x3 y x4 a 0.5 (en contraposición al tratamiento original que suponía una desviación estándar de 1). ¿Mejoran o empeoran sus estimaciones? ¿Qué sucede si reduce aún más la desviación estándar de estos cuatro predictores, a 0,25? ¿Qué patrón general ve y por qué lo ve? ( Recuerde: todo lo demás sobre estos tratamientos, como el tamaño de la muestra y el número de experimentos, debe ser el mismo que el del experimento de control original. De lo contrario, todo lo demás no se mantiene igual).

 
B.
Bonificación: ¿Qué sucede si tiene una variable omitida en el modelo? Cambie su función de probabilidad logarítmica para excluir x4 de su procedimiento de estimación, pero continúe incluyendo x4 cuando simule los datos en el ciclo for . Dado que solo estima los coeficientes para x1 , x2 y x3 , ¿cómo se comparan sus estimaciones en este tratamiento con el tratamiento original en términos de sesgo y error absoluto medio?

 
 
Notas al pie
1 .
Véase también: Signorino (2002) y Signorino y Yilmaz (2003).

2 .
Se pueden cargar otras distribuciones a través de varios paquetes. Por ejemplo, otra distribución útil es la normal multivariante. Al cargar la biblioteca MASS , un usuario puede tomar muestras de la distribución normal multivariante con el comando mvrnorm . Incluso de manera más general, el paquete mvtnorm permite al usuario calcular probabilidades t multivariadas normales y multivariadas , cuantiles, desviaciones aleatorias y densidades.

3 .
Formalmente, la composición de esta lista se basa en una distribución uniforme estándar, que luego se convierte a la distribución que queramos utilizando una función de cuantiles.

4 .
Recuerde del Cap. 1 que la función módulo nos da el resto de la división. 

5 .
Ver Nocedal y Wright (1999) para una revisión de cómo funcionan estas técnicas.

6 .
Cuando se utiliza la estimación de máxima verosimilitud, como alternativa al uso de optim es utilizar el paquete maxLik . Esto puede ser útil específicamente para la máxima probabilidad, aunque optim tiene la ventaja de poder maximizar o minimizar también otros tipos de funciones, cuando sea apropiado.

7 .
Para ver la versión original completa del programa, consulte http://hdl.handle.net/1902.1/16781 . Tenga en cuenta que aquí usamos la familia de objetos S3 , pero el código original usa la familia S4 . El programa original también considera funciones alternativas de utilidad para votantes además de la función de proximidad cuadrática que se enumera aquí, como una función de proximidad absoluta y el modelo direccional de utilidad para votantes (Rabinowitz y Macdonald 1989).

8 .
Si prefiere utilizar el sistema de objetos S4 para este ejercicio, lo siguiente que tendríamos que hacer es definir la clase de objeto antes de definir la función. Si quisiéramos llamar a nuestra clase de objeto simulación , escribiríamos: setClass ("simulación", representación (resultadoA = "matriz", resultadoD = "matriz", bestResponseA = "numérico", bestResponseD = "numérico", equilibrioA = "carácter ", equilibriumD =" carácter ")) . El sistema de objetos S3 no requiere este paso, como veremos cuando creemos objetos de nuestra clase autodefinida game.simulation .

9 .
Por ejemplo, en la frase resultA = resultA , el término a la izquierda del signo igual indica que este término en la lista se llamará resultA , y el término a la derecha llama al objeto de este nombre de la función para llenar este espacio .

10 .
El código diferiría ligeramente para el sistema de objetos S4 . Si definimos nuestra clase de simulación como se describe en la nota al pie anterior, aquí reemplazaríamos la definición de resultado de la siguiente manera: resultado <-new ("simulación", resultadoA = resultadoA, resultadoD = resultadoD, bestResponseA = bestResponseA, bestResponseD = bestResponseD, equilibriumA = equilibrio A, equilibrio D = equilibrio D) . Reemplazar el comando list con el nuevo comando asigna la salida a nuestra clase de simulación y llena las distintas ranuras en un solo paso. Esto significa que podemos omitir el paso adicional de usar el comando de clase , que necesitábamos cuando usamos el sistema S3 .

11 .
Si, en cambio, hubiéramos guardado treatment.1 como un objeto de simulación S4 como se describe en las notas al pie anteriores, el comando para llamar a un atributo específico sería en su lugar: treatment.1@equilibriumA .

12 .
Los lectores interesados ​​en realizar una investigación como esta en su propio trabajo deben leer sobre el paquete de juegos , que fue desarrollado para estimar este tipo de modelo.

Material suplementario
318886_1_En_11_MOESM1_ESM.zip (392 kb)
Dataverse (2,154 KB)
Referencias
Alvarez RM, Levin I, Pomares J, Leiras M (2013) Votar de forma segura y sencilla: el impacto del voto electrónico en la percepción ciudadana. Polit Sci Res Methods 1 (1): 117-137
CrossRefGoogle Académico
Bates D, Maechler M, Bolker B, Walker S (2014) lme4 : modelos lineales de efectos mixtos que utilizan Eigen y S4. Versión del paquete R 1.1-7. http://www.CRAN.R-project.org/package=lme4
Becker RA, Cleveland WS, Shyu MJ (1996) El diseño visual y el control de la pantalla Trellis. J Comput Graph Stat 5 (2): 123-155
Google Académico
Beniger JR, Robyn DL (1978) Gráficos cuantitativos en estadística: una breve historia. Am Stat 32 (1): 1–11
zbMATHGoogle Académico
Berkman M, Plutzer E (2010) Evolución, creacionismo y la batalla por controlar las aulas de Estados Unidos. Cambridge University Press, Nueva York
CrossRefGoogle Académico
Black D (1948) Sobre el fundamento de la toma de decisiones en grupo. J Polit Econ 56 (1): 23–34
CrossRefGoogle Académico
Black D (1958) La teoría de los comités y las elecciones. Cambridge University Press, Londres
zbMATHGoogle Académico
Cuadro GEP, Tiao GC (1975) Análisis de intervenciones con aplicaciones a problemas económicos y ambientales. Asociación J Am Stat 70: 70–79
CrossRefMathSciNetzbMATHGoogle Académico
Box GEP, Jenkins GM, Reinsel GC (2008) Análisis de series de tiempo: previsión y control, 4ª ed. Wiley, Hoboken, Nueva Jersey
CrossRefzbMATHGoogle Académico
Box-Steffensmeier JM, Freeman JR, Hitt MP, Pevehouse JCW (2014) Análisis de series de tiempo para las ciencias sociales. Cambridge University Press, Nueva York
CrossRefGoogle Académico
Brambor T, Clark WR, Golder M (2006) Comprensión de los modelos de interacción: mejora de los análisis empíricos. Polit Anal 14 (1): 63–82
CrossRefGoogle Académico
Brandt PT, Freeman JR (2006) Avances en el modelado de series de tiempo bayesiano y el estudio de la política: pruebas teóricas, pronósticos y análisis de políticas. Polit Anal 14 (1): 1–36
CrossRefGoogle Académico
Brandt PT, Williams JT (2001) Un modelo lineal autorregresivo de Poisson: el modelo de Poisson AR (p). Polit Anal 9 (2): 164–184
CrossRefGoogle Académico
Brandt PT, Williams JT (2007) Múltiples modelos de series temporales. Sage, Thousand Oaks, CA
Google Académico
Bueno de Mesquita B, Lalman D (1992) Guerra y razón. Prensa de la Universidad de Yale, New Haven
Google Académico
Carlin BP, Louis TA (2009) Métodos bayesianos para el análisis de datos. Chapman & Hall / CRC, Boca Ratón, FL
zbMATHGoogle Académico
Libro de cocina de gráficos Chang W (2013) R. O'Reilly, Sebastopol, CA
Google Académico
Cleveland WS (1993) Visualización de datos. Prensa Hobart, Sebastopol, CA
Google Académico
Cowpertwait PSP, Metcalfe AV (2009) Serie temporal introductoria con R. Springer, Nueva York
zbMATHGoogle Académico
Cryer JD, Chan KS (2008) Análisis de series de tiempo con aplicaciones en R, 2ª ed. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Downs A (1957) Una teoría económica de la democracia. Harper and Row, Nueva York
Google Académico
Eliason SR (1993) Estimación de máxima verosimilitud: lógica y práctica. Sage, Thousand Oaks, CA
Google Académico
Enders W (2009) Series de tiempo econométricas aplicadas, 3ª ed. Wiley, Nueva York
Google Académico
Fitzmaurice GM, Laird NM, Ware JH (2004) Análisis longitudinal aplicado. Wiley-Interscience, Hoboken, Nueva Jersey
zbMATHGoogle Académico
Fogarty BJ, Monogan JE III (2014) Modelado de datos de recuento de series de tiempo: los desafíos únicos que enfrentan los estudios de comunicación política. Soc Sci Res 45: 73–88
CrossRefGoogle Académico
Gelman A, Hill J (2007) Análisis de datos utilizando modelos de regresión y multinivel / jerárquicos. Cambridge University Press, Nueva York
Google Académico
Gelman A, Carlin JB, Stern HS, Rubin DB (2004) Análisis de datos bayesianos, 2ª ed. Chapman & Hall / CRC, Boca Ratón, FL
zbMATHGoogle Académico
Gibney M, Cornett L, Wood R, Haschke P (2013) Escala de terror político, 1976–2012. Obtenido el 27 de diciembre de 2013 del sitio web de la escala de terror político: http://www.politicalterrorscale.org
Gill J (2001) Modelos lineales generalizados: un enfoque unificado. Sage, Thousand Oaks, CA
Google Académico
Gill J (2008) Métodos bayesianos: un enfoque de las ciencias sociales y del comportamiento, 2ª ed. Chapman & Hall / CRC, Boca Ratón, FL
zbMATHGoogle Académico
Granger CWJ (1969) Investigación de relaciones causales mediante modelos econométricos y métodos espectrales cruzados. Econometrica 37: 424–438
CrossRefGoogle Académico
Granger CWJ, Newbold P (1974) Regresiones espurias en econometría. J Econ 26: 1045–1066
zbMATHGoogle Académico
Gujarati DN, Porter DC (2009) Econometría básica, 5ª ed. McGraw-Hill / Irwin, Nueva York
Google Académico
Halley E (1686) Un relato histórico de los vientos alisios y monzones, observables en los mares entre y cerca de los trópicos, con un intento de asignar la causa física de dichos vientos. Philos Trans 16 (183): 153–168
CrossRefGoogle Académico
Hamilton JD (1994) Análisis de series de tiempo. Prensa de la Universidad de Princeton, Princeton, Nueva Jersey
zbMATHGoogle Académico
Hanmer MJ, Kalkan KO (2013) Detrás de la curva: aclarando el mejor enfoque para calcular las probabilidades pronosticadas y los efectos marginales a partir de modelos de variables dependientes limitadas. Am J Polit Sci 57 (1): 263–277
CrossRefGoogle Académico
Honaker J, King G, Blackwell M (2011) Amelia II: un programa para datos faltantes. J Stat Softw 45 (7): 1–47
CrossRefGoogle Académico
Hotelling H (1929) Estabilidad en competencia. Econ J 39 (153): 41–57
CrossRefGoogle Académico
Huber PJ (1967) El comportamiento de las estimaciones de máxima verosimilitud en condiciones no estándar. En: LeCam LM, Neyman J (eds) Actas del quinto simposio de Berkeley sobre estadística matemática y probabilidad, volumen 1: estadística University of California Press, Berkeley, CA
Google Académico
Iacus SM, King G, Porro G (2009) cem : software para emparejamiento exacto aproximado. J Stat Softw 30 (9): 1–27
CrossRefGoogle Académico
Iacus SM, King G, Porro G (2011) Métodos de emparejamiento multivariante que limitan el desequilibrio monótono. J Am Stat Assoc 106 (493): 345–361
CrossRefMathSciNetzbMATHGoogle Académico
Iacus SM, King G, Porro G (2012) Inferencia causal sin verificación de saldo: coincidencia exacta aproximada. Polit Anal 20 (1): 1–24
CrossRefGoogle Académico
Imai K, van Dyk DA (2004) Inferencia causal con regímenes de tratamiento general: generalización de la puntuación de propensión. J Am Stat Assoc 99 (467): 854–866
CrossRefMathSciNetzbMATHGoogle Académico
Jones DM, Bremer SA, Singer JD (1996) Disputas interestatales militarizadas, 1816-1992: fundamento, reglas de codificación y patrones empíricos. Confl Manag Peace Sci 15 (2): 163–213
CrossRefGoogle Académico
Kastellec JP, Leoni EL (2007) Uso de gráficos en lugar de tablas en ciencias políticas. Perspect Polit 5 (4): 755–771
CrossRefGoogle Académico
Keele L, Kelly NJ (2006) Modelos dinámicos para teorías dinámicas: los entresijos de las variables dependientes rezagadas. Polit Anal 14 (2): 186–205
CrossRefGoogle Académico
King G (1989) Metodología política unificadora. Cambridge University Press, Nueva York
Google Académico
King G, Honaker J, Joseph A, Scheve K (2001) Analizando datos incompletos de ciencia política: un algoritmo alternativo para la imputación múltiple. Am Polit Sci Rev 95 (1): 49–69
Google Académico
Koyck LM (1954) Rezagos distribuidos y análisis de inversiones. Holanda Septentrional, Amsterdam
Google Académico
Laird NM, Fitzmaurice GM (2013) Modelado de datos longitudinal. En: Scott MA, Simonoff JS, Marx BD (eds) El manual Sage de modelado multinivel. Sage, Thousand Oaks, CA
Google Académico
LaLonde RJ (1986) Evaluación de las evaluaciones econométricas de programas de formación con datos experimentales. Am Econ Rev 76 (4): 604–620
Google Académico
Little RJA, Rubin DB (1987) Análisis estadístico con datos faltantes, 2ª ed. Wiley, Nueva York
zbMATHGoogle Académico
Long JS (1997) Modelos de regresión para variables dependientes categóricas y limitadas. Sage, Thousand Oaks, CA
zbMATHGoogle Académico
Lowery D, Gray V, Monogan JE III (2008) La construcción de comunidades de interés: distinguiendo modelos ascendentes y descendentes. J Polit 70 (4): 1160–1176
CrossRefGoogle Académico
Lütkepohl H (2005) Nueva introducción al análisis de múltiples series de tiempo. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Martin AD, Quinn KM, Park JH (2011) MCMCpack : Markov chain Monte Carlo en R. J Stat Softw 42 (9): 1–21
CrossRefGoogle Académico
Mátyás L, Sevestre P (eds) (2008) La econometría de los datos de panel: fundamentos y desarrollos recientes en la teoría y la práctica, 3ª ed. Springer, Nueva York
zbMATHGoogle Académico
McCarty NM, Poole KT, Rosenthal H (1997) Redistribución de la renta y realineamiento de la política estadounidense. Estudios del instituto empresarial estadounidense sobre la comprensión de la desigualdad económica. AEI Press, Washington, DC
Google Académico
Monogan JE III (2011) Análisis de datos de panel. En: Badie B, Berg-Schlosser D, Morlino L (eds) Enciclopedia internacional de ciencia política. Sage, Thousand Oaks, CA
Google Académico
Monogan JE III (2013a) Un caso para registrar estudios de resultados políticos: una aplicación en las elecciones a la Cámara de 2010. Polit Anal 21 (1): 21–37
CrossRefGoogle Académico
Monogan JE III (2013b) Colocación estratégica del partido con un electorado dinámico. J Theor Polit 25 (2): 284–298
CrossRefGoogle Académico
Nocedal J, Wright SJ (1999) Optimización numérica. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Owsiak AP (2013) Democratización y acuerdos fronterizos internacionales. J Polit 75 (3): 717–729
CrossRefGoogle Académico
Peake JS, Eshbaugh-Soha M (2008) El impacto en el establecimiento de la agenda de los principales discursos televisivos presidenciales. Polit Commun 25: 113-137
CrossRefGoogle Académico
Petris G, Petrone S, Campagnoli P (2009) Modelos lineales dinámicos con R. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Pfaff B (2008) Análisis de series temporales integradas y cointegradas con R, 2ª ed. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Playfair W (1786/2005) En: Wainer H, Spence I (eds) Atlas comercial y político y breviario estadístico. Cambridge University Press, Nueva York
Google Académico
Poe SC, Tate CN (1994) Represión de los derechos humanos a la integridad personal en la década de 1980: un análisis global. Am Polit Sci Rev 88 (4): 853–872
CrossRefGoogle Académico
Poe SC, Tate CN, Keith LC (1999) Revisión de la represión del derecho humano a la integridad personal: un estudio internacional transnacional que abarca los años 1976-1993. Int Stud Q 43 (2): 291–313
CrossRefGoogle Académico
Poole KT, Rosenthal H (1997) Congreso: una historia político-económica de la votación nominal. Oxford University Press, Nueva York
Google Académico
Poole KT, Lewis J, Lo J, Carroll R (2011) Escala de votos nominales con wnominate en R. J Stat Softw 42 (14): 1–21
CrossRefGoogle Académico
Rabinowitz G, Macdonald SE (1989) Una teoría direccional de la votación por cuestiones. Am Polit Sci Rev 83: 93-121
CrossRefGoogle Académico
Robert CP (2001) La elección bayesiana: de los fundamentos de la teoría de la decisión a la implementación computacional, 2ª ed. Springer, Nueva York
zbMATHGoogle Académico
Rubin DB (1987) Imputación múltiple por falta de respuesta en encuestas. Wiley, Nueva York
CrossRefzbMATHGoogle Académico
Rubin DB (2006) Muestreo emparejado para efectos causales. Cambridge University Press, Nueva York
CrossRefzbMATHGoogle Académico
Scott MA, Simonoff JS, Marx BD (eds) (2013) El manual Sage de modelado multinivel. Sage, Thousand Oaks, CA
Google Académico
Sekhon JS, Grieve RD (2012) Un método de emparejamiento para mejorar el equilibrio de covariables en los análisis de rentabilidad. Health Econ 21 (6): 695–714
CrossRefGoogle Académico
Shumway RH, Stoffer DS (2006) Análisis de series de tiempo y sus aplicaciones con ejemplos de R, 2ª ed. Springer, Nueva York
zbMATHGoogle Académico
Signorino CS (1999) Interacción estratégica y análisis estadístico del conflicto internacional. Am Polit Sci Rev 93: 279–297
CrossRefGoogle Académico
Signorino CS (2002) Estrategia y selección en relaciones internacionales. Int Interact 28: 93-115
CrossRefGoogle Académico
Signorino CS, Yilmaz K (2003) Especificación errónea estratégica en modelos de regresión. Am J Polit Sci 47: 551–566
CrossRefGoogle Académico
Sims CA, Zha T (1999) Bandas de error para respuestas de impulso. Econometrica 67 (5): 1113–1155
CrossRefMathSciNetzbMATHGoogle Académico
Singh SP (2014a) Funciones de pérdida de utilidad lineales y cuadráticas en la investigación del comportamiento electoral. J Theor Polit 26 (1): 35–58
CrossRefGoogle Académico
Singh SP (2014b) No todos los ganadores de las elecciones son iguales: satisfacción con la democracia y la naturaleza del voto. Eur J Polit Res 53 (2): 308–327
CrossRefGoogle Académico
Singh SP (2015) Voto obligatorio y cálculo de decisiones de participación. Polit Stud 63 (3): 548–568
CrossRefGoogle Académico
Tufte ER (2001) La presentación visual de información cuantitativa, 2ª ed. Prensa gráfica, Cheshire, CT
Google Académico
Tukey JW (1977) Análisis de datos exploratorios. Addison-Wesley, Reading, PA
zbMATHGoogle Académico
Wakiyama T, Zusman E, Monogan JE III (2014) ¿Puede sostenerse una transición energética baja en carbono en el Japón posterior a Fukushima? Evaluación de los diferentes impactos de los choques exógenos. Política energética 73: 654–666
CrossRefGoogle Académico
Wei WWS (2006) Análisis de series de tiempo: métodos univariados y multivariados, 2ª ed. Pearson, Nueva York
zbMATHGoogle Académico
White H (1980) Un estimador de matriz de covarianza consistente con heterocedasticidad y una prueba directa de heterocedasticidad. Econometrica 48 (4): 817–838
CrossRefMathSciNetzbMATHGoogle Académico
Yau N (2011) Visualice esto: la guía FlowingData para diseño, visualización y estadísticas. Wiley, Indianápolis
Google Académico
