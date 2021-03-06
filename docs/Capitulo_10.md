# Álgebra lineal con aplicaciones de programación {#Álgebralinealconaplicacionesdeprogramación}

<hr style="background-color:#03193b;height:2px">

Palabras clave:
- Mínimos cuadrados ordinarios
- Multiplicación de matrices 
- Álgebra de matrices 
- Marco de datos 
- Compartir votos

El  lenguaje R tiene varios comandos de álgebra matricial incorporados. Esto resulta útil para los analistas que desean escribir sus propios estimadores o tienen otros problemas en álgebra lineal que desean calcular usando software. En algunos casos, es más fácil aplicar una fórmula para predicciones, errores estándar o alguna otra cantidad directamente en lugar de buscar un programa enlatado para calcular la cantidad, si existe. El álgebra de matrices hace que sea sencillo calcular estas cantidades usted mismo. En este capítulo se presenta la sintaxis y comandos disponibles para la realización de álgebra matricial en R .

El capítulo continúa describiendo primero cómo un usuario puede ingresar datos originales a mano, como un medio para crear vectores, matrices y marcos de datos. Luego presenta varios de los comandos asociados con el álgebra lineal. Finalmente, trabajamos a través de un ejemplo aplicado en el que estimamos un modelo lineal con mínimos cuadrados ordinarios programando nuestro propio estimador.

Como datos de trabajo a lo largo del capítulo, consideramos un ejemplo simple de las elecciones al Congreso de Estados Unidos de 2010. Modelamos la participación del candidato republicano en el voto bipartidista en las elecciones para la Cámara de Representantes ( y ) en 2010. Las variables de entrada son una constante ( x 1 ), la participación de Barack Obama en el voto presidencial bipartidista en 2008 ( x 2 ), y la situación financiera del candidato republicano en relación con el demócrata en cientos de miles de dólares ( x 3 ). Para simplificar, modelamos las nueve carreras de la Cámara en el estado de Tennessee. Los datos se presentan en la Tabla  10.1 .
Cuadro 10.1
Datos de las elecciones al Congreso de Tennessee en 2010

Distrito

Republicano ( y )

Constante ( x 1 )

Obama ( x 2 )

Financiamiento ( x 3 )

 
1

0,808

1

0,290

4.984

 
2

0,817

1

0.340

5.073

 
3

0.568

1

0.370

12.620

 
4

0.571

1

0.340

−6,443

 
5

0.421

1

0.560

−5,758

 
6

0,673

1

0.370

15.603

 
7

0,724

1

0.340

14.148

 
8

0.590

1

0,430

0.502

 
9

0,251

1

0,770

−9.048

 
Nota: Datos de Monogan (2013a)

10.1 Creación de vectores y matrices
Como primera tarea, al asignar valores a un vector o matriz, debemos usar el comando de asignación tradicional ( <- ). El comando c  c ombines varios elementos componentes en un vector objeto. 1 Entonces, para crear un vector a con los valores específicos de 3, 4 y 5:

a <- c (3,4,5)

Dentro de c , todo lo que tenemos que hacer es separar cada elemento del vector con una coma. Como un ejemplo más interesante, supongamos que quisiéramos ingresar tres de las variables de la tabla  10.1 : la participación republicana en el voto bipartidista en 2010, la participación de Obama en el voto bipartidista en 2008 y la ventaja financiera republicana, todos como vectores. Teclearíamos:

Y <-c (.808, .817, .568, .571, .421, .673, .724, .590, .251)

X2 <-c (.29, .34, .37, .34, .56, .37, .34, .43, .77)

X3 <-c (4.984,5.073,12.620, -6.443, -5.758,15.603,14.148,0.502,

     -9.048)

Siempre que ingrese datos en forma vectorial, generalmente debemos asegurarnos de haber ingresado el número correcto de observaciones. El comando de longitud devuelve cuántas observaciones hay en un vector. Para verificar las tres entradas de nuestros vectores, escribimos:

largo); longitud (X2); longitud (X3)

Los tres de nuestros vectores deberían tener una longitud de 9 si se ingresaron correctamente. Observe aquí el uso del punto y coma ( ; ). Si un usuario prefiere colocar varios comandos en una sola línea de texto, el usuario puede separar cada comando con un punto y coma en lugar de poner cada comando en una línea separada. Esto permite que los comandos simples se almacenen de forma más compacta.

Si queremos crear un vector que siga un patrón de repetición , podemos usar el comando rep . Por ejemplo, en nuestro modelo de rendimientos electorales de Tennessee, nuestro término constante es simplemente un vector de 1 de 9 × 1:

X1 <- rep (1, 9)

El primer término dentro de rep es el término que se debe repetir y el segundo término es el número de veces que debe repetirse.

Los vectores secuenciales también son fáciles de crear. Dos puntos ( : ) indicaciones R  a enteros lista secuencial desde el inicio hasta el punto de parada. Por ejemplo, para crear un índice para el distrito del Congreso de cada observación, necesitaríamos un vector que contenga valores de 1 a 9:

índice <- c (1: 9)

