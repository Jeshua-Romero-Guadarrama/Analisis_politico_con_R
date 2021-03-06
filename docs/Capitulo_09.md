# Análisis de series temporales {#Análisisdeseriestemporales}

<hr style="background-color:#03193b;height:2px">

Palabras clave:
- Series de tiempo 
- Mínimos cuadrados ordinarios 
- Análisis de series temporales 
- Correlación en serie 
- Variable endógena 

La mayoría de los métodos descritos hasta ahora en este libro están orientados principalmente al análisis transversal, o al estudio de una muestra de datos tomados en el mismo momento. En este capítulo, pasamos a los métodos para modelar una serie de tiempo, o una variable que se observa secuencialmente a intervalos regulares a lo largo del tiempo (por ejemplo, diaria, semanal, mensual, trimestral o anual). Los datos de series de tiempo con frecuencia tienen tendencias y procesos de error complejos, por lo que no tener en cuenta estas características puede producir resultados falsos (Granger y Newbold 1974). Han surgido varios enfoques para el análisis de series de tiempo para abordar estos problemas y prevenir inferencias falsas. Dentro de la ciencia política, los académicos de la opinión pública, la economía política, los conflictos internacionales y varios otros temas trabajan regularmente con datos referenciados en el tiempo, por lo que las herramientas adecuadas para el análisis de series de tiempo son importantes en el análisis político.

Muchos investigadores no piensan en R  como un programa para el análisis de series de tiempo, sino que utilizan software especializado como Autobox, EViews o RATS. Incluso SAS y Stata tienden a conseguir más atención para el análisis de series temporales de R  hace. Sin embargo, R en  realidad tiene una amplia gama de comandos para modelos de series de tiempo, particularmente a través de los paquetes TSA y vars . En este capítulo, ilustraremos tres enfoques para el análisis de series de tiempo en R : el enfoque de Box-Jenkins, extensiones a modelos lineales estimados con mínimos cuadrados y autorregresión vectorial. Esta no es una lista exhaustiva de las herramientas disponibles para estudiar series de tiempo, pero solo pretende presentar algunos métodos destacados. Ver secc. 9.4 para leer más sobre series de tiempo.

Tanto el enfoque de Box-Jenkins como las extensiones de los modelos lineales son ejemplos de modelos de series de tiempo de ecuación única. Ambos enfoques tratan una serie de tiempo como una variable de resultado y se ajustan a un modelo de ese resultado que puede expresarse en una ecuación, al igual que los modelos de regresión de los capítulos anteriores pueden expresarse en una sola ecuación. Dado que ambos enfoques entran en esta categoría amplia, el conjunto de datos de trabajo que usamos tanto para el enfoque de Box-Jenkins como para las extensiones de los modelos lineales será el de Peake y Eshbaugh-Soha (2008) datos mensuales sobre la cobertura televisiva de la política energética que se introdujeron por primera vez en el Cap. 3 Por el contrario, la autorregresión vectorial es un modelo de series de tiempo de ecuaciones múltiples (para más detalles sobre esta distinción, consulte Brandt y Williams  2007 o Lütkepohl 2005). Con un modelo de autorregresión vectorial, dos o más series de tiempo se consideran endógenas, por lo que se requieren múltiples ecuaciones para especificar completamente el modelo. Esto es importante porque las variables endógenas pueden afectarse entre sí y, para interpretar el efecto de una variable de entrada, se debe considerar el contexto más amplio del sistema completo. Dado que los modelos de ecuaciones múltiples tienen una especificación tan diferente, al discutir la autorregresión vectorial, el ejemplo de trabajo será Brandt y Freeman (2006) análisis de acciones políticas semanales en el conflicto israelo-palestino; Se plantearán más detalles una vez que lleguemos a esa sección.

9.1 El método Box-Jenkins
El enfoque de Box-Jenkins para las series de tiempo se basa en modelos de promedio móvil integrado autorregresivo (ARIMA) para capturar el proceso de error en los datos. Para obtener una explicación completa de este enfoque, consulte Box et al. (2008). La lógica básica de este enfoque es que una serie de tiempo debe filtrarse, o blanquearse previamente , de cualquier tendencia y proceso de error antes de intentar ajustar un modelo inferencial. Una vez que se ha especificado un modelo del ruido o error, el investigador puede proceder a probar las hipótesis. 1 Debido a las formas funcionales no lineales que a menudo surgen en ARIMA y en los modelos de función de transferencia, estas generalmente se estiman con la máxima probabilidad.

Para ilustrar el proceso desde la identificación de un modelo de error hasta la prueba de una hipótesis, revisamos la de Peake y Eshbaugh-Soha (2008) Serie temporal mensual sobre cobertura de pólizas energéticas. Comenzamos recargando nuestros datos: 2

pres.energy <-read.csv ("PESenergy.csv")

Nuestra variable de resultado es el número de historias relacionadas con la energía en los noticieros de televisión nocturnos por mes ( Energía ). Un buen primer paso es trazar la serie que se está estudiando para ver si alguna tendencia u otras características son evidentes de inmediato. Consulte la figura  3.6 para ver el código de ejemplo que usamos para trazar nuestra variable de resultado ( Energía ) y el precio del petróleo, que es uno de los predictores ( petróleoc ). Como un primer vistazo a la serie de cobertura de noticias, los datos parecen estar estacionarios , lo que significa que se mueven alrededor de una media sin tener una tendencia en una dirección o mostrar un patrón integrado . 

Como punto sustancialmente importante sobre las series de tiempo en general, la distinción entre series estacionarias y no estacionarias es importante. Muchos métodos de series de tiempo están diseñados específicamente para modelar series estacionarias, por lo que su aplicación a una serie no estacionaria puede resultar problemático. Una serie estacionaria es aquella en la que la media y la varianza no cambian condicionalmente en el tiempo, y la serie no tiene una tendencia en una dirección. Si una serie estacionaria se altera o se mueve a un valor más alto o más bajo, eventualmente regresa a un nivel de equilibrio. Los dos tipos de series no estacionarias son series de tendencia y series integradas. Con tendenciaserie, como su nombre lo indica, el promedio condicional cambia con el tiempo, por lo general subiendo o bajando de manera constante. Por ejemplo, los precios nominales de los bienes de consumo pueden subir o bajar de un trimestre a otro, pero el valor medio tiende a subir con el tiempo. Una serie integrada, también llamada serie de raíz unitaria, no tiene un valor de equilibrio y tiene la característica de "memoria larga". Esto significa que si algo cambia el valor de una serie, los valores futuros de la serie se verán afectados por ese cambio en el futuro. Por ejemplo, supongamos que el promedio industrial Dow Jones cayó 300 puntos en un día, de 18.200 a 17.900. En tal caso, los valores futuros del Dow se basarán en el nuevo valor de 17.900 más lo mucho que subió o bajó el valor cada día subsiguiente, y el Dow no tenderá a volver a los valores anteriores. Es importante destacar que al diferenciarel valor actual de una serie a partir de su valor anterior, o midiendo cuánto cambió una serie de una vez a la siguiente, las series integradas o en tendencia se pueden hacer estacionarias. Por lo tanto, se pueden aplicar muchos modelos para series estacionarias a la diferencia de una serie no estacionaria. Siempre que realice un trabajo de series de tiempo aplicadas, observe siempre el gráfico de la serie, como la figura  3.6 en este caso, así como los diagnósticos que se describirán para determinar si está estudiando una serie estacionaria, de tendencia o de raíz unitaria. 3 

Como segundo paso, nos volvemos a medidas de diagnóstico y vistazo a la una uto c orrelation f unción (ACF) y p artial un uto c orrelation f unción (FAP). La función de autocorrelación muestra cuánto se correlacionan los valores actuales de una serie con los valores anteriores en un cierto retraso . Retrasosson importantes para el modelado de series de tiempo, como se verá más adelante en el capítulo, y actúan cambiando el índice de una serie para hacer retroceder el tiempo y formar una nueva serie. Por ejemplo, un retraso de primer orden es una nueva serie donde cada observación es el valor que ocurrió un período de tiempo anteriormente. Un retraso de segundo orden es una nueva serie en la que cada observación es el valor de dos períodos de tiempo anteriores, y así sucesivamente. Esto es importante cuando se usa la función de autocorrelación porque, si estamos estudiando una serie y con índice de tiempo t , ACF (1) nos daría la correlación entre y t y y t −1 . (Para la cobertura de energía, entonces, ACF (1) nos dice la correlación entre la cobertura de los meses actuales con la cobertura de los meses anteriores). Posteriormente, ACF (2) sería la autocorrelación entre y t y y t −2 , y más generalmente ACF (p) es la autocorrelación entre y t y y t - p . El PACF proporciona la autocorrelación entre los valores actuales y rezagados que no se tienen en cuenta en todos los rezagos anteriores . Entonces, para el primer retraso, ACF (1) = PACF (1), pero para todos los retrasos posteriores, solo la autocorrelación única de ese retraso aparece en el PACF. Si quisiéramos conocer los ACF y PACF de nuestra serie de políticas energéticas, escribiríamos:

acf (pres.energy $ Energy, lag.max = 24)

pacf (pres.energy $ Energy, lag.max = 24)

Las funciones acf y pacf están disponibles sin cargar un paquete, aunque el código cambia ligeramente si los usuarios cargan el paquete TSA . 4 Observe que dentro de las funciones acf y pacf , primero enumeramos las series que estamos diagnosticando. En segundo lugar, designamos lag.max , que es el número de retrasos de autocorrelación que deseamos considerar. Dado que estos son datos mensuales, 24 rezagos nos dan 2 años de rezagos. En algunas series, la estacionalidademergerá, en el que vemos evidencia de valores similares en la misma época cada año. Esto se vería con rezagos significativos alrededor de 12 y 24 con datos mensuales (o alrededor de 4, 8 y 12, por el contrario, con datos trimestrales). No aparece tal evidencia en este caso.

Los gráficos de nuestro ACF y PACF se muestran en la Fig.  9.1 . En cada una de estas figuras, el eje horizontal representa la longitud del rezago y el eje vertical representa el valor de correlación (o correlación parcial). En cada retraso, la autocorrelación para ese retraso se muestra con una línea sólida similar a un histograma desde cero hasta el valor de la correlación. Las líneas discontinuas azules representan el umbral para una correlación significativa. Específicamente, las bandas azules representan el intervalo de confianza del 95% basado en una serie no correlacionada. 5 Todo esto significa que si la línea similar a un histograma no cruza la línea discontinua, el nivel de autocorrelación no es perceptiblemente diferente de cero, pero los picos de correlación fuera de la banda de error son estadísticamente significativos.
Abrir imagen en nueva ventanaFigura 9.1
Figura 9.1
Función de autocorrelación y función de autocorrelación parcial de la cobertura televisiva mensual de la política energética a través de 24 rezagos. ( a ) ACF. ( b ) PACF