Como acotación al margen, una orden más general es la siguientes comandos, lo que nos permite definir los intervalos de un ss influencia, así como los valores inicial y final. Por ejemplo, si por alguna razón quisiéramos crear una secuencia de - 2 a 1 en incrementos de 0.25, escribiríamos:

e <- seq (-2, 1, por = 0,25)

Cualquier vector que creamos se puede imprimir en la pantalla simplemente escribiendo el nombre del vector. Por ejemplo, si simplemente escribimos Y en el símbolo del sistema, nuestro vector de participación de votos republicanos se imprime en la salida:

[1] 0,808 0,817 0,568 0,571 0,421 0,673 0,724

[8] 0,590 0,251

En este caso, se imprimen los nueve valores del vector Y. El número impreso entre llaves al comienzo de cada fila ofrece el índice del primer elemento de esa fila. Por ejemplo, la octava observación de Y es el valor 0.590. Para vectores más largos, esto ayuda al usuario a realizar un seguimiento de los índices de los elementos dentro del vector.

10.1.1 Crear matrices
Pasando a las matrices, podemos crear un objeto de clase matrix de varias formas posibles. Primero, podríamos usar el comando matrix : en el caso más simple posible, supongamos que quisiéramos crear una matriz con todos los valores iguales. Para crear una matriz b de 4 × 4 con cada valor igual a 3:

b <- matriz (3, ncol = 4, nrow = 4, byrow = FALSE)

La sintaxis del comando de matriz primero llama a los elementos que definirán la matriz: en este caso, enumeramos un solo escalar, por lo que este número se repitió para todas las celdas de la matriz. Alternativamente, podríamos listar un vector en su lugar que incluya suficientes entradas para llenar toda la matriz. Luego, necesitamos especificar el número de columnas ( ncol ) y filas ( nrow ). Por último, el argumento byrow se establece en FALSE de forma predeterminada. Con la configuración FALSE , R  llena la matriz columna por columna. En su lugar, un ajuste VERDADERO llena la matriz fila por fila. Como regla, siempre que cree una matriz, escriba el nombre de la matriz ( ben este caso) en la consola de comandos para ver si la forma en que R  ingresa los datos coincide con lo que pretendía.

Una segunda opción simple es crear una matriz a partir de vectores que ya se han ingresado. Podemos unir los vectores juntos como c OLUMNA vectores utilizando el cbind mando y como r ow vectores utilizando el rbind comando. Por ejemplo, en nuestro modelo de declaraciones de elecciones de Tennessee, necesitaremos crear una matriz de todas las variables de entrada en la que las variables definen las columnas y las observaciones definen las filas. Como hemos definido nuestros tres vectores variables (y cada vector está ordenado por observación), podemos simplemente crear dicha matriz usando cbind :

X <-cbind (1, X2, X3)

X

El comando cbind trata nuestros vectores como columnas en una matriz. Esto es lo que queremos, ya que una matriz de predictores define filas con observaciones y columnas con variables. El 1 en el comando cbind asegura que todos los elementos de la primera columna sean iguales a la constante 1. (Por supuesto, de la forma en que diseñamos X1 , también podríamos haber incluido ese vector). Al escribir X en la consola, obtenemos el imprimir:

          X2 X3

 [1,] 1 0,29 4,984

 [2,] 1 0,34 5,073

 [3,] 1 0,37 12,620

 [4,] 1 0,34 -6,443

 [5,] 1 0,56 -5,758

 [6,] 1 0.37 15.603

 [7,] 1 0,34 14,148

 [8,] 1 0,43 0,502

 [9,] 1 0,77 -9,048

Estos resultados coinciden con los datos que tenemos en la tabla  10.1 , por lo que nuestra matriz de covariables debería estar lista cuando estemos listos para estimar nuestro modelo.

Solo para ilustrar el comando rbind , R  fácilmente combinaría los vectores como filas de la siguiente manera:

T <-rbind (1, X2, X3)

T

No formateamos datos como este, pero para ver cómo se ven los resultados, al escribir T en la consola se obtiene la impresión:

    [, 1] [, 2] [, 3] [, 4] [, 5] [, 6] [, 7]

   1.000 1.000 1.00 1.000 1.000 1.000 1.000

X2 0,290 0,340 0,37 0,340 0,560 0,370 0,340

X3 4.984 5.073 12.62 -6.443 -5.758 15.603 14.148

    [, 8] [, 9]

   1.000 1.000

X2 0,430 0,770

X3 0,502 -9,048

Por tanto, vemos que cada vector variable forma una fila y cada observación forma una columna. En la impresión, cuando R  carece de espacio para imprimir una fila completa en una sola línea, envuelve todas las filas a la vez, presentando las columnas 8 y 9 más adelante.

En tercer lugar, podríamos crear una matriz utilizando subíndices. (Los detalles adicionales sobre los subíndices de vectores y matrices se presentan en la Sección  10.1.3 .) A veces, al crear una matriz, el usuario conocerá todos los valores por adelantado, pero en otras ocasiones el usuario debe crear una matriz y luego completar la valores más tarde. En el último caso, es una buena idea crear “espacios en blanco” para completar designando cada celda como faltante (o NA ). La buena característica de esto es que un usuario puede identificar fácilmente una celda que nunca se completó. Por el contrario, si se crea una matriz con algún valor numérico predeterminado, digamos 0, más adelante es imposible distinguir una celda que tiene un 0 predeterminado de una con un valor real de 0. Entonces, si quisiéramos crear un 3 × 3 matriz denominada en blanco para rellenar más tarde, escribiríamos:

blanco <- matriz (NA, ncol = 3, nrow = 3)

Si luego quisiéramos asignar el valor de 8 a la primera fila, tercer elemento de columna, escribiríamos:

en blanco [1,3] <- 8

Si luego quisiéramos insertar el valor π (= 3. 141592 … ) en la segunda fila, la entrada de la primera columna, escribiríamos:

en blanco [2,1] <- pi

Si quisiéramos usar nuestro vector previamente definido a  = (3, 4, 5) ′ para definir la segunda columna, escribiríamos:

en blanco [, 2] <- a

Luego podríamos verificar nuestro progreso simplemente escribiendo en blanco en el símbolo del sistema, que imprimiría:

         [, 1] [, 2] [, 3]

[1,] NA 3 8

[2,] 3,141593 4 NA

[3,] NA 5 NA

A la izquierda de la matriz, se definen los términos de las filas. En la parte superior de la matriz, se definen los términos de la columna. Observe que cuatro elementos todavía se codifican NA porque nunca se ofreció un valor de reemplazo.

En cuarto lugar, a diferencia de rellenar matrices después de la creación, también podemos conocer los valores que queremos en el momento de crear la matriz. Como ejemplo simple, para crear una matriz W de 2 × 2 en la que enumeramos el valor de cada celda columna por columna:

W <- matriz (c (1,2,3,4), ncol = 2, nrow = 2)

Observe que nuestro primer argumento ahora es un vector porque queremos proporcionar elementos únicos para cada una de las celdas de la matriz. Con cuatro elementos en el vector, tenemos el número correcto de entradas para una matriz de 2 × 2. Además, en este caso, ignoramos el argumento byrow porque el valor predeterminado es completar por columnas. Por el contrario, si quisiéramos enumerar los elementos de celda fila por fila en la matriz Z , simplemente estableceríamos byrow en VERDADERO :

Z <- matriz (c (1,2,3,4), ncol = 2, nrow = 2, byrow = TRUE)

Escriba W y Z en la consola y observe cómo se ha reorganizado el mismo vector de entradas de celda pasando de una celda a otra.

Alternativamente, supongamos que queríamos crear un 10 × 10 matriz N dónde estaba cada entrada en la célula un r Andom extraer de una norma de distribución al con media 10 y desviación estándar 2, o  norte( 10 , 4 ) :

N <- matriz (rnorm (100, media = 10, sd = 2), nrow = 10, ncol = 10)

Debido a que rnorm devuelve un objeto de la clase de vector, nuevamente estamos listando un vector para crear nuestras entradas de celda. El comando rnorm se extrae de la distribución normal con nuestras especificaciones 100 veces, lo que proporciona el número de entradas de celda que necesitamos para una matriz de 10 × 10.

En quinto lugar, en muchas ocasiones, que se desee crear una matriz diagonal que contiene sólo elementos en su principal diag onal (desde la parte superior izquierda a la parte inferior derecha), con ceros en todas las demás células. El comando diag facilita la creación de dicha matriz:

D <- diag (c (1: 4))

Al escribir D en la consola, ahora vemos cómo aparece esta matriz diagonal:

     [, 1] [, 2] [, 3] [, 4]

[1,] 1 0 0 0

[2,] 0 2 0 0

[3,] 0 0 3 0

[4,] 0 0 0 4

Además, si uno inserta una matriz cuadrada en el comando diag , devolverá un vector de los elementos diagonales de la matriz. Por ejemplo, en una matriz de varianza-covarianza, los elementos diagonales son varianzas y se pueden extraer rápidamente de esta manera.

10.1.2 Conversión de matrices y tramas de datos
Un medio final de crear un objeto de matriz es con el comando as.matrix . Este comando puede tomar un objeto de la clase de marco de datos y convertirlo en un objeto de la clase de matriz . Para un marco de datos llamado mydata , por ejemplo, el comando mydata.2 <- as.matrix (mydata) coaccionaría los datos en forma de matriz, haciendo que todas las operaciones de matriz subsiguientes fueran aplicables a los datos. Además, escribir mydata.3 <- as.matrix (mydata [, 4: 8]) solo tomaría las columnas de la cuatro a la ocho del marco de datos y las convertiría en una matriz, lo que permitiría al usuario dividir los datos en subconjuntos mientras crea un objeto de matriz .

De manera similar, suponga que queremos tomar un objeto de la clase de matriz y crear un objeto de la clase de marco de datos , tal vez nuestros datos electorales de Tennessee de la tabla  10.1 . En este caso, el comando as.data.frame funcionará. Si simplemente quisiéramos crear un marco de datos a partir de nuestra matriz de covariables X , podríamos escribir:

X.df <- como.data.frame (X)

Si quisiéramos crear un marco de datos utilizando todas las variables de la Tabla  10.1 , incluida la variable dependiente y el índice, podríamos escribir:

tennessee <- como.data.frame (cbind (index, Y, X1, X2, X3))

En este caso, el comando cbind incrustado en as.data.frame define una matriz, que se convierte inmediatamente en un marco de datos. En general, entonces, si un usuario desea ingresar sus propios datos en R , la forma más sencilla será definir vectores variables, unirlos como columnas y convertir la matriz en un marco de datos.

10.1.3 Subíndice
Como se mencionó en la sección de creación de matrices, llamar a elementos de vectores y matrices por sus subíndices puede ser útil para extraer la información necesaria o para realizar asignaciones. Para llamar a un valor específico, podemos indexar un vector y por el n- ésimo elemento, usando Y [n] . Entonces, el tercer elemento del vector y , o y 3 , es llamado por:

Y [3]

Para indexar una matriz X n × k para el valor X ij , donde i representa la fila y j representa la columna, use la sintaxis X [i, j] . Si queremos seleccionar todos los valores de la j- ésima columna de X , podemos usar X [, j] . Por ejemplo, para devolver la segunda columna de la matriz X , escriba:

X [, 2]

Alternativamente, si una columna tiene un nombre, entonces el nombre (entre comillas) también se puede usar para llamar a la columna. Por ejemplo:

X [, "X2"]

De manera similar, si deseamos seleccionar todos los elementos de la i- ésima fila de X , podemos usar X [i,] . Para la primera fila de la matriz X , escriba:

X [1,]

Alternativamente, también podríamos usar un nombre de fila, aunque la matriz X no tiene nombres asignados a las filas.

Sin embargo, si lo deseamos, podemos crear nombres de filas para matrices. Por ejemplo:

nombres de fila (X) <- c ("Dist. 1", "Dist. 2", "Dist. 3", "Dist. 4",

     "Dist. 5", "Dist. 6", "Dist. 7", "Dist. 8", "Dist. 9")

Del mismo modo, el comando COLNAMES permite al usuario definir col UMN nombres . Para simplemente escribir nombres de fila (X) o colnames (X) sin realizar una asignación, R  imprimirá los nombres de fila o columna guardados para una matriz.

10.2 Comandos vectoriales y matriciales
Ahora que tenemos la sensación de crear vectores y matrices, recurrimos a comandos que extraen información de estos objetos o nos permiten realizar álgebra lineal con ellos. Como se mencionó anteriormente, después de ingresar un vector o matriz en R , además de imprimir el objeto en la pantalla para una inspección visual, también es una buena idea verificar y asegurarse de que las dimensiones del objeto sean correctas. Por ejemplo, para obtener la longitud de un vector a :

longitud (a)

Del mismo modo, para obtener las dim ensiones de una matriz, tipo:

tenue (X)

El comando dim primero imprime el número de filas, luego el número de columnas. La verificación de estas dimensiones ofrece una garantía adicional de que los datos de una matriz se han introducido correctamente.

En el caso de los vectores, los elementos se pueden tratar como datos y se pueden extraer cantidades resumidas. Por ejemplo, para sumar la suma de los elementos de un vector:

suma (X2)

De manera similar, para tomar la media de los elementos de un vector (en este ejemplo, la media del voto por distrito de Obama en 2008):

media (X2)

Y tomar la var iance de un vector:

var (X2)

Otra opción que tenemos para matrices y vectores es que podemos tomar muestras de un objeto dado con el comando sample . Supongamos que queremos una muestra de diez números de N , nuestra matriz de 10 × 10 de extracciones aleatorias de una distribución normal:

set.seed (271828183)

N <- matriz (rnorm (100, media = 10, sd = 2), nrow = 10, ncol = 10)

s <- muestra (N, 10)

El primer comando, set.seed , hace que esta simulación sea más replicable. (Ver. Cap  11 para obtener más detalles acerca de este comando.) Esto nos da un vector llamado s de elementos aleatorios de diez N . También tenemos la opción de aplicar el comando de ejemplo a los vectores. 2 

El comando de aplicación es a menudo la forma más eficiente de hacer cálculos vectorizados. Por ejemplo, para calcular las medias de todas las columnas en nuestra matriz de datos de Tennessee X :

aplicar (X, 2, media)

En este caso, el primer argumento enumera la matriz a analizar. El segundo argumento, 2 , le dice a R  que aplique una función matemática a lo largo de las columnas de la matriz. El tercer argumento, mean , es la función que queremos aplicar a cada columna. Si quisiéramos la media de las filas en lugar de las columnas, podríamos usar un 1 como segundo argumento en lugar de un 2. Cualquier función definida en R  se puede usar con el comando apply .

10.2.1 Álgebra de matrices
También están disponibles los comandos que son más específicos del álgebra matricial. Siempre que el usuario desee guardar la salida de una operación vectorial o matricial, se debe utilizar el comando de asignación. Por ejemplo, si por alguna razón necesitáramos la diferencia entre cada participación republicana del voto bipartidista y la participación de Obama en el voto bipartidista, podríamos asignar el vector m a la diferencia de los dos vectores escribiendo:

m <- Y - X2

Con las operaciones aritméticas, R  es bastante flexible, pero una palabra sobre cómo funcionan los comandos puede evitar confusiones futuras. Para suma ( + ) y resta ( - ): si los argumentos son dos vectores de la misma longitud (como es el caso en el cálculo del vector m ), entonces R  calcula la suma o resta de vectores regulares donde cada elemento se suma o se resta de su elemento correspondiente en el otro vector. Si los argumentos son dos matrices del mismo tamaño, se aplica la suma o resta de matrices cuando cada entrada se combina con su entrada correspondiente en la otra matriz. Si un argumento es un escalar, entonces el escalar se agregará a cada elemento del vector o matriz.

Nota : Si se agregan dos vectores de longitud desigual o dos matrices de diferente tamaño, aparece un mensaje de error, ya que estos argumentos no son conformes. Para ilustrar esto, si intentáramos sumar nuestra muestra de diez números de antes, s , a nuestro vector de la participación de Obama en 2008 en los votos, x 2 , de la siguiente manera:

s + X2

En este caso recibiríamos una salida que deberíamos ignorar, seguida de un mensaje de error en rojo:

 [1] 11.743450 8.307068 11.438161 14.251645 10.828459

 [6] 10.336895 9.900118 10.092051 12.556688 9.775185

Mensaje de advertencia:

En s + X2: la longitud del objeto más larga no es múltiplo de más corta

longitud del objeto

Este mensaje de advertencia nos dice que la salida es una tontería. Dado que el vector s tiene una longitud de 10, mientras que el vector X2 tiene una longitud de 9, lo que R  ha hecho aquí es agregar el décimo elemento de sa el primer elemento de X2 y usarlo como el décimo elemento de la salida. El mensaje de error que sigue sirve como recordatorio de que una entrada basura que rompe las reglas del álgebra lineal produce una salida basura que nadie debería usar.

Por el contrario, * , / , ^ (exponentes), exp (función exponencial) y log aplican la operación relevante a cada entrada escalar en el vector o matriz. Para nuestra matriz de predictores de Tennessee, por ejemplo, pruebe el comando:

X.sq <- X ^ 2

Esto volvería una matriz que los cuadrados de cada celda por separado en la matriz X . De manera similar, la operación de multiplicación simple ( * ) realiza la multiplicación elemento por elemento. Por lo tanto, si dos vectores son de la misma longitud o dos matrices del mismo tamaño, la salida será un vector o matriz del mismo tamaño donde cada elemento es el producto de los elementos correspondientes. A modo de ilustración, multipliquemos la participación de Obama en el voto bipartidista por la ventaja financiera republicana de varias formas. (Las cantidades pueden ser tontas, pero esto ofrece un ejemplo de cómo funciona el código). Intente, por ejemplo:

x2.x3 <- X2 * X3

El vector de salida es:

[1] 1.44536 1.72482 4.66940 -2.19062 -3.22448

[6] 5.77311 4.81032 0.21586 -6.96696

Esto corresponde a la multiplicación escalar elemento por elemento.

Más a menudo, el usuario realmente querrá realizar una multiplicación de matrices adecuada. En R , la multiplicación de matrices se realiza mediante la operación % *% . Entonces, si hubiéramos multiplicado nuestros dos vectores de participación de votos de Obama y ventaja financiera republicana de esta manera:

x2.x3.inner <- X2% *% X3

R  ahora devolvería el producto interno o el producto escalar (  X2⋅X3 ). Esto equivale a 6.25681 en nuestro ejemplo. Otra cantidad útil para la multiplicación de vectores es el producto externo o producto tensorial (  X2⊗X3 ). Podemos obtener esto transponiendo el segundo vector en nuestro código:

x2.x3.outer <- X2% *% t (X3)

La salida de este comando es una matriz de 9 × 9. Para obtener esta cantidad se utilizó el comando t ranspose ( t ). 3 En el álgebra de matrices, es posible que a su vez vectores fila a vectores de columna o viceversa, y el t operación ranspose logra esto. De manera similar, cuando se aplica a una matriz, cada fila se convierte en una columna y cada columna se convierte en una fila.

Como un ejemplo más de multiplicación de matrices, la variable de entrada matriz X tiene un tamaño de 9 × 3, y la matriz T que permite que las variables definan filas es una matriz de 3 × 9. En este caso, se podría crear la matriz P  =  TX porque el número de columnas de T es el mismo que el número de filas de X . Por lo tanto, podemos decir que las matrices son compatibles para la multiplicación y darán como resultado una matriz de tamaño 3 × 3. Podemos calcular esto como:

P <- T% *% X

Tenga en cuenta que si nuestras matrices no son aptas para la multiplicación, R  devolverá un mensaje de error. 4