El ACF, en la figura  9.1a comienza mostrando la autocorrelación de retardo cero, que es cuánto se correlacionan los valores actuales con ellos mismos, siempre exactamente 1.0. Luego, vemos los valores más informativos de autocorrelación en cada rezago. Con un retraso, por ejemplo, la correlación en serie es 0,823. Para dos rezagos, cae a 0.682. El sexto retraso es el último retraso en mostrar una autocorrelación discernible, por lo que podemos decir que el ACF decae rápidamente. El PACF, en la figura  9.1 b, omite el retardo cero y comienza con una correlación serial de primer orden. Como se esperaba, esto es 0.823, tal como mostró el ACF. Sin embargo, una vez que tenemos en cuenta la correlación serial de primer orden, los términos de autocorrelación parcial en los retrasos posteriores no son estadísticamente discernibles. 6

En este punto, determinamos qué proceso de error ARIMA dejaría una huella empírica como la que muestran este ACF y PACF. Para obtener más detalles sobre las huellas comunes, consulte Enders (2009, pag. 68). Notacionalmente, llamamos al proceso de error ARIMA ( p , d , q ), donde p es el número de términos autorregresivos, d es la cantidad de veces que la serie necesita ser diferenciada y q es el número de términos de promedio móvil. Funcionalmente, un modelo ARMA general , que incluye componentes de media móvil y autorregresiva, se escribe de la siguiente manera:
yt=α0+∑i = 1pagαIyt - yo+∑i = 1qβIϵt - yo+ϵt
(9,1)
Aquí, y t es nuestra serie de interés, y este modelo ARMA se convierte en un modelo ARIMA cuando decidimos si necesitamos diferenciar y t o no. Observe que y t se retrasa p veces para los términos autorregresivos, y el término de perturbación ( ε t ) se retrasa q veces para los términos de promedio móvil. En el caso de la cobertura de la póliza energética, el ACF muestra un rápido decaimiento y vemos un pico significativo en el PACF, por lo que podemos decir que estamos ante un proceso autorregresivo de primer orden, denotado AR (1) o ARIMA (1,0 , 0).
Una vez que hemos identificado nuestra una uto r egressive i NTEGRADO m Oving un modelo verage, se puede estimar utilizando la Arima función:

ar1.mod <-arima (pres.energy $ Energy, order = c (1,0,0))

La primera entrada es la serie que estamos modelando, y la opción de orden nos permite especificar p , d y q (en orden) para nuestro proceso ARIMA ( p , d , q ). Al escribir ar1.mod , vemos la salida de nuestros resultados:

Llamada:

arima (x = energía pres. $ Energía, orden = c (1, 0, 0))

Coeficientes:

         intercepción ar1

      0.8235 32.9020

se 0.0416 9.2403

sigma ^ 2 estimado como 502,7: probabilidad logarítmica = -815,77,

 aic = 1637.55

Simplemente, esto nos muestra la estimación y el error estándar del coeficiente autorregresivo ( ar1 ) y la intersección, así como la varianza residual, la probabilidad logarítmica y el AIC.

El siguiente paso en el proceso de modelado de Box-Jenkins es diagnosticar si el modelo estimado filtra suficientemente los datos. 7 Hacemos esto de dos maneras: Primero, estudiando el ACF y PACF para los residuos del modelo ARIMA. El código es:

acf (ar1.mod $ residuales, lag.max = 24)

pacf (ar1.mod $ residuales, lag.max = 24)

Al igual que con muchos otros modelos, podemos llamar a nuestros residuos usando el nombre del modelo y un signo de dólar ( ar1.mod $ residuales ). Los gráficos resultantes se presentan en la figura  9.2 . Como muestran tanto el ACF como el PACF, el segundo y el cuarto rezagos apenas cruzan el umbral de significancia, pero no hay un patrón claro o evidencia de una característica pasada por alto del proceso de error. La mayoría (pero no todos) los analistas estarían satisfechos con este patrón en estas cifras.
Abrir imagen en nueva ventanaFigura 9.2
Figura 9.2
Función de autocorrelación y función de autocorrelación parcial para los residuos del modelo AR (1) a través de 24 rezagos. Las líneas discontinuas azules representan un intervalo de confianza del 95% para una serie no correlacionada. ( a ) ACF. ( b ) PACF (figura de color en línea)

Como segundo paso de diagnosticar si hemos filtrado suficientemente los datos, se calcula el Ljung- Box Q - prueba . Esta es una prueba conjunta de varios rezagos para determinar si existe evidencia de correlación serial en alguno de los rezagos. La hipótesis nula es que los datos son independientes, por lo que un resultado significativo sirve como evidencia de un problema. La sintaxis de esta prueba es:

Box.test (ar1.mod $ residuals, lag = 24, type = "Ljung-Box")

Primero especificamos la serie de interés, luego, con la opción de retraso, indicamos cuántos retrasos deben entrar en la prueba y, por último, especificamos con el tipo Ljung-Box (a diferencia de la prueba Box-Pierce, que no funciona tan bien). Nuestros resultados de esta prueba son:

        Prueba de Box-Ljung

datos: ar1.mod $ residuales

X al cuadrado = 20,1121, gl = 24, valor p =

0,6904

El estadístico de nuestra prueba no es significativo ( p  = 0. 6904), por lo que esta prueba no muestra evidencia de correlación serial durante 2 años de retraso. Si estamos satisfechos de que AR (1) caracteriza nuestro proceso de error, podemos proceder al modelado real en la siguiente sección.

9.1.1 Funciones de transferencia frente a modelos estáticos
Para estimar un modelo teóricamente motivado, necesitamos establecer una distinción importante entre dos tipos de formas funcionales: por un lado, una forma funcional estática supone que los valores actuales de los predictores afectan los valores actuales de los resultados. Por ejemplo, podemos creer que el precio del petróleo en un mes determinado afecta la cobertura de temas energéticos en la televisión en el mismo mes, mientras que los precios del petróleo del próximo mes afectarán la cobertura energética del próximo mes. Si pensamos que este es el único proceso en curso, entonces queremos una forma funcional estática. Por otro lado, podemos utilizar una dinámicaforma funcional. En el caso de los precios del petróleo, esto significaría que los precios del petróleo de este mes afectarían la cobertura energética de este mes, y también la cobertura del mes siguiente en menor grado, el mes siguiente en menor grado aún, y así sucesivamente. Esencialmente, una forma funcional dinámica permite efectos secundarios de cada observación de una entrada.

Estimación de un modelo de regresión estática con un proceso de error ARIMA sólo requiere dos pasos en R . Primero, todas las variables predictoras deben colocarse en una matriz. En segundo lugar, simplemente podemos agregar una opción a nuestra función arima :

predictores <-as.matrix (subconjunto (pres.energy, select = c (rmn1173,

     grf0175, grf575, jec477, jec1177, jec479, embargo, rehenes,

     oilc, Aprobación, Desempleo)))

static.mod <-arima (pres.energy $ Energy, order = c (1,0,0),

     xreg = predictores)

En este código, primero subconjuntamos nuestro conjunto de datos original y tratamos el subconjunto como una matriz denominada predictores . En segundo lugar, usamos la misma función arima que usamos para estimar nuestro modelo de ruido AR (1), pero agregamos la opción xreg = predictores . Esto ahora estima un modelo en el que se corrige el proceso de error temporal, pero también incluimos predictores de interés motivados teóricamente. Si escribimos static.mod , la salida es:

Llamada:

arima (x = energía pres. $ Energía, orden = c (1,0,0), xreg =

 predictores)

Coeficientes:

         ar1 intercepción rmn1173 grf0175 grf575

      0,8222 5,8822 91,3265 31,8761 -8,2280

se 0.0481 52.9008 15.0884 15.4643 15.2025

       jec477 jec1177 jec479 embargo rehenes

      29.6446 -6.6967 -20.1624 35.3247 -16.5001

se 15.0831 15.0844 15.2238 15.1200 13.7619

        Oilc Aprobación Desempleo

      0,8855 -0,2479 1,0080

se 1.0192 0.2816 3.8909

sigma ^ 2 estimado como 379,3: probabilidad logarítmica = -790,42,

 aic = 1608,84

Esto ahora nos muestra la estimación y el error estándar no solo para el coeficiente autorregresivo y la intersección, sino también para el coeficiente de regresión parcial de todos los demás predictores del modelo. Nuevamente, las tres medidas de ajuste se informan al final de la impresión.

Si quisiéramos incluir uno o más predictores que tengan un efecto dinámico, entonces recurrimos al método de las funciones de transferencia , que especifican que un predictor tiene un efecto de desbordamiento en las observaciones posteriores. Un caso especial de esto se llama análisis de intervención , en el que un tratamiento se codifica con un indicador (Box y Tiao 1975). Con el análisis de intervención, pueden surgir una variedad de formas funcionales, dependiendo en gran medida de si la variable indicadora está codificada como un pulso (tomando un valor de 1 solo en el momento del tratamiento y 0 en caso contrario) o un paso (tomando un valor de 0 antes del tratamiento). el tratamiento y 1 en todos los momentos posteriores). Es recomendable probar ambas codificaciones y leer más en la forma funcional de cada una (Enders 2009, Secta. 5.1 ). 

Como ilustración, ajustamos una función de transferencia para una intervención de pulso para el discurso de Richard Nixon en noviembre de 1973. Para estimar este modelo, ahora necesitamos cargar el paquete TSA para acceder a la función arimax ( ARIMA con predictores x ). Recuerde que es posible que deba usar install.packages para descargar TSA :

install.packages ("TSA")

biblioteca (TSA)

dynamic.mod <-arimax (pres.energy $ Energy, order = c (1,0,0),

     xreg = predictores [, - 1], xtransf = predictores [, 1],

     transferencia = lista (c (1,0)))

La sintaxis de arimax es similar a la de arima , pero ahora se nos permiten algunas opciones más para las funciones de transferencia. Observe en este caso que usamos el código xreg = predictores [-1] para eliminar el indicador del discurso de noviembre de 1973 de Nixon de los predictores estáticos. En su lugar, colocamos este predictor con la opción xtransf . Lo último que debemos hacer es especificar el orden de nuestra función de transferencia, lo que hacemos con la opción de transferencia . La opción de transferencia acepta una lista de vectores, un vector por predictor de función de transferencia. Para nuestra única función de transferencia, especificamos c (1,0): El primer término se refiere al orden del término de decaimiento dinámico (por lo que un 0 aquí en realidad vuelve a ser un modelo estático), y el segundo término se refiere a la duración del retraso del efecto del predictor (por lo que si esperábamos que un efecto creciera , podríamos poner un número mayor que 0 aquí). Con estos ajustes, decimos que el discurso de Nixon tuvo un efecto en el mes que lo pronunció, y luego el efecto se extendió a los meses siguientes a un ritmo decreciente.

Al escribir dynamic.mod , obtenemos nuestro resultado:

Llamada:

arimax (x = energía pres. $ Energía, orden = c (1,0,0),

 xreg = predictores [, - 1],

    xtransf = predictores [, 1], transferencia = lista (c (1, 0)))

Coeficientes:

         ar1 intercepción grf0175 grf575 jec477

      0,8262 20,2787 31,5282 -7,9725 29,9820

se 0.0476 46.6870 13.8530 13.6104 13.5013

      jec1177 jec479 embargo rehenes oilc

      -6,3304 -19,8179 25,9388 -16,9015 0,5927

se 13.5011 13.6345 13.2305 12.4422 0.9205

      Aprobación Desempleo T1-AR1 T1-MA0

       -0,2074 0,1660 0,6087 160,6241

se 0,2495 3,5472 0,0230 17,0388

sigma ^ 2 estimado como 305,1: probabilidad logarítmica = -770,83,

 aic = 1569,66

El resultado es similar al del modelo de regresión ARIMA estático, pero ahora hay dos términos para el efecto del habla de Nixon. El primero, T1-AR1 , da el término de desintegración. Cuanto más cercano esté este término a 1, más persistente será el efecto de la variable. El segundo término, T1-MA0 , es el efecto inicial del discurso sobre la cobertura energética en el mes en que se dio. 8En términos de ajuste del modelo, observe que la salida de cada ARIMA o modelo de función de transferencia que hemos estimado informa el criterio de información de Akaike (AIC). Con esta medida, las puntuaciones más bajas indican un mejor ajuste penalizado. Con una puntuación de 1569,66, este modelo de función de transferencia dinámica tiene el AIC más bajo de cualquier modelo que hayamos ajustado a estos datos. Por lo tanto, el modelo dinámico tiene un mejor ajuste que el modelo estático o el modelo ateórico AR (1) sin predictores.

Sin embargo, para tener una idea real del efecto de un análisis de intervención, un analista siempre debe intentar dibujar el efecto que modeló. (Nuevamente, es clave estudiar la forma funcional detrás de la especificación de intervención elegida, como lo describe Enders 2009, Secta. 5.1 .) Para dibujar el efecto de nuestra intervención para el discurso de Nixon de 1973, escribimos:  

meses <-c (1: 180)

y.pred <-dynamic.mod $ coef [2:12]% *% c (1, predictores [58, -1]) +

     160,6241 * predictores [, 1] +

     160.6241 * (. 6087 ^ (meses-59)) * como numérico (meses> 59)

plot (y = energía pres. $ Energía, x = meses, xlab = "Mes",

     ylab = "Historias de políticas energéticas", tipo = "l", ejes = F)

eje (1, en = c (1,37,73,109,145), etiquetas = c ("Enero de 1969",

     "Ene. 1972", "Ene. 1975", "Ene. 1978", "Ene. 1981"))

eje (2)

caja()

líneas (y = y.pred, x = meses, lty = 2, col = "blue", lwd = 2)

En la primera línea, simplemente creamos un índice de tiempo para los 180 meses del estudio. En la segunda línea, creamos valores predichos para el efecto de la intervención manteniendo todo lo demás igual. Una suposición crítica que hacemos es que mantenemos todos los demás predictores iguales al establecerlos en sus valores de octubre de 1973, el mes 58 de la serie (por lo tanto, predictores [58, -1]). Entonces, considerando los componentes de esta segunda línea, el primer término multiplica los coeficientes de los predictores estáticos por sus últimos valores antes de la intervención, el segundo término captura el efecto de la intervención en el mes del discurso y el tercer término captura el efecto de desbordamiento. efecto de la intervención en función del número de meses transcurridos desde el discurso. Las siguientes cuatro líneas simplemente dibujan un gráfico de los valores de nuestra serie original y administran algunas de las características del gráfico. Por último, agregamos una línea discontinua que muestra el efecto de la intervención manteniendo constante todo lo demás. El resultado se muestra en la Fig.  9.3. Como muestra la figura, el resultado de esta intervención es un salto grande y positivo en el número esperado de noticias, que se prolonga durante unos meses, pero finalmente regresa a los niveles previos a la intervención. Este tipo de gráfico es esencial para comprender cómo la intervención dinámica afecta realmente al modelo.
Abrir imagen en nueva ventanaFigura 9.3
Figura 9.3
La línea discontinua muestra el efecto dinámico del discurso de Nixon de noviembre de 1973, manteniendo todo lo demás igual. La línea continua muestra los valores observados de la serie.

Como gráfico final para complementar nuestra visión del efecto de la intervención dinámica, podríamos dibujar un gráfico que muestre qué tan bien se alinean las predicciones del modelo completo con los valores reales de la serie. Podríamos hacer esto con el siguiente código:

meses <-c (1: 180)

full.pred <-pres.energy $ Energy-dynamic.mod $ residuales

plot (y = full.pred, x = months, xlab = "Month",

     ylab = "Historias de políticas energéticas", type = "l",

     ylim = c (0,225), ejes = F)

puntos (y = energía pres. $ Energía, x = meses, pch = 20)

leyenda (x = 0, y = 200, leyenda = c ("Previsto", "Verdadero"),

     pch = c (NA, 20), lty = c (1, NA))

eje (1, en = c (1,37,73,109,145), etiquetas = c ("Enero de 1969",

     "Ene. 1972", "Ene. 1975", "Ene. 1978", "Ene. 1981"))

eje (2)

caja()

Nuevamente, comenzamos creando un índice de tiempo, meses . En la segunda línea, creamos nuestros valores predichos restando los residuos de los valores verdaderos. En la tercera línea de código, dibujamos un gráfico lineal de los valores predichos del modelo. En la cuarta línea, agregamos puntos que muestran los valores reales de la serie observada. Las líneas restantes completan el formato del gráfico. El gráfico resultante se muestra en la figura  9.4 . Como puede verse, el ajuste en la muestra es bueno, con los valores predichos siguiendo de cerca los valores verdaderos.
Abrir imagen en nueva ventanaFigura 9.4
Figura 9.4
Valores predichos de un modelo de función de transferencia completo en una línea, con valores observados reales como puntos

Como punto final aquí, se anima al lector a consultar el código en la Sección. 9.5 para una sintaxis alternativa para producir las Figs. 9.3 y  9.4 . La compensación de la forma alternativa de dibujar estas figuras es que requiere más líneas de código por un lado, pero por otro lado, es más generalizable y más fácil de aplicar a su propia investigación. Además, el código alternativo presenta cómo el comando ts permite a los analistas convertir una variable en un objeto de serie temporal . Al ver a ambos enfoques es que vale la pena para ilustrar que, en general, muchas tareas se pueden realizar de muchas maneras en R .

9.2 Extensiones a modelos de regresión lineal por mínimos cuadrados
Un segundo enfoque para el análisis de series de tiempo se basa más en la literatura econométrica y busca formas de extender los modelos de regresión lineal para tener en cuenta los problemas únicos asociados con los datos referenciados en el tiempo. Dado que ya discutimos ampliamente la visualización con estos datos en la Sect. 9.1 , no revisaremos los problemas de gráficos aquí. Sin embargo, al igual que con los modelos de tipo Box-Jenkins, el analista siempre debe comenzar dibujando una gráfica lineal de la serie de interés e idealmente también algunos predictores clave. Incluso los gráficos de diagnóstico como el ACF y PACF serían apropiados, además de los diagnósticos residuales como los que discutiremos en breve.

Al modelar datos desde un enfoque econométrico, los investigadores nuevamente tienen que decidir si usar una especificación estática o dinámica del modelo. Para los modelos estáticos en los que los valores actuales de los predictores afectan los valores actuales del resultado, los investigadores pueden estimar el modelo con mínimos cuadrados ordinarios (MCO) en el raro caso de que no haya correlación serial. Sin embargo, para las ganancias de eficiencia en un modelo estático, los mínimos cuadrados generalizados factibles (FGLS) son un mejor estimador. Por el contrario, en el caso de una forma funcional dinámica, se puede introducir una estructura de retardo en la especificación del modelo lineal.

Comenzando con modelos estáticos, el tipo de modelo más simple (aunque raramente apropiado) sería estimar el modelo usando MCO simple. Volviendo a los datos de nuestra política energética, la especificación de nuestro modelo aquí sería:

static.ols <-lm (Energía ~ rmn1173 + grf0175 + grf575 + jec477 +

     jec1177 + jec479 + embargo + rehenes + oilc +

     Aprobación + Desempleo, datos = energía pres.)

Al escribir resumen (static.ols) obtenemos nuestro resultado familiar de un modelo de regresión lineal. Sin embargo, tenga en cuenta que si existe una correlación en serie en las perturbaciones, estas estimaciones de los errores estándar son incorrectas:

Llamada:

lm (fórmula = Energía ~ rmn1173 + grf0175 + grf575 + jec477 +

  jec1177 +

    jec479 + embargo + rehenes + oilc + Aprobación +

      Desempleo

    datos = pres.energy)

Derechos residuales de autor:

     Mín. 1T Mediana 3T Máx.

-104,995 -12,921 -3,448 8,973 111,744

Coeficientes:

            Estimar Std. Valor t de error Pr (> | t |)

(Intercepción) 319.7442 46.8358 6.827 1.51e-10 ***

rmn1173 78,8261 28,8012 2,737 0,00687 **

grf0175 60.7905 26.7006 2.277 0.02406 *

grf575 -4,2676 26,5315 -0,161 0,87240

jec477 47.0388 26.6760 1.763 0.07966.

jec1177 15.4427 26.3786 0.585 0.55905

jec479 72.0519 26.5027 2.719 0.00724 **

embargo 96.3760 13.3105 7.241 1.53e-11 ***

rehenes -4,5289 7,3945 -0,612 0,54106

aceitec -5.8765 1.0848 -5.417 2.07e-07 ***

Aprobación -1.0693 0.2147 -4.980 1.57e-06 ***

Desempleo -3.7018 1.3861 -2.671 0.00831 **

---

Signif. códigos: 0 *** 0,001 ** 0,01 * 0,05. 0,1 1

Error estándar residual: 26,26 en 168 grados de

 libertad

R cuadrado múltiple: 0,5923, R cuadrado ajustado:

 0.5656

Estadístico F: 22,19 en 11 y 168 DF, valor p:

 <2.2e-16

Sin embargo, necesitamos diagnosticar si la correlación serial está presente en los residuos antes de estar contentos con los resultados. Para hacer esto, necesitamos cargar el paquete lmtest (presentado por primera vez en el Capítulo  6 ) para que algunos diagnósticos estén disponibles. Al cargar este paquete, podemos calcular una prueba de Durbin-Watson o Breusch-Godfrey para la autocorrelación:  

biblioteca (lmtest)

dwtest (static.ols)

bgtest (static.ols)