Más allá de la multiplicación de matrices y la operación de transposición, otra cantidad importante que es exclusiva del álgebra de matrices es el det erminante de una matriz cuadrada (o una matriz que tiene el mismo número de filas que de columnas). Nuestra matriz P es cuadrada porque tiene tres filas y tres columnas. Una razón para calcular el determinante es que una matriz cuadrada tiene una inversa solo si el determinante es distinto de cero. 5 Para calcular el determinante de P , escribimos:

det (P)

Esto arroja un valor de 691,3339. Por tanto, sabemos que P tiene una inversa.

Para una matriz cuadrada que se puede invertir, la inversa es una matriz que se puede multiplicar por la original para producir la matriz identidad. Con la solución de comandos, R  o bien resolver por la inversa de la matriz o transmitir que la matriz es invertible. Para probar este concepto, escriba:

invP <- resolver (P)

invP% *% P

En la primera línea, creamos P -1 , que es la inversa de P . En la segunda línea, multiplicamos P −1 P y la impresión es:

                           X2 X3

   1.000000e + 00 -6.106227e-16 -6.394885e-14

X2 1.421085e-14 1.000000e + 00 1.278977e-13

X3 2.775558e-17 -2.255141e-17 1.000000e + 00

Esta es la forma básica de la matriz de identidad, con valores de 1 a lo largo de la diagonal principal (desde la parte superior izquierda a la inferior derecha) y valores de 0 fuera de la diagonal. Mientras que los elementos fuera de la diagonal no se enumeran exactamente como cero, esto puede atribuirse a error de redondeo en R parte ‘s. La notación científica para la segunda fila, el primer elemento de la columna, por ejemplo, significa que los primeros 13 dígitos después del lugar decimal son cero, seguidos de un 1 en el decimocuarto dígito.

10.3 Ejemplo aplicado: Programación de regresión OLS
Para ilustrar las diversas operaciones de álgebra matricial que R  tiene disponibles, en esta sección trabajaremos un ejemplo aplicado calculando el estimador de mínimos cuadrados ordinarios (MCO) con nuestro propio programa usando datos reales.

10.3.1 Cálculo manual de OLS
Primero, para motivar el trasfondo del problema, considere la formulación del modelo y cómo lo estimaríamos a mano. Nuestro modelo de regresión lineal poblacional para la participación de votos en Tennessee es:  y = X β+ u . En este modelo, y es un vector de la proporción de votos republicanos en cada distrito, X es una matriz de predictores (incluida una constante, la proporción de votos de Obama en 2008 y la ventaja financiera del republicano en relación con la demócrata),  β consta del coeficiente parcial para cada predictor, y u es un vector de perturbaciones. Nosotros estimamos  β^= (X′X)- 1X′y , produciendo la función de regresión muestral   y^= Xβ^ .

Para iniciar el cálculo de la mano, tenemos que definir X . Tenga en cuenta que debemos incluir un vector de 1 para estimar un término de intersección. En forma escalar, nuestro modelo de población es:  yI=β1X1 yo+β2X2 yo+β3X3 yo+tuI , donde x 1 i  = 1 para todo i . Esto nos da la matriz de predictores:
X =⎡⎣⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢1111111110,290,340,370,340,560,370,340,430,774.9845.07312.620- 6.443- 5.75815.60314.1480.502- 9.048⎤⎦⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥
A continuación, premultiplica X por su transposición:
X′X =⎡⎣⎢1.0000,2904.9841.0000.3405.0731.0000.37012.6201.0000.340- 6.4431.0000.560- 5.7581.0000.37015.6031.0000.34014.1481.0000,4300.5021.0000,770- 9.048⎤⎦⎥⎡⎣⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢1111111110,290,340,370,340,560,370,340,430,774.9845.07312.620- 6.443- 5.75815.60314.1480.502- 9.048⎤⎦⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥
que funciona para:
X′X =⎡⎣⎢9.000003.8100031.681003.810001.796106.2568131.681006.25681810.24462⎤⎦⎥
También necesitamos X′y :
X′y =⎡⎣⎢1.0000,2904.9841.0000.3405.0731.0000.37012.6201.0000.340- 6.4431.0000.560- 5.7581.0000.37015.6031.0000.34014.1481.0000,4300.5021.0000,770- 9.048⎤⎦⎥⎡⎣⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢⎢0,8080,8170.5680.5710.4210,6730,7240.5900,251⎤⎦⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥⎥
que funciona para:
X′y =⎡⎣⎢5.42302.094328.0059⎤⎦⎥
Invertir a mano
La última cantidad que necesitamos es la inversa de X′X . Haciendo esto a mano, podemos resolver por eliminación de Gauss-Jordan:
[X′X | Yo ]=⎡⎣⎢⎢9.000003.8100031.681003.810001.796106.2568131.681006.25681810.244621,000000,000000,000000,000001,000000,000000,000000,000001,00000⎤⎦⎥⎥
Dividir la fila 1 entre 9:
⎡⎣⎢⎢1,000003.8100031.681000.423331.796106.256813.520116.25681810.244620.111110,000000,000000,000001,000000,000000,000000,000001,00000⎤⎦⎥⎥
Reste 3,81 multiplicado por la fila 1 de la fila 2:
⎡⎣⎢⎢1,000000,0000031.681000.423330.183206.256813.52011- 7.15481810.244620.11111- 0,423330,000000,000001,000000,000000,000000,000001,00000⎤⎦⎥⎥
Reste 31.681 veces la fila 1 de la fila 3:
⎡⎣⎢⎢1,000000,000000,000000.423330.18320- 7.154813.52011- 7.15481698.724020.11111- 0,42333- 3.520110,000001,000000,000000,000000,000001,00000⎤⎦⎥⎥
Divida la fila 2 entre 0,1832:
⎡⎣⎢⎢1,000000,000000,000000.423331,00000- 7.154813.52011- 39.05464698.724020.11111- 2.31077- 3.520110,000005.458520,000000,000000,000001,00000⎤⎦⎥⎥
Suma 7,15481 veces la fila 2 a la fila 3:
⎡⎣⎢⎢1,000000,000000,000000.423331,000000,000003.52011- 39.05464419.295490.11111- 2.31077- 20.053230,000005.4585239.054670,000000,000001,00000⎤⎦⎥⎥
Dividir la fila 3 entre 419.29549:
⎡⎣⎢⎢1,000000,000000,000000.423331,000000,000003.52011- 39.054641,000000.11111- 2.31077- 0.047830,000005.458520.093140,000000,000000,00239⎤⎦⎥⎥
Sumar 39.05464 veces la fila 3 a la fila 2:
⎡⎣⎢⎢1,000000,000000,000000.423331,000000,000003.520110,000001,000000.11111- 4.17859- 0.047830,000009.096070.093140,000000.093340,00239⎤⎦⎥⎥
Reste 3,52011 multiplicado por la fila 3 de la fila 1:
⎡⎣⎢⎢1,000000,000000,000000.423331,000000,000000,000000,000001,000000.27946- 4.17859- 0.04783- 0.327869.096070.09314- 0,008410.093340,00239⎤⎦⎥⎥
Reste 42333 multiplicado por la fila 2 de la fila 1:
⎡⎣⎢⎢1,000000,000000,000000,000001,000000,000000,000000,000001,000002.04838- 4.17859- 0.04783- 4.178499.096070.09314- 0.047920.093340,00239⎤⎦⎥⎥= [ I | (X′X)- 1]
Como una pequeña arruga en estos cálculos manuales, podemos ver que ( X′X ) −1 está un poco fuera de lugar debido a un error de redondeo. En realidad, debería ser una matriz simétrica.
Respuesta final a mano
Si postmultiplicamos ( X′X ) −1 por X′y obtenemos:
(X′X)- 1X′y =⎡⎣⎢2.04838- 4.17859- 0.04783- 4.178499.096070.09314- 0.047920.093340,00239⎤⎦⎥⎡⎣⎢5.42302.094328.0059⎤⎦⎥=⎡⎣⎢1.0178- 1,00180,0025⎤⎦⎥=β^
O en forma escalar:   y^I= 1.0178 - 1.0018X2 yo+ 0,0025X3 yo .
10.3.2 Escribir un estimador de MCO en R
Dado que invertir la matriz requirió tantos pasos e incluso se produjo algún error de redondeo, será más fácil que R  haga algo del trabajo pesado por nosotros. (A medida que aumente el número de observaciones o variables, encontraremos la ayuda computacional aún más valiosa). Para programar nuestro propio estimador de MCO en R , los comandos clave que necesitamos son:
Multiplicación de matrices: % *%

Transponer: t

Matriz inversa: resolver

Sabiendo esto, es fácil programar un estimador para   β^= (X′X)- 1X′y .

Primero debemos ingresar nuestros vectores variables usando los datos de la tabla  10.1 y combinar las variables de entrada en una matriz. Si aún no los ha introducido, escriba:

Y <-c (.808, .817, .568, .571, .421, .673, .724, .590, .251)

X1 <- rep (1, 9)

X2 <-c (.29, .34, .37, .34, .56, .37, .34, .43, .77)

X3 <-c (4.984,5.073,12.620, -6.443, -5.758,15.603,14.148,0.502,

     -9.048)

X <-cbind (X1, X2, X3)

Para estimar MCO con nuestro propio programa, simplemente necesitamos traducir el estimador ( X′X ) −1 X′y a la  sintaxis R :

beta.hat <-solve (t (X)% *% X)% *% t (X)% *% Y

beta.hat

Desglosando esto un poco: el comando solve comienza porque la primera cantidad es inversa, ( X′X ) −1 . Dentro de la llamada a resolver , el primer argumento debe transponerse (de ahí el uso de t ) y luego se posmultiplica por la matriz de covariables no transpuesta (de ahí el uso de % *% ). Seguimos postmultiplicando la transpuesta de X , luego postmultiplicando el vector de resultados ( y ). Cuando imprimimos nuestros resultados, obtenemos:

           [, 1]

    1.017845630

X2 -1.001809341

X3 0,002502538