Para los comandos dwtest y bgtest , simplemente proporcionamos el nombre del modelo como argumento principal. El D urbin- W atson prueba (calculado con dwtest ) prueba de correlación serial de primer orden y no es válido para un modelo que incluye una variable dependiente rezagada. Nuestra prueba produce el siguiente resultado:

        Prueba de Durbin-Watson

datos: static.ols

DW = 1,1649, valor p = 1,313e-09

hipótesis alternativa: la verdadera autocorrelación es mayor

   de 0

El estadístico d de Durbin-Watson (1.1649 en este caso), no tiene una distribución paramétrica. Tradicionalmente, el valor de d se ha comparado con tablas basadas en los resultados de Monte Carlo para determinar la importancia. Sin embargo, R proporciona un valor p aproximado con el estadístico. Para una prueba de Durbin-Watson, la hipótesis nula es que no hay autocorrelación, por lo que nuestro valor significativo de d sugiere que la autocorrelación es un problema.

Mientras tanto, los resultados de nuestro B reusch- G odfrey prueba (calculadas con bgtest ) ofrecer una conclusión similar. La prueba de Breusch-Godfrey tiene una distribución χ 2 y se puede utilizar para probar la autocorrelación en un modelo con una variable dependiente rezagada. De forma predeterminada, el comando bgtest comprueba la correlación en serie de primer orden, aunque la correlación en serie de orden superior se puede probar con la opción de orden . Nuestro resultado en este caso es:

Prueba de Breusch-Godfrey para la correlación serial de

   ordenar hasta

1

datos: static.ols

Prueba LM = 38.6394, gl = 1, valor p = 5.098e-10

Nuevamente, la hipótesis nula es que no hay autocorrelación, por lo que nuestro valor significativo de χ 2 muestra que la correlación serial es una preocupación, y debemos hacer algo para explicar esto.

En este punto, podemos sacar una de dos conclusiones: la primera posibilidad es que la especificación de nuestro modelo estático sea ​​correcta, y necesitamos encontrar un estimador que sea eficiente en presencia de autocorrelación de errores. La segunda posibilidad es que hemos pasado por alto un efecto dinámico y necesitamos volver a especificar nuestro modelo. (En otras palabras, si hay un verdadero efecto de desbordamiento y no lo hemos modelado, los errores parecerán estar correlacionados en serie). Consideraremos cada posibilidad.

Primero, si estamos seguros de que nuestra especificación estática es correcta, entonces nuestra forma funcional es correcta, pero según el teorema de Gauss-Markov, OLS es ineficiente con autocorrelación de errores y los errores estándar están sesgados. Como alternativa, podemos utilizar mínimos cuadrados generalizados factibles (FGLS), que estima el nivel de correlación del error y lo incorpora al estimador. Aquí hay una variedad de técnicas de estimación, incluidos los estimadores de Prais-Winsten y Cochrane-Orcutt. Procedemos ilustrando el estimador Cochrane-Orcutt, aunque los usuarios deben tener cuidado con los supuestos del modelo. 9 En resumen, Cochrane - Orcuttvuelve a estimar el modelo varias veces, actualizando la estimación de la autocorrelación del error cada vez, hasta que converge a una estimación estable de la correlación. Para implementar este procedimiento en R , necesitamos instalar y luego cargar el paquete orcutt :

install.packages ("orcutt")

biblioteca (orcutt)

cochrane.orcutt (static.ols)

Una vez que hemos cargado este paquete, insertamos el nombre de un modelo lineal que hemos estimado con OLS en el comando cochrane.orcutt . Esto luego vuelve a estimar iterativamente el modelo y produce nuestros resultados FGLS de la siguiente manera:

$ Cochrane.Orcutt

Llamada:

lm (fórmula = YB ~ XB - 1)

Derechos residuales de autor:

    Mín. 1T Mediana 3T Máx.

-58.404 -9.352 -3.658 8.451 100.524

Coeficientes:

              Estimar Std. Valor t de error Pr (> | t |)

XB (intercepción) 16.8306 55.2297 0.305 0.7609

XBrmn1173 91,3691 15,6119 5,853 2,5e-08 ***

XBgrf0175 32.2003 16.0153 2.011 0.0460 *

XBgrf575 -7,9916 15,7288 -0,508 0,6121

XBjec477 29.6881 15.6159 1.901 0.0590.

XBjec1177 -6,4608 15,6174 -0,414 0,6796

XBjec479 -20.0677 15.6705 -1.281 0.2021

XBembargo 34.5797 15.0877 2.292 0.0232 *

XBhostages -16.9183 14.1135 -1.199 0.2323

XBoilc 0,8240 1,0328 0,798 0,4261

Aprobación XBA -0,2399 0,2742 -0,875 0,3829

XB Desempleo -0,1332 4,3786 -0,030 0,9758

---

Signif. códigos: 0 *** 0,001 ** 0,01 * 0,05. 0,1 1

Error estándar residual: 20,19 en 167 grados de

 libertad

R cuadrado múltiple: 0,2966, R cuadrado ajustado:

   0.2461

Estadístico F: 5,87 en 12 y 167 DF, valor de p:

 1.858e-08

$ rho

[1] 0,8247688

$ number.interaction

[1] 15

La primera parte de la tabla se parece a la salida de regresión lineal familiar (aunque las letras XB aparecen antes del nombre de cada predictor). Todos estos coeficientes, errores estándar y estadísticas inferenciales tienen exactamente la misma interpretación que en un modelo estimado con MCO, pero nuestras estimaciones ahora deberían ser eficientes porque se calcularon con FGLS. Cerca de la parte inferior de la salida, vemos $ rho , que nos muestra nuestra estimación final de autocorrelación de errores. También vemos $ number.interaction , que nos informa que el modelo se volvió a estimar en 15 iteraciones antes de que convergiera al resultado final. FGLS está destinado a producir estimaciones eficientes si una especificación estática es correcta.

Por el contrario, si creemos que una especificación dinámica es correcta, debemos trabajar para volver a especificar nuestro modelo lineal para capturar esa forma funcional. De hecho, si nos equivocamos en la forma funcional, nuestros resultados están sesgados, por lo que hacerlo bien es fundamental. Agregar una especificación de retraso a nuestro modelo puede ser considerablemente más fácil si instalamos y cargamos el paquete dyn . Nombramos nuestro modelo koyck.ols por razones que serán evidentes en breve:

install.packages ("dyn")

biblioteca (dyn)

pres.energy <-ts (pres.energy)

koyck.ols <-dyn $ lm (Energía ~ retraso (Energía, -1) + rmn1173 +

     grf0175 + grf575 + jec477 + jec1177 + jec479 + embargo +

     rehenes + petróleoc + Aprobación + Desempleo, datos = energía pres.)

Después de cargar dyn , la segunda línea usa el comando ts para declarar que nuestros datos son datos de series de tiempo. En la tercera línea, observe que cambiamos el comando del modelo lineal para que lea, dyn $ lm . Esta modificación nos permite incluir variables rezagadas dentro de nuestro modelo. En particular, ahora hemos agregado rezago (Energía, -1) , que es el valor rezagado de nuestra variable dependiente. Con el comando lag , especificamos la variable que se retrasa y cuántas veces se retrasa. Al especificar -1 , buscamos el valor inmediatamente anterior. (Los valores positivos representan valores futuros). El retraso predeterminado es 0, que solo devuelve valores actuales.

Podemos ver los resultados de este modelo escribiendo resumen (koyck.ols) :

Llamada:

lm (fórmula = dyn (Energía ~ retraso (Energía, -1) + rmn1173 +

   grf0175 +

   grf575 + jec477 + jec1177 + jec479 + embargo +

     rehenes +

    petróleoc + Aprobación + Desempleo), datos = energía pres.)

Derechos residuales de autor:

    Mín. 1T Mediana 3T Máx.

-51.282 -8.638 -1.825 7.085 70.472

Coeficientes:

                 Estimar Std. Valor t de error Pr (> | t |)

(Intercepción) 62.11485 36.96818 1.680 0.09479.

rezago (Energía, -1) 0,73923 0,05113 14,458 <2e-16 ***

rmn1173 171.62701 20.14847 8.518 9.39e-15 ***

grf0175 51.70224 17.72677 2.917 0.00403 **

grf575 7.05534 17.61928 0.400 0.68935

jec477 39.01949 17.70976 2.203 0.02895 *

jec1177 -10.78300 17.59184 -0.613 0.54075

jec479 28.68463 17.83063 1.609 0.10958

embargo 10.54061 10.61288 0.993 0.32206

rehenes -2.51412 4.91156 -0.512 0.60942

aceitec -1.14171 0.81415 -1.402 0.16268

Aprobación -0,15438 0,15566 -0,992 0,32278

Desempleo -0,88655 0,96781 -0,916 0,36098

---

Signif. códigos: 0 *** 0,001 ** 0,01 * 0,05. 0,1 1

Error estándar residual: 17,42 en 166 grados de

 libertad

  (2 observaciones eliminadas por falta de información)

R cuadrado múltiple: 0,822, R cuadrado ajustado:

 0,8092

Estadístico F: 63,89 en 12 y 166 DF, valor de p:

 <2.2e-16

Nuestra especificación aquí a menudo se denomina modelo Koyck. Esto se debe a que Koyck (1954) observó que cuando se incluye una variable dependiente rezagada como predictor, cada predictor tendrá efectos de desbordamiento en los meses siguientes.

Considere dos ejemplos de efectos de desbordamiento de predictores. Primero, nuestro coeficiente para el habla de Nixon es aproximadamente 172. Aquí, estamos interesados ​​en un efecto de impulso por el cual el predictor aumentó a 1 en el mes en que se dio el habla, y luego volvió a 0. Por lo tanto, en el mes de noviembre de 1973 cuando se pronunció el discurso, el efecto esperado de este discurso manteniendo todo lo demás igual es un aumento de 172 historias en la cobertura de la política energética. Sin embargo, en diciembre de 1973, el nivel de cobertura de noviembre es un predictor, y la cobertura de noviembre fue moldeada por el discurso. Dado que nuestro coeficiente en la variable dependiente rezagada es aproximadamente 0,74, y desde 0. 74 × 172 ≈ 128, por lo tanto, esperamos que el habla aumente la cobertura de energía en diciembre en 128, ceteris paribus. Sin embargo, el efecto también persistiría en enero porque el valor de diciembre predice el valor de enero de 1974. Desde 0. 74 × 0. 74 × 172 ≈ 94, esperamos que el efecto del discurso de Nixon sea un aumento de 94 pisos en la cobertura de energía en enero, ceteris paribus . Este tipo de efecto dinámico decadente persiste para un efecto de impulso en cualquiera de estas variables, por lo que esta es una forma poderosa de especificar un modelo dinámico. Además, tenga en cuenta que un investigador podría trazar fácilmente estos efectos de la intervención en descomposición a lo largo del tiempo para visualizar el impacto dinámico de una intervención. Este gráfico es válido para un modelo de Koyck y se parecería al resultado de la figura  9.3 .