Estos son los mismos resultados que obtuvimos a mano, a pesar de las discrepancias de redondeo que encontramos al invertir por nuestra cuenta. Sin embargo, escribir el programa en R nos dio al mismo tiempo un control total sobre el estimador, siendo mucho más rápido que las operaciones manuales.

Por supuesto, una opción aún más rápida sería usar el comando enlatado lm que usamos en el Cap. 6 :  

tennessee <- como.data.frame (cbind (Y, X2, X3))

lm (Y ~ X2 + X3, datos = Tennessee)

Esto también produce exactamente los mismos resultados. En la práctica, siempre que se calculan las estimaciones de MCO, este es casi siempre el enfoque que queremos adoptar. Sin embargo, este procedimiento nos ha permitido verificar la utilidad de los comandos de álgebra matricial de R. Si el usuario encuentra la necesidad de programar un estimador más complejo para el cual no existe un comando predefinido, éste debería ofrecer herramientas relevantes para lograrlo.

10.3.3 Otras aplicaciones
Con estas herramientas básicas en la mano, los usuarios ahora deberían poder comenzar a programar con matrices y vectores. Algunas aplicaciones intermedias de esto incluyen: calcular errores estándar a partir de modelos estimados con optim (ver capítulo  11 ) y valores predichos para formas funcionales complicadas (ver el código que produjo la figura  9.3 ). Algunas de las aplicaciones más avanzadas que pueden beneficiarse del uso de estas herramientas incluyen: mínimos cuadrados generalizados factibles (incluidos los mínimos cuadrados ponderados), optimización de funciones de verosimilitud para distribuciones normales multivariadas y generación de datos correlacionados mediante la descomposición de Chol esky (consulte el comando chol ). Muy pocos programas ofrecen la flexibilidad de estimación y programación que  R lo  hace siempre que el álgebra matricial es esencial para el proceso.

10.4 Problemas de práctica
Para estos problemas de práctica, considere los datos sobre las elecciones al Congreso en Arizona en 2010, que se presentan a continuación en la Tabla  10.2 :
Cuadro 10.2
Datos de las elecciones al Congreso de Arizona en 2010

Distrito

Republicano ( y )

Constante ( x 1 )

Obama ( x 2 )

Financiamiento ( x 3 )

 
1

0,50

1

0,44

−9,11

 
2

0,65

1

0,38

8.31

 
3

0,52

1

0,42

8.00

 
4

0,28

1

0,66

−8,50

 
5

0,52

1

0,47

−7,17

 
6

0,67

1

0,38

5.10

 
7

0,45

1

0,57

−5,32

 
8

0,47

1

0,46

−18,64

 
Nota: Datos de Monogan (2013a)

1. Cree un vector que consista en la proporción republicana del voto bipartidista ( y ) en los ocho distritos electorales de Arizona. Usando cualquier medio que prefiera, cree una matriz, X , en la que las ocho filas representan los ocho distritos de Arizona, y las tres columnas representan un vector constante de unos ( x 1 ), la participación de Obama en 2008 en los votos ( x 2 ) y el Saldo financiero republicano ( x 3 ). Imprima su vector y matriz, y pídale a R  que calcule la longitud del vector y las dimensiones de la matriz. ¿Todos estos resultados son consistentes con los datos que ve en la Tabla  10.2 ?

2. Usando su matriz, X , calcule X'X usando los comandos de R. Cual es tu resultado?

3. Usando los comandos de R , encuentre la inversa de su respuesta anterior. Es decir, calcule ( X′X ) −1 . Cual es tu resultado?

4. Con los datos de la tabla  10.2 , considere el modelo de regresión  yI=β1X1 yo+β2X2 yo+β3X3 yo+tuI . Usando los comandos de álgebra matricial de R , calcule  β^= (X′X)- 1X′y . ¿Cuáles son sus estimaciones de los coeficientes de regresión?  β^1 ,   β^2 , y   β^3 ? ¿Cómo se comparan sus resultados si los calcula usando el comando lm del Cap. 6 ? 

Notas al pie
1 .
Alternativamente, cuando los elementos combinados son objetos complejos, c crea en su lugar un objeto de lista .

2 .
Para los lectores interesados ​​en bootstrapping, que es una de las aplicaciones más comunes de muestreo a partir de datos, el enfoque más eficiente será instalar el paquete de arranque y probar algunos de los ejemplos que ofrece la biblioteca.

3 .
Una sintaxis alternativa habría sido X2% o% X3 .

4 .
Una variedad única de multiplicación de matrices se denomina producto de Kronecker ( H ⊗ L ). El producto Kronecker tiene aplicaciones útiles en el análisis de datos de panel. Consulte el comando kronecker en R  para obtener más información.

5 .
Como otra aplicación en estadística, la función de verosimilitud para una distribución normal multivariante también recurre al determinante de la matriz de covarianza.

Material suplementario
318886_1_En_10_MOESM1_ESM.zip (106 kb)
Dataverse (2,154 KB)
Referencias
Monogan JE III (2013a) Un caso para registrar estudios de resultados políticos: una aplicación en las elecciones a la Cámara de 2010. Polit Anal 21 (1): 21–37
CrossRefGoogle Académico