En segundo lugar, considere el efecto del desempleo. No deberíamos darle mucha importancia al impacto de este predictor porque el coeficiente no es discernible, pero sirve como ejemplo de interpretación del efecto dinámico de un predictor continuo. Si estamos interesados ​​en un efecto escalón , nos gustaría saber cuál sería el impacto a largo plazo si un predictor aumentara en una sola unidad y se mantuviera en ese nivel más alto. Entonces, aunque no se puede discernir estadísticamente, el coeficiente de desempleo es - 0. 89, lo que significa que un aumento de un punto porcentual en el desempleo disminuye la atención de las noticias a la política energética en casi nueve décimas partes de una historia, en el mismo mes, en promedio, y ceteris paribus . Pero si el desempleo se mantuvo un punto porcentual más alto, ¿cómo cambiaría la cobertura a largo plazo? Si β13 es el coeficiente de desempleo y β 2 es el coeficiente de la variable dependiente rezagada, entonces el efecto a largo plazo se calcula mediante (Keele y Kelly 2006, pag. 189):
β131 -β2
(9,2)
Podemos calcular esto en R  simplemente haciendo referencia a nuestros coeficientes:
koyck.ols $ coeficientes [13] / (1-koyck.ols $ coeficientes [2])

Nuestra producción es - 3. 399746, lo que significa que un aumento persistente de 1% en el desempleo reduciría la cobertura de noticias de televisión sobre política energética a largo plazo en 3.4 historias en promedio y todo lo demás igual. Nuevamente, este tipo de efecto a largo plazo podría ocurrir para cualquier variable que no se limite a una entrada de pulso.

Como estrategia final, podríamos incluir uno o más rezagos de uno o más predictores sin incluir una variable dependiente rezagada. En este caso, cualquier derrame se limitará a lo que incorporemos directamente en la especificación del modelo. Por ejemplo, si solo quisiéramos un efecto dinámico del habla de Nixon y una especificación estática para todo lo demás, podríamos especificar este modelo:

udl.mod <-dyn $ lm (Energía ~ rmn1173 + retardo (rmn1173, -1) +

     retraso (rmn1173, -2) + retraso (rmn1173, -3) + retraso (rmn1173, -4) +

     grf0175 + grf575 + jec477 + jec1177 + jec479 + embargo +

     rehenes + petróleoc + Aprobación + Desempleo, datos = energía pres.)

En esta situación, hemos incluido el valor actual del indicador de habla de Nixon, así como cuatro rezagos. Para una intervención, eso significa que este predictor tendrá efecto en noviembre de 1973 y durante 4 meses después. (En abril de 1974, sin embargo, el efecto cae abruptamente a 0, donde permanece). Vemos los resultados de este modelo escribiendo resumen (udl.mod) :

Llamada:

lm (fórmula = dyn (Energía ~ rmn1173 + retraso (rmn1173, -1) + retraso

    (rmn1173, -2) + retraso (rmn1173, -3) + retraso (rmn1173, -4)

    + grf0175 + grf575 + jec477 + jec1177 + jec479 + embargo

    + rehenes + petróleoc + Aprobación + Desempleo), datos = energía pres.)

Derechos residuales de autor:

    Mín. 1T Mediana 3T Máx.

-43,654 -13,236 -2,931 7,033 111,035

Coeficientes:

                 Estimar Std. Valor t de error Pr (> | t |)

(Intercepción) 334.9988 44.2887 7.564 2.89e-12 ***

rmn1173 184.3602 34.1463 5.399 2.38e-07 ***

retraso (rmn1173, -1) 181.1571 34.1308 5.308 3.65e-07 ***

retraso (rmn1173, -2) 154.0519 34.2151 4.502 1.29e-05 ***

retraso (rmn1173, -3) 115.6949 34.1447 3.388 0.000885 ***

retraso (rmn1173, -4) 75.1312 34.1391 2.201 0.029187 *

grf0175 60.5376 24.5440 2.466 0.014699 *

grf575 -3.4512 24.3845 -0.142 0.887629

jec477 45.5446 24.5256 1.857 0.065146.

jec1177 14.5728 24.2440 0.601 0.548633

jec479 71.0933 24.3605 2.918 0.004026 **

embargo -9.7692 24.7696 -0.394 0.693808

rehenes -4,8323 6,8007 -0,711 0,478392

aceitec -6.1930 1.0232 -6.053 9.78e-09 ***

Aprobación -1.0341 0.1983 -5.216 5.58e-07 ***

Desempleo -4,4445 1,3326 -3,335 0,001060 **

---

Signif. códigos: 0 *** 0,001 ** 0,01 * 0,05. 0,1 1

Error estándar residual: 24,13 en 160 grados de libertad

  (8 observaciones eliminadas por falta de información)

R-cuadrado múltiple: 0,6683, R-cuadrado ajustado: 0,6372

Estadístico F: 21,49 en 15 y 160 DF, valor de p: <2,2e-16

Como era de esperar, el efecto se reduce cada mes después del inicio. Para este modelo de rezago distribuido sin restricciones, tenemos que estimar varios parámetros más de los que requiere el modelo de Koyck, pero en algunos casos puede tener sentido teóricamente.

9.3 Autorregresión vectorial
El enfoque final que describiremos en este capítulo es la autorregresión vectorial (VAR). El enfoque VAR es útil cuando se estudian varias variables que son endógenas entre sí porque existe una causalidad recíproca entre ellas. El marco básico es estimar un modelo de regresión lineal para cada una de las variables endógenas. En cada modelo lineal, incluir varios valores retardados de la variable de resultado en sí (digamos p rezagos de la variable), así como p rezagos de todas las otras variables endógenas. Así que para el caso simple de dos variables endógenas, X e Y , en los que nos planteamos nuestra longitud del rezago que p  = 3, queremos estimar dos ecuaciones que pueden ser representados de la siguiente manera:
ytXt==β0+β1yt - 1+β2yt - 2+β3yt - 3+β4Xt - 1+β5Xt - 2+β6Xt - 3+ϵtγ0+γ1Xt - 1+γ2Xt - 2+γ3Xt - 3+γ4yt - 1+γ5yt - 2+γ6yt - 3+δt
(9,3)
Un tratamiento más completo de esta metodología, incluida la notación para modelos con variables más endógenas y ejemplos de modelos que también incluyen variables exógenas, se puede encontrar en Brandt y Williams (2007). Con este modelo, se pueden utilizar pruebas como las de causalidad de Granger para evaluar si existe un efecto causal de una variable endógena a otra (Granger 1969).
Como ejemplo de cómo implementar un modelo VAR en R , pasamos al trabajo de Brandt y Freeman (2006), quienes analizan datos semanales sobre el conflicto israelo-palestino. Sus datos se extraen del Kansas Event Data System, que codifica automáticamente los informes de noticias en inglés para medir los eventos políticos, con el objetivo de utilizar esta información como una advertencia temprana para predecir cambios políticos. Las variables endógenas en estos datos son acciones políticas escaladas tomadas por EE. UU., Israel o Palestina, y dirigidas a uno de los otros actores. Esto produce seis variables a2i , a2p , i2a , p2a , i2p y p2i . Las abreviaturas son "a" para estadounidense, "i" para israelí y "p" para palestino. Como ejemplo, i2pmide el valor escalado de las acciones israelíes dirigidas a los palestinos. Los datos semanales que usaremos van desde el 15 de abril de 1979 al 26 de octubre de 2003, por un total de 1278 semanas.

Para continuar, necesitamos instalar y cargar el paquete vars para que los comandos de estimación relevantes estén disponibles. También necesitamos el paquete externo porque nuestros datos están en formato Stata: 10

install.packages ("vars")

biblioteca (vars)

biblioteca (extranjera)

levant.0 <- read.dta ("levant.dta")

levant <- subconjunto (levant.0,

     select = c ("a2i", "a2p", "i2p", "i2a", "p2i", "p2a"))

Después de cargar los paquetes, cargamos nuestros datos en la tercera línea, nombrándola levant.0 . Estos datos también contienen tres índices relacionados con la fecha, por lo que, para fines de análisis, en realidad necesitamos crear una segunda copia de los datos que solo incluya nuestras seis variables endógenas sin los índices. Hacemos esto usando el comando subconjunto para crear el conjunto de datos llamado levant .

Un paso clave en este punto es elegir la longitud de retraso adecuada, p . La longitud del retraso debe capturar todos los procesos de error y la dinámica causal. Un enfoque para determinar la longitud de retardo apropiada es ajustar varios modelos, elegir el modelo con el mejor ajuste y luego ver si los residuos muestran alguna evidencia de correlación serial. El comando VARselect calcula automáticamente varias v ector un uto r modelos egression y seleccione s que se retrasan longitud tiene el mejor ajuste. Dado que nuestros datos son semanales, consideramos retrasos de hasta 104 semanas, o datos de 2 años, para considerar todas las opciones posibles. El siguiente comando puede tardar un minuto en ejecutarse porque se están estimando 104 modelos:

levant.select <-VARselect (levant, type = "const", lag.max = 104)

Para saber cuál de las longitudes de retardo se ajusta mejor, podemos escribir levant.select $ selection . Nuestro resultado es simplemente el siguiente:

AIC (n) HQ (n) SC (n) FPE (n)

    60 4 1 47

Esto informa la longitud de retraso elegida para el criterio de información de Akaike, el criterio de información de Hannan-Quinn, el criterio de Schwarz y el error de predicción de pronóstico. Los cuatro índices están codificados para que los valores más bajos sean mejores. Para contrastar los extremos, el valor más bajo del criterio de Schwarz, que tiene una fuerte penalización por parámetros adicionales, es para el modelo con solo un rezago de las variables endógenas. Por el contrario, el mejor ajuste en el AIC proviene del modelo que requiere 60 rezagos, lo que quizás indique que existe una estacionalidad anual. Se puede ver una impresión mucho más larga con el valor de cada uno de los cuatro índices de ajuste para los 104 modelos escribiendo: levant.select $ criteria. Idealmente, nuestros índices de ajuste se habrían establecido en modelos con longitudes de retardo similares. Como no lo hicieron, y dado que tenemos 1278 observaciones, tomaremos la ruta más segura con el largo retraso sugerido por la AIC. 11

Para estimar la v ector un uto r modelo egression con p  = 60 retardos de cada variable endógena, escribimos:

levant.AIC <-VAR (levant, tipo = "const", p = 60)

El comando VAR requiere el nombre de un conjunto de datos que contiene todas las variables endógenas. Con la opción de tipo , hemos elegido "const" en este caso (el predeterminado). Esto significa que cada uno de nuestros modelos lineales incluye una constante. (Otras opciones incluyen especificar una "tendencia", "tanto" una constante como una tendencia, o "ninguna" que no incluye ninguna.) Con la opción p elegimos la longitud del rezago. Una opción que no usamos aquí es la opción exógena , que nos permite especificar variables exógenas.

Una vez que hemos estimado nuestro modelo usando VAR , lo siguiente que debemos hacer es diagnosticar el modelo usando una llamada especial a la función plot :

parcela (levant.AIC, lag.acf = 104, lag.pacf = 104)

Al utilizar el nombre de nuestro modelo VAR, levant.AIC , como único argumento en la gráfica , R  proporcionará automáticamente una gráfica de diagnóstico para cada una de las variables endógenas. Con las opciones de lag.acf y lag.pacf , especificamos que las gráficas de ACF y PACF que los informes de este comando deben mostrarnos el valor de 2 años (104 semanas) de patrones de autocorrelación. Para nuestros datos, R  produce seis gráficos. R  muestra estos gráficos uno a la vez, y entre cada uno, la consola presentará el siguiente mensaje:

Presione <Return> para ver la siguiente trama:

R  mantendrá un gráfico en el visor hasta que presione la tecla Retorno , momento en el que pasa al siguiente gráfico. Este proceso se repite hasta que se muestra el gráfico de cada resultado.

Alternativamente, si quisiéramos ver la gráfica de diagnóstico para una variable endógena en particular, podríamos escribir:

plot (levant.AIC, lag.acf = 104, lag.pacf = 104, names = "i2p")

Aquí, la opción de nombres nos ha permitido especificar que queremos ver los diagnósticos de i2p (acciones israelíes dirigidas hacia Palestina). La gráfica de diagnóstico resultante se muestra en la figura 9.5 . . El gráfico tiene cuatro partes: En la parte superior hay un gráfico de líneas que muestra los valores reales en una línea negra sólida y los valores ajustados en una línea discontinua azul. Directamente debajo de esto hay un gráfico lineal de los residuos contra una línea en cero. Estos dos primeros gráficos pueden ilustrar si el modelo hace predicciones insesgadas del resultado de manera consistente y si los residuos son homocedásticos a lo largo del tiempo. En general, los pronósticos se sitúan constantemente alrededor de cero, aunque en las últimas 200 observaciones la varianza del error parece aumentar ligeramente. El tercer gráfico, en la parte inferior izquierda, es el ACF para los residuos en i2p . 12 Por último, en la parte inferior derecha del panel, vemos el PACF para i2pResiduos de. Ningún pico es significativo en el ACF y solo un pico es significativo en el PACF durante 2 años, por lo que concluimos que nuestra estructura de rezagos ha filtrado suficientemente cualquier correlación serial en esta variable.
Abrir imagen en nueva ventanaFigura 9.5
Figura 9.5
Valores predichos, función de autocorrelación residual y función de autocorrelación parcial residual para la serie Israel-Palestina en un modelo de autorregresión vectorial de seis variables

Al interpretar un modelo VAR, recurrimos a dos herramientas para extraer inferencias e interpretaciones de estos modelos. Primero, usamos la prueba de causalidad de Granger para determinar si una variable endógena causa las otras. Esta prueba es simplemente una prueba F de bloque para determinar si todos los rezagos de una variable pueden excluirse del modelo. Para una prueba conjunta de si una variable afecta a las otras variables en el sistema, podemos usar el comando de causalidad . Por ejemplo, si quisiéramos probar si las acciones israelíes hacia Palestina causaron acciones de las otras cinco díadas dirigidas, escribiríamos:

causalidad (levant.AIC, cause = "i2p") $ Granger

El comando aquí solicita el nombre del modelo primero ( levant.AIC ), y luego con la opción de causa , especificamos cuál de las variables endógenas deseamos probar el efecto. Nuestro resultado es el siguiente:

Causalidad Granger H0: i2p no causa Granger

   a2i a2p i2a

p2i p2a

datos: VAR objeto levant.AIC

Prueba F = 2.1669, df1 = 300, df2 = 5142, valor p

 <2.2e-16

La hipótesis nula es que los coeficientes para todos los rezagos de i2p son cero cuando se modela cada una de las otras cinco variables de resultado. Sin embargo, nuestra prueba F es significativa aquí, como muestra el minúsculo valor p . Por lo tanto, concluiríamos que las acciones israelíes dirigidas hacia Palestina tienen un efecto causal sobre las otras variables de acción política en el sistema.

Podemos proceder a probar si cada una de las otras cinco variables de Granger causan los otros predictores en el sistema considerando cada una con el comando de causalidad :

causalidad (levant.AIC, cause = "a2i") $ Granger

causalidad (levant.AIC, cause = "a2p") $ Granger

causalidad (levant.AIC, cause = "i2a") $ Granger

causalidad (levant.AIC, cause = "p2i") $ Granger

causalidad (levant.AIC, cause = "p2a") $ Granger

Los resultados no se reproducen aquí para preservar el espacio. Sin embargo, en el nivel de confianza del 95%, verá que cada variable causa significativamente a las demás, a excepción de las acciones estadounidenses dirigidas hacia Israel ( a2i ).

Finalmente, para tener una idea del impacto sustantivo de cada predictor, recurrimos al análisis de respuesta al impulso. La lógica aquí es algo similar al análisis de intervención que graficamos en la figura  9.3 . Una función de respuesta al impulso considera un aumento de una unidad en una de las variables endógenas y calcula cómo tal choque exógeno influiría dinámicamente en todas las variables endógenas, dados los patrones de autorregresión y dependencia.

Al interpretar una función de impulso-respuesta, una consideración clave es el hecho de que los choques a una variable endógena casi siempre están correlacionados con los choques a otras variables. Por lo tanto, debemos considerar que es probable que un choque de una unidad en el término residual de una variable cree un movimiento contemporáneo en los residuos de las otras variables. Los términos fuera de la diagonal de la matriz de varianza-covarianza de los residuos de las variables endógenas,  Σ^ , nos dice cuánto covarían los choques.

Hay varias formas de abordar este problema. Uno es usar la teoría para determinar el orden de las variables endógenas. En este enfoque, el investigador asume que un choque en una variable endógena no se ve afectado por choques en ninguna otra variable. Entonces, los choques de una segunda variable se ven afectados solo por los de la primera variable y no por los demás. El investigador repite de forma recursiva este proceso para identificar un sistema en el que todas las variables se pueden ordenar en una cadena causal. Con un sistema diseñado teóricamente como este, el investigador puede determinar cómo los choques contemporáneos se afectan entre sí con una descomposición estructurada de Cholesky de  Σ^  (Enders 2009, pag. 309). Una segunda opción, que es la opción predeterminada en el comando irf de R , es asumir que no hay conocimiento teórico del orden causal de los choques contemporáneos y aplicar el método de ortogonalización de los residuos . Esto implica otra descomposición de Cholesky, en la que encontramos  A- 10  resolviendo   A- 1 ′0A0=Σ^ . Para obtener más detalles sobre el orden de respuesta o la ortogonalización de residuos, consulte Brandt y Williams (2007, págs. 36-41 y 66-70), Enders (2009, págs. 307-315) o Hamilton (1994, págs. 318-324).

Como ejemplo de una función de respuesta al impulso, graficaremos el efecto de un evento político adicional de Israel dirigido hacia Palestina. R  calculará nuestra i mpulse r espuesta f unción con el IRF del sistema, utilizando la ortogonalización por defecto de los residuos. Una de las opciones clave de este comando es el arranque , que determina si se debe construir un intervalo de confianza con correas de arranque . Generalmente, es aconsejable informar la incertidumbre en las predicciones, pero el proceso puede tardar varios minutos. 13 Entonces, si el lector desea un resultado rápido, configure boot = FALSE . Para obtener el resultado con intervalos de confianza, escriba:

levant.irf <-irf (levant.AIC, impulso = "i2p", n.ahead = 12, boot = TRUE)

Llamamos a nuestra función de respuesta al impulso levant.irf . El comando requiere que digamos el nombre de nuestro modelo ( levant.AIC ). La opción impulso nos pide que nombremos la variable de la que queremos el efecto, la opción n. Anticipada establece con qué anticipación deseamos pronosticar (decimos 12 semanas o 3 meses), y finalmente boot determina si crear intervalos de confianza basados ​​en una muestra de bootstrap.

Una vez que hayamos calculado esto, podemos graficar la función de respuesta al impulso con una llamada especial para graficar :

parcela (levant.irf)

El gráfico resultante se muestra en la figura  9.6 . Hay seis paneles, uno para cada una de las seis variables endógenas. El eje vertical de cada panel enumera el nombre de la variable endógena y representa el cambio esperado en esa variable. El eje horizontal de cada panel representa el número de meses que han transcurrido desde el choque. Cada panel muestra una línea roja sólida en cero, que representa dónde cae un no efecto en el gráfico. La línea negra continua representa el efecto esperado en cada mes y las líneas discontinuas rojas representan cada intervalo de confianza. Como muestra la figura, el impacto real de una conmoción en las acciones de Israel a Palestina es dramático para la propia serie Israel a Palestina ( i2p) debido a la autorregresión y la retroalimentación de los efectos a otras series. También vemos un salto significativo en las acciones de Palestina a Israel ( p2i ) durante los siguientes 3 meses. Con las otras cuatro series, los efectos palidecen en comparación. Fácilmente podríamos producir gráficos similares a la figura  9.6 calculando la función de respuesta al impulso para un choque en cada una de las seis entradas. En general, esta es una buena idea, pero se omite aquí por espacio.
Abrir imagen en nueva ventanaFigura 9.6
Figura 9.6
Función de respuesta al impulso para un choque de una unidad en la serie Israel-Palestina en un modelo de autorregresión vectorial de seis variables

9.4 Lecturas adicionales sobre el análisis de series de tiempo
Con estas herramientas en la mano, el lector debe tener alguna idea de cómo estimar e interpretar modelos de series de tiempo en R  usando tres enfoques: modelado de Box-Jenkins, modelado econométrico y autorregresión vectorial. Tenga en cuenta que, si bien este capítulo utiliza ejemplos simples, el análisis de series de tiempo generalmente es un desafío. Puede ser difícil encontrar un buen modelo que represente adecuadamente todos los procesos de tendencia y error, pero el analista debe trabajar con cuidado en todos estos problemas o las inferencias estarán sesgadas. (Vean de nuevo, Granger y Newbold 1974.) Por lo tanto, asegúrese de reconocer que a menudo se necesitan varios intentos para encontrar un modelo que tenga en cuenta todos los problemas.

También vale la pena tener en cuenta que este capítulo no puede abordar de manera integral ninguno de los tres enfoques que consideramos, y mucho menos abordar todos los tipos de análisis de series de tiempo. (El análisis espectral, el análisis de ondículas, los modelos de espacio de estados y los modelos de corrección de errores son solo algunos de los temas que no se tratan aquí). Por lo tanto, se recomienda al lector interesado que consulte otros recursos para obtener más detalles importantes sobre varios temas en el modelado de series de tiempo. Buenos libros que cubren una variedad de temas de series de tiempo e incluyen  código R son: Cowpertwait y Metcalfe (2009), Cryer y Chan (2008) y Shumway y Stoffer (2006). Para obtener más profundidad sobre la teoría detrás de las series de tiempo, los buenos volúmenes de científicos políticos incluyen Box-Steffensmeier et al. (2014) y Brandt y Williams (2007). Otros buenos libros que cubren la teoría de series de tiempo incluyen: Box et al. 2008, Enders (2009), Wei (2006), Lütkepohl (2005), y para una versión avanzada, consulte Hamilton (1994). Varios libros se centran en temas específicos y en la aplicación de los métodos en R : por ejemplo, Petris et al. (2009) cubre modelos lineales dinámicos, también llamados modelos de espacio de estado, y explica cómo usar el paquete dlm correspondiente en R  para aplicar estos métodos. Pfaff (2008) analiza los modelos de datos cointegrados, como los modelos de corrección de errores y los modelos de corrección de errores vectoriales, utilizando el  paquete R vars . Además, los lectores interesados ​​en modelos de sección transversal de series de tiempo, o datos de panel, deben consultar el paquete plm , que facilita la estimación de modelos apropiados para esos métodos. Mientras tanto, Mátyás y Sevestre (2008) ofrece una base teórica sobre métodos de datos de panel.

9.5 Código de serie temporal alternativo
Como se menciona en la Secta. 9.1 , ahora mostraremos una sintaxis alternativa para producir las Figs. 9.3 y  9.4 . Este código es un poco más largo, pero es más generalizable. 14 En primer lugar, si no tiene todos los paquetes, objetos de datos y modelos cargados de antes, asegúrese de volver a cargar algunos de ellos para que podamos dibujar la figura:

biblioteca (TSA)

pres.energy <-read.csv ("PESenergy.csv")

predictores <-as.matrix (subconjunto (pres.energy, select = c (rmn1173,

     grf0175, grf575, jec477, jec1177, jec479, embargo, rehenes,

     oilc, Aprobación, Desempleo)))

Las tres líneas de código anteriores se ejecutaron anteriormente en el capítulo. A modo de recordatorio, la primera línea carga el paquete TSA , la segunda carga los datos de cobertura de nuestra política energética y la tercera crea una matriz de predictores.

Ahora, para volver a dibujar cualquiera de las figuras, debemos dedicarnos un poco a la gestión de datos:

meses <- 1: 180

static.predictors <- predictores [, - 1]

predictores.dinámicos <- predictores [, 1, drop = FALSE]

y <- ts (energía pres. $ Energía, frecuencia = 12, inicio = c (1972, 1))

Primero, definimos un índice mensual, como antes. En segundo lugar, subconjuntamos nuestros predictores matriciales con aquellos que tienen un efecto estático. En tercer lugar, aislamos nuestro predictor dinámico del discurso de Nixon. En cuarto lugar, utilizamos el comando ts para declarar la cobertura de la política energética como un objeto de serie temporal. En esta última línea, usamos la opción de frecuencia para especificar que estos son datos mensuales (de ahí el valor 12) y la opción de inicio para notar que estos datos comienzan en el primer mes de 1972.

A continuación, necesitamos estimar realmente nuestra función de transferencia. Una vez hecho esto, podemos guardar varias salidas del modelo:

dynamic.mod <-arimax (y, order = c (1,0,0), xreg = static.predictors,

     xtransf = predictores dinámicos, transferencia = lista (c (1,0)))

b <- coef (mod dinámico)

static.coefs <- b [match (colnames (static.predictors), names (b))]

ma.coefs <- b [grep ("MA0", nombres (b))]

ar.coefs <- b [grep ("AR1", nombres (b))]

La primera línea reajusta nuestra función de transferencia. El segundo usa el comando coef para extraer los coeficientes del modelo y guardarlos en un vector llamado b . Las últimas tres líneas separan nuestros coeficientes en efectos estáticos ( static.coefs ), efectos dinámicos iniciales ( ma.coefs ) y términos de decaimiento ( ar.coefs ). En cada línea, hacemos referencia cuidadosamente a los nombres de nuestro vector de coeficientes, usando el comando match para encontrar coeficientes para los predictores estáticos, y luego el comando grep para buscar términos que contengan MA0 y AR1 , respectivamente, como términos de salida de una transferencia función hacer.

Con todos estos elementos extraídos, ahora pasamos específicamente a volver a dibujar la Fig.  9.3 , que muestra el efecto de la intervención del habla de Nixon contra los datos reales. Nuestro efecto de intervención consta de dos partes, el valor esperado de mantener todos los predictores estáticos en sus valores para el mes 58 y el efecto dinámico de la función de transferencia. Creamos esto de la siguiente manera:

xreg.pred <-b ["interceptar"] + static.coefs% *% static.predictors [58,]

transf.pred <- as.numeric (predictores.dinámicos% *% ma.coefs +

     ma.coefs * (ar.coefs ^ (meses-59)) * (meses> 59))

y.pred <-ts (xreg.pred + transf.pred, frecuencia = 12, inicio = c (1972,1))

La primera línea simplemente hace la predicción estática a partir de una ecuación lineal. El segundo usa nuestros efectos iniciales y términos de decaimiento para predecir el efecto dinámico de la intervención. En tercer lugar, sumamos las dos piezas y las guardamos como una serie de tiempo con la misma frecuencia y fecha de inicio que la serie original. Con tanto y como y.pred ahora codificados como series de tiempo de la misma frecuencia durante el mismo lapso de tiempo, ahora es fácil recrear la Fig.  9.3 :

plot (y, xlab = "Month", ylab = "Energy Policy Stories", type = "l")

líneas (y.pred, lty = 2, col = 'blue', lwd = 2)

La primera línea simplemente traza la serie de tiempo original y la segunda línea agrega el efecto de intervención en sí.

Con todo el trabajo de configuración que hemos realizado, reproducir la Fig.  9.4 ahora solo requiere tres líneas de código:

full.pred <-ajustado (dynamic.mod)

plot (full.pred, ylab = "Historias de política energética", type = "l",

     ylim = c (0,225))

puntos (y, pch = 20)

La primera línea simplemente usa el comando ajustado para extraer valores ajustados del modelo de función de transferencia. La segunda línea traza estos valores ajustados y la tercera agrega los puntos que representan la serie original.

9.6 Problemas de práctica
Este conjunto de problemas de práctica revisa cada uno de los tres enfoques del modelado de series de tiempo presentados en el capítulo y luego plantea una pregunta adicional sobre los datos de energía de Peake y Eshbaugh-Soha que le pide que aprenda sobre un nuevo método. Las preguntas 1 a 3 se relacionan con modelos de una sola ecuación, por lo que todas estas preguntas utilizan un conjunto de datos sobre el consumo de electricidad en Japón. Mientras tanto, la pregunta n. ° 4 utiliza datos económicos de EE. UU. Para un modelo de ecuaciones múltiples.
1.
Visualización de series de tiempo: Wakiyama et al. (2014) estudian el consumo de electricidad en Japón, evaluando si el accidente nuclear de Fukushima del 11 de marzo de 2011 afectó el consumo de electricidad en varios sectores. Lo hacen mediante la realización de un análisis de intervención sobre medidas mensuales de consumo eléctrico en megavatios (MW), de enero de 2008 a diciembre de 2012. Cargan el paquete externo y abren estos datos en formato Stata desde el archivo comprehensivoJapanEnergy.dta . Este archivo de datos está disponible en Dataverse (consulte la página vii) o en el contenido en línea de este capítulo (consulte la página 155). Nos centraremos en el consumo de electricidad de los hogares (nombre de la variable: casa). Tome el logaritmo de esta variable y trace una gráfica lineal del consumo de electricidad del hogar registrado de mes a mes. ¿Qué patrones son evidentes en estos datos?

 
2.
Modelado de Box – Jenkins:
un.
Trace la función de autocorrelación y la función de autocorrelación parcial para el consumo de electricidad doméstico registrado en Japón. ¿Cuáles son las características más evidentes de estas figuras?

 
B.
Wakiyama y col. (2014) argumentan que un ARIMA (1,0,1), con un componente ARIMA de temporada (1,0,0) se ajusta a esta serie. Estime este modelo e informe sus resultados. ( Sugerencia: para este modelo, querrá incluir la opción estacional = lista (orden = c (1,0,0), período = 12) en el comando arima ).

 
C.
¿Qué tan bien encaja este modelo ARIMA? ¿Cómo se ven el ACF y el PACF para los residuos de este modelo? ¿Cuál es el resultado de una prueba Q de Ljung-Box ?

 
D.
Utilice el comando arimax del paquete TSA . Estime un modelo que utiliza el proceso de error ARIMA de antes, los predictores estáticos de temperatura ( temp ) y temperatura al cuadrado ( temp2 ), y una función de transferencia para la intervención de Fukushima ( ficticia ).

 
mi.
Bono: el indicador de Fukushima es en realidad una intervención escalonada , en lugar de un pulso . Esto significa que el efecto se acumula en lugar de decaer . La nota a pie de página  8 describe cómo se acumulan estos efectos. Haga un dibujo del efecto acumulativo de la intervención de Fukushima en el consumo de electricidad de los hogares registrado.

 
 
3.
Modelado econométrico:
un.
Ajuste un modelo lineal estático utilizando OLS para el consumo de electricidad doméstico registrado en Japón. Utilice la temperatura ( temp ), la temperatura al cuadrado ( temp2 ) y el indicador de Fukushima ( ficticio ) como predictores. Cargue el paquete lmtest y calcule tanto una prueba de Durbin-Watson como una de Breusch-Godfrey para este modelo lineal. ¿Qué conclusiones sacarías de cada uno? ¿Por qué crees que obtienes este resultado?

 
B.
Vuelva a estimar el modelo lineal estático de consumo eléctrico doméstico registrado utilizando FGLS con el algoritmo Cochrane-Orcutt. ¿Qué tan similares o diferentes son sus resultados de los resultados de OLS? ¿Por qué crees que es esto?

 
C.
Cargue el paquete dyn y agregue una variable dependiente retrasada a este modelo. ¿Cuál de los tres modelos econométricos cree que es el más apropiado y por qué? ¿Cree que su modelo econométrico preferido o el análisis de intervención de Box-Jenkins es más apropiado? ¿Por qué?

 
 
4.
Autorregresión vectorial:
un.
Enders (2009, pag. 315) presenta datos trimestrales sobre la economía de Estados Unidos, que va desde el segundo trimestre de 1959 hasta el primer trimestre de 2001. Cargue los paquetes vars y extranjeros , y luego abra los datos en formato Stata desde el archivo moneyDem.dta . Este archivo está disponible en Dataverse (consulte la página vii) o en el contenido en línea de este capítulo (consulte la página 155). Haga un subconjunto de los datos para incluir solo tres variables: cambio en el PIB real registrado ( dlrgdp ), cambio en la oferta monetaria real M2 ( dlrm2 ) y cambio en la tasa de interés a 3 meses de las letras del Tesoro de EE . UU . ( Drs ). Con el comando VARselect , determine la longitud de retardo que mejor se ajuste para un modelo VAR de estas tres variables, de acuerdo con el AIC.

 
B.
Calcule el modelo que determinó que es el que mejor se ajusta según el AIC. Examine las gráficas de diagnóstico. ¿Cree que estas series están libres de correlación serial y que la forma funcional es correcta?

 
C.
Para cada una de las tres variables, pruebe si la variable Granger causa las otras dos.

 
D.
En política monetaria, la tasa de interés es una herramienta de política importante para el Banco de la Reserva Federal. Calcule una función de respuesta al impulso para un aumento de un punto porcentual en la tasa de interés ( drs ). Dibuje un gráfico de los cambios esperados en la oferta monetaria registrada ( dlrm2 ) y el PIB real registrado ( dlrgdp ). ( Sugerencia: incluya la opción response = c ("dlrgdp", "dlrm2") en la función irf .) Sea claro acerca de si está ortogonalizando los residuos o haciendo una suposición teórica sobre el orden de respuesta.

 
 
5.
Bono: es posible que haya notado que Peake y Eshbaugh-Soha (2008) los datos sobre la cobertura televisiva mensual del tema energético se utilizaron como ejemplo para la regresión del recuento en el Cap. 7 y como ejemplo de serie temporal en este capítulo. Brandt y Williams ( 2001) desarrollan un modelo autorregresivo de Poisson (PAR) para los datos de recuento de series de tiempo, y Fogarty y Monogan (2014) aplican este modelo a estos datos de política energética. Replica este modelo PAR en estos datos. Para obtener información sobre la replicación, consulte: http://hdl.handle.net/1902.1/16677 .

 
Notas al pie
1 .
Muchos usan modelos ARIMA para pronosticar valores futuros de una serie. Los modelos ARIMA en sí mismos son ateóricos, pero a menudo pueden ser efectivos para la predicción. Dado que la mayor parte del trabajo de ciencia política implica probar hipótesis motivadas teóricamente, esta sección se centra más en el papel que los modelos ARIMA pueden servir para establecer modelos inferenciales.

2 .
Si aún no tiene el archivo de datos PESenergy.csv , puede descargarlo del Dataverse (consulte la página vii) o del contenido del capítulo en línea (consulte la página 155).

3 .
Además de examinar la serie original o la función de autocorrelación, una prueba de Dickey-Fuller aumentada también sirve para diagnosticar si una serie de tiempo tiene una raíz unitaria. Cargando el URBANA paquete, el comando adf.test llevará a cabo esta prueba en R .

4 .
El principal cambio notable es que la versión predeterminada de acf grafica la correlación de retardo cero, ACF (0), que siempre es 1.0. La versión TSA elimina esto y comienza con la primera autocorrelación de retardo, ACF (1).

5 .
La fórmula para estas bandas de error es: 0 ± 1. 96 × se r . El error estándar de un coeficiente de correlación es:   smir=1 -r2n - 2----√ . Entonces, en este caso, establecemos r  = 0 bajo la hipótesis nula, y n es el tamaño de la muestra (o la longitud de la serie).

6 .
Técnicamente, el PACF en el tercer rezago es negativo y significativo, pero los patrones comunes de los procesos de error sugieren que es poco probable que esto sea una parte crítica del proceso ARIMA.

7 .
Aquí mostramos en el texto principal cómo recopilar un diagnóstico a la vez, pero el lector también puede intentar escribir tsdiag (ar1.mod, 24) para recopilar representaciones gráficas de algunos diagnósticos a la vez.

8 .
En este caso, tenemos una entrada de pulso, por lo que podemos decir que en noviembre de 1973, el efecto del discurso fue un aumento esperado de 161 noticias, manteniendo todo lo demás igual. En diciembre de 1973, el efecto de arrastre es que esperamos 98 historias más, manteniendo todo lo demás igual porque 161 × 0. 61 ≈ 98. En enero de 1974, el efecto de la intervención es que esperamos 60 historias más, ceteris paribus porque 161 × 0 61 × 0. 61 ≈ 60. El efecto de la intervención continúa hacia adelante en un patrón similar de decadencia. Por el contrario, si hubiéramos obtenido estos resultados con una intervención escalonada en lugar de un pulsointervención, entonces estos efectos se acumularían en lugar de decaer. Bajo este hipotético, los efectos serían 161 en noviembre de 1973, 259 en diciembre de 1973 (porque 161 + 98 = 259) y 319 en enero de 1974 (porque 161 + 98 + 60 = 319).

9 .
En particular, en cada etapa del proceso iterativo, el modelo lineal se estima regresando   y∗t=yt- ρyt - 1  en   X∗t=Xt- ρXt - 1  (Hamilton 1994, pag. 223). Este procedimiento asume que el proceso de ajuste dinámico es el mismo para el resultado y las variables de entrada, lo cual es poco probable. Por tanto, una especificación dinámica como un modelo de rezago distributivo autorregresivo sería más flexible.

10 .
Este ejemplo requiere el archivo levant.dta. Descargue este archivo del Dataverse (consulte la página vii) o del contenido en línea de este capítulo (consulte la página 155).

11 .
Se le anima a examinar los modelos que habrían sido elegidos por el criterio de Hannan-Quinn (4 rezagos) o el criterio de Schwarz (1 rezago) por su cuenta. ¿Cómo funcionan estos modelos en términos de diagnóstico? ¿Cómo cambiarían las inferencias?

12 .
Tenga en cuenta que, de forma predeterminada, el gráfico que  presenta R en realidad incluye la correlación perfecta de retraso cero. Si desea eliminar eso, dada nuestra gran longitud de retraso y el tamaño del panel, simplemente cargue el paquete TSA antes de dibujar el gráfico para cambiar el valor predeterminado.

13 .
Tenga en cuenta que los intervalos de confianza basados ​​en bootstrap no siempre brindan las coberturas correctas porque confunden la información sobre qué tan bien se ajusta el modelo con la incertidumbre de los parámetros. Por esta razón, los enfoques bayesianos suelen ser la mejor manera de representar la incertidumbre (Brandt y Freeman 2006; Sims y Zha 1999).

14 .
Mi agradecimiento a Dave Armstrong por escribir y sugerir este código alternativo.

Material suplementario
318886_1_En_9_MOESM1_ESM.zip (400 kb)
Dataverse (2,154 KB)
Referencias
Cuadro GEP, Tiao GC (1975) Análisis de intervenciones con aplicaciones a problemas económicos y ambientales. Asociación J Am Stat 70: 70–79
CrossRefMathSciNetzbMATHGoogle Académico
Box GEP, Jenkins GM, Reinsel GC (2008) Análisis de series de tiempo: previsión y control, 4ª ed. Wiley, Hoboken, Nueva Jersey
CrossRefzbMATHGoogle Académico
Box-Steffensmeier JM, Freeman JR, Hitt MP, Pevehouse JCW (2014) Análisis de series de tiempo para las ciencias sociales. Cambridge University Press, Nueva York
CrossRefGoogle Académico
Brandt PT, Freeman JR (2006) Avances en el modelado de series de tiempo bayesiano y el estudio de la política: pruebas teóricas, pronósticos y análisis de políticas. Polit Anal 14 (1): 1–36
CrossRefGoogle Académico
Brandt PT, Williams JT (2001) Un modelo lineal autorregresivo de Poisson: el modelo de Poisson AR (p). Polit Anal 9 (2): 164–184
CrossRefGoogle Académico
Brandt PT, Williams JT (2007) Múltiples modelos de series temporales. Sage, Thousand Oaks, CA
Google Académico
Cowpertwait PSP, Metcalfe AV (2009) Serie temporal introductoria con R. Springer, Nueva York
zbMATHGoogle Académico
Cryer JD, Chan KS (2008) Análisis de series de tiempo con aplicaciones en R, 2ª ed. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Enders W (2009) Series de tiempo econométricas aplicadas, 3ª ed. Wiley, Nueva York
Google Académico
Fogarty BJ, Monogan JE III (2014) Modelado de datos de recuento de series de tiempo: los desafíos únicos que enfrentan los estudios de comunicación política. Soc Sci Res 45: 73–88
CrossRefGoogle Académico
Granger CWJ (1969) Investigación de relaciones causales mediante modelos econométricos y métodos espectrales cruzados. Econometrica 37: 424–438
CrossRefGoogle Académico
Granger CWJ, Newbold P (1974) Regresiones espurias en econometría. J Econ 26: 1045–1066
zbMATHGoogle Académico
Hamilton JD (1994) Análisis de series de tiempo. Prensa de la Universidad de Princeton, Princeton, Nueva Jersey
zbMATHGoogle Académico
Keele L, Kelly NJ (2006) Modelos dinámicos para teorías dinámicas: los entresijos de las variables dependientes rezagadas. Polit Anal 14 (2): 186–205
CrossRefGoogle Académico
Koyck LM (1954) Rezagos distribuidos y análisis de inversiones. Holanda Septentrional, Amsterdam
Google Académico
Lütkepohl H (2005) Nueva introducción al análisis de múltiples series de tiempo. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Mátyás L, Sevestre P (eds) (2008) La econometría de los datos de panel: fundamentos y desarrollos recientes en la teoría y la práctica, 3ª ed. Springer, Nueva York
zbMATHGoogle Académico
Peake JS, Eshbaugh-Soha M (2008) El impacto en el establecimiento de la agenda de los principales discursos televisivos presidenciales. Polit Commun 25: 113-137
CrossRefGoogle Académico
Petris G, Petrone S, Campagnoli P (2009) Modelos lineales dinámicos con R. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Pfaff B (2008) Análisis de series temporales integradas y cointegradas con R, 2ª ed. Springer, Nueva York
CrossRefzbMATHGoogle Académico
Shumway RH, Stoffer DS (2006) Análisis de series de tiempo y sus aplicaciones con ejemplos de R, 2ª ed. Springer, Nueva York
zbMATHGoogle Académico
Sims CA, Zha T (1999) Bandas de error para respuestas de impulso. Econometrica 67 (5): 1113–1155
CrossRefMathSciNetzbMATHGoogle Académico
Wakiyama T, Zusman E, Monogan JE III (2014) ¿Puede sostenerse una transición energética baja en carbono en el Japón posterior a Fukushima? Evaluación de los diferentes impactos de los choques exógenos. Política energética 73: 654–666
CrossRefGoogle Académico
Wei WWS (2006) Análisis de series de tiempo: métodos univariados y multivariados, 2ª ed. Pearson, Nueva York
zbMATHGoogle Académico
