# Uso de paquetes para aplicar modelos avanzados {#Usodepaquetesparaaplicarmodelosavanzados}

<hr style="background-color:#03193b;height:2px">

Palabras clave:
- Cadena Markov Monte Carlo 
- Muestra emparejada 
- Muestra de Monte Carlo de la cadena Markov 
- Llamada de rol 
- Partido titular 

En los primeros siete capítulos de este libro, hemos tratado a R  como un programa de software estadístico tradicional y hemos revisado cómo puede realizar la gestión de datos, informar estadísticas simples y estimar una variedad de modelos de regresión. En el resto de este libro, sin embargo, nos centraremos en la flexibilidad adicional que  ofrece R , tanto en términos de capacidad de programación que está disponible para el usuario como en el suministro de herramientas aplicadas adicionales a través de paquetes . En este capítulo, nos enfocamos en cómo cargar lotes adicionales de código de paquetes escritos por el usuario puede agregar funcionalidad que muchos programas de software no permitirán. Aunque hemos utilizado paquetes para una variedad de propósitos en los siete capítulos anteriores (incluidos car , gmodels, y lattice , por nombrar algunos), aquí destacaremos los paquetes que habilitan métodos únicos. Si bien el sitio web de CRAN enumera numerosos paquetes que los usuarios pueden instalar en un momento dado, nos centraremos en cuatro paquetes particulares para ilustrar los tipos de funcionalidad que se pueden agregar.

El primer paquete que discutiremos, lme4 , permite a los usuarios estimar modelos multinivel, ofreciendo así una extensión a los modelos de regresión discutidos en los Capítulos. 6 y 7 (Bates et al.  2014). Los otros tres fueron desarrollados específicamente por científicos políticos para abordar los problemas de análisis de datos que encontraron en su investigación: MCMCpack permite a los usuarios estimar una variedad de modelos en un marco bayesiano utilizando la cadena de Markov Monte Carlo (MCMC) (Martin et al. 2011). cem permite al usuario realizar un emparejamiento exacto aproximado, un método para la inferencia causal con datos de campo (Iacus et al. 2009, 2011). Por último, wnominate permite al usuario escalar los datos de elección, como los datos de la lista legislativa, para estimar los puntos ideales ideológicos de los legisladores o encuestados (Poole y Rosenthal 1997; Poole y col. 2011). Las siguientes cuatro secciones considerarán cada paquete por separado, por lo que cada sección presentará su ejemplo de datos a su vez. Estas secciones están diseñadas para ofrecer una breve descripción de los tipos de capacidades  que ofrecen los paquetes R , aunque algunos lectores pueden no estar familiarizados con los antecedentes de algunos de estos métodos. Se anima al lector interesado a consultar algunos de los recursos citados para aprender más sobre la teoría detrás de estos métodos.

8.1 Modelos multinivel con lme4
Habiendo discutido los modelos lineales en el Cap. 6 y varios ejemplos de modelos lineales generalizados en el Cap. 7  , pasamos ahora a una extensión de este tipo de modelos: modelos multinivel. Los modelos multinivel, o modelos jerárquicos, son apropiados siempre que los datos de interés tengan una estructura anidada o longitudinal. Una estructura anidada ocurre cuando se puede pensar que las observaciones están dentro o como parte de una unidad de nivel superior: un ejemplo de política común es estudiar los resultados del aprendizaje de los estudiantes, pero los estudiantes están anidados dentro de las aulas. En tal caso, el investigador debería tener en cuenta el hecho de que los estudiantes de la muestra no son independientes entre sí, pero es probable que sean similares si se encuentran en la misma clase. De manera similar, siempre que un investigador estudia individuos que han repetido observaciones a lo largo del tiempo, es razonable pensar que las observaciones referenciadas en el tiempo están integradas dentro de las observaciones del individuo. Por ejemplo,1986) datos introducidos por primera vez en el Cap. 4 , los ingresos de los participantes en su estudio se observan en 1974, 1975 y 1978. Algunos análisis de políticas pueden optar por considerar las tres observaciones temporales para cada individuo como anidadas dentro del caso de cada individuo. 1 Se pueden encontrar explicaciones más completas de los modelos multinivel en Scott et al. ( 2013) y Gelman y Hill (2007). Continuamos ampliando dos de nuestros ejemplos anteriores para ilustrar un modelo lineal multinivel y un modelo logit multinivel.

8.1.1 Regresión lineal multinivel
En este ejemplo, volvemos a nuestro ejemplo del Cap. 6 sobre el número de horas que los docentes dedican a la docencia en el aula. Originalmente, ajustamos un modelo lineal utilizando mínimos cuadrados ordinarios (MCO) como nuestro estimador. Sin embargo, Berkman y Plutzer ( 2010) señalan que es probable que los profesores del mismo estado compartan características similares. Estas características podrían ser similitudes en la capacitación, en la cultura local o en la ley estatal. Para dar cuenta de estas similitudes no observadas, podemos pensar en los maestros como anidados dentro de los estados. Por esta razón, agregaremos un efecto aleatorio para cada estado. Los efectos aleatorios explican la correlación intraclase o la correlación de errores entre observaciones dentro del mismo grupo. En presencia de correlación intraclase, las estimaciones de MCO son ineficientes porque los términos de perturbación no son independientes, por lo que un modelo de efectos aleatorios da cuenta de este problema.

Primero, recargamos los datos de la Encuesta Nacional de Maestros de Biología de Escuelas Secundarias de la siguiente manera: 2

rm (lista = ls ())

biblioteca (extranjera)

evolución <-read.dta ("BPchap7.dta")

evolución $ mujer [evolución $ mujer == 9] <- NA

evolución <-subconjunto (evolución,! is.na (mujer))

Recuerde que teníamos un puñado de observaciones de mujeres que debían ser recodificadas como perdidas. Como antes, subconjuntamos nuestros datos para omitir estas observaciones faltantes.

Para adaptarse a un modelo multinivel, en realidad hay algunos comandos disponibles. Vamos a optar por usar un comando del lme4 ( l Inear m ixed correo biblioteca fecto). 3 En nuestro primer uso, instalaremos el paquete y luego, en cada uso, cargaremos la biblioteca:

install.packages ("lme4")

biblioteca (lme4)

Una vez que hemos cargado la biblioteca, nos ajustamos nuestro modelo lineal multinivel mediante el LMER ( l inear m ixed e FECTOS r egression) comando:

hours.ml <-lmer (hrs_allev ~ fase1 + senior_c + ph_senior + notest_p +

     ph_notest_p + female + biocred3 + degr3 + evol_course + certificado +

     idsci_trans + confiado + (1 | st_fip), datos = evolución)

La sintaxis del comando lmer es casi idéntica al código que usamos al ajustar un modelo con OLS usando lm . De hecho, el único atributo que agregamos es el término adicional (1 | st_fip) en el lado derecho del modelo. Esto agrega una intersección aleatoria por estado. En cualquier ocasión en la que queramos incluir un efecto aleatorio, colocamos entre paréntesis el término para el que se incluye el efecto seguido de una barra vertical y la variable que identifica las unidades de nivel superior. Entonces, en este caso, queríamos una intersección aleatoria (de ahí el uso de 1 ), y queríamos que estos fueran asignados por estado (de ahí el uso de st_fip ).

Obtenemos nuestros resultados escribiendo:

resumen (horas.ml)

En nuestra salida de resultados, R  imprime la correlación entre todos los efectos fijos o parámetros de regresión estimados. Esta parte de la impresión se omite a continuación:

Ajuste de modelo lineal mixto de REML ['lmerMod']

Fórmula:

hrs_allev ~ fase1 + senior_c + ph_senior + notest_p + ph_notest

 _p +

    female + biocred3 + degr3 + evol_course + certificada + idsci_

      trans +

    seguro + (1 | st_fip)

   Datos: evolución

Criterio REML en la convergencia: 5940

Residuos escalados:

    Mín. 1T Mediana 3T Máx.

-2,3478 -0,7142 -0,1754 0,5566 3,8846

Efectos aleatorios:

 Grupos Nombre Varianza Desv. Estándar

 st_fip (Intercepción) 3.089 1.758

 Residual 67.873 8.239

Número de obs: 841, grupos: st_fip, 49

Efectos fijos:

            Estimar Std. Valor t de error

(Intercepción) 10.5676 1.2138 8.706

fase1 0,7577 0,4431 1,710

senior_c -0.5291 0.3098 -1.708

ph_senior -0.5273 0.2699 -1.953

notest_p 0.1134 0.7490 0.151

ph_notest_p -0.5274 0.6598 -0.799

mujer -0,9702 0,6032 -1,608

biocred3 0.5157 0.5044 1.022

grados3 -0,4434 0,3887 -1,141

evol_course 2.3894 0.6270 3.811

certificado -0,5335 0,7188 -0,742

idsci_trans 1.7277 1.1161 1.548

seguro 2.6739 0.4468 5.984

La salida primero imprime una variedad de estadísticas de ajuste: AIC, BIC, log-verosimilitud, desviación y desviación de máxima verosimilitud restringida. En segundo lugar, imprime la varianza y la desviación estándar de los efectos aleatorios. En este caso, la varianza para el término st_fip es la varianza de nuestros efectos aleatorios a nivel de estado. La varianza residual corresponde a la varianza del error de regresión que normalmente calcularíamos para nuestros residuos. Por último, los efectos fijos que se informan son sinónimos de coeficientes de regresión lineal que normalmente nos interesan, aunque ahora nuestras estimaciones han tenido en cuenta la correlación intraclase entre profesores dentro del mismo estado. Cuadro  8.1compara nuestras estimaciones OLS y multinivel una al lado de la otra. Como puede verse, el modelo multinivel ahora divide la varianza inexplicable en dos componentes (nivel estatal e individual), y las estimaciones de coeficientes han cambiado algo.
Cuadro 8.1
Dos modelos de horas de clase dedicadas a la enseñanza de la evolución por profesores de biología de secundaria

 	
OLS

Multi nivel

 
Parámetro

Estimar

Std. error

Pr (> |  z  |)

Estimar

Std. error

Pr (> |  z  |)

 
Interceptar

10.2313

1.1905

0,0000

10.5675

1.2138

0,0000

 
Índice de estándares 2007

0,6285

0.3331

0.0596

0,7576

0.4431

0.0873

 
Antigüedad (centrada)

−0,5813

0.3130

0.0636

−0,5291

0.3098

0.0876

 
Estándares × antigüedad

−0,5112

0.2717

0.0603

−0,5273

0.2699

0.0508

 
Cree que no hay prueba

0.4852

0,7222

0.5019

0,1135

0,7490

0.8795

 
Estándares × cree que no hay prueba

−0,5362

0,6233

0.3899

−0,5273

0,6598

0.4241

 
La maestra es mujer

−1,3546

0,6016

0.0246

−0,9703

0,6032

0.1077

 
Créditos obtenidos en biología (0-2)

0.5559

0.5072

0.2734

0.5157

0.5044

0.3067

 
Grados en ciencias (0-2)

−0.4003

0.3922

0.3077

−0,4434

0.3887

0.2540

 
Clase de evolución completada

2.5108

0,6300

0,0001

2.3894

0,6270

0,0001

 
Tiene certificación normal

−0,4446

0,7212

0.5377

−0,5335

0,7188

0.4580

 
Se identifica como científico

1.8549

1.1255

0.0997

1.7277

1.1161

0.1216

 
Experiencia autoevaluada (−1 a +2)

2.6262

0.4501

0,0000

2.6738

0,4468

0,0000

 
Varianza a nivel de estado

-

3.0892

 	 
Varianza a nivel individual

69.5046

 	
67,8732

 	 
Notas : N  = 841. Datos de Berkman y Plutzer (2010)

8.1.2 Regresión logística multinivel
Si bien es algo más complejo, la lógica del modelado multinivel también se puede aplicar al estudiar variables dependientes limitadas. Hay dos enfoques amplios para extender los GLM a un marco multinivel: modelos marginales, que tienen una interpretación de población promediada, y modelos lineales mixtos generalizados (GLMM), que tienen una interpretación a nivel individual (Laird y Fitzmaurice 2013, págs. 149-156). Si bien se anima a los lectores a leer más sobre los tipos de modelos disponibles, su estimación y su interpretación, por ahora nos centramos en el proceso de estimación de un GLMM.

En este ejemplo, volvemos a nuestro ejemplo de Sect. 7.1.1 del último capítulo, sobre si un encuestado informó haber votado por el partido en el poder en función de la distancia ideológica del partido. Como Singh ( 2014a) observa, los votantes que hagan su elección en el mismo país-año se enfrentarán a muchas características de la elección que son exclusivas de esa elección. Por lo tanto, es probable que exista una correlación intraclase entre los votantes dentro de la misma elección. Además, el efecto de la ideología en sí puede ser más fuerte en algunas elecciones que en otras: los métodos multinivel, incluidos los GLMM, nos permiten evaluar si existe variación en el efecto de un predictor entre grupos, que es una característica que usaremos.

Volviendo a los detalles del código, si la biblioteca lme4 no está cargada, la necesitamos nuevamente. Además, si los datos de no se cargan, entonces necesitamos cargar la biblioteca externa y el conjunto de datos en sí. Todo esto se logra de la siguiente manera: 4

biblioteca (lme4)

biblioteca (extranjera)

votando <-read.dta ("SinghJTP.dta")

Basándonos en el modelo de la tabla  7.1 , primero simplemente agregamos una intersección aleatoria a nuestro modelo. La sintaxis para estimar el modelo e imprimir los resultados es:  

inc.linear.ml <-glmer (votadainc ~ distanciainc + (1 | cntryyear),

     familia = binomio (enlace = "logit"), datos = votación)

resumen (incluido lineal.ml)

Tenga en cuenta que ahora usamos la glmer comando ( g eneralized l inear m ixed e FECTOS r egression). Al usar la opción de familia , podemos usar cualquiera de las funciones de enlace comunes disponibles para el comando glm . Un vistazo al resultado muestra que, además de los efectos fijos tradicionales que reflejan los coeficientes de regresión logística, también se nos presenta la varianza de la intersección aleatoria para el país y el año de la elección:

Ajuste de modelo lineal mixto generalizado por Laplace

  aproximación

Fórmula: voteinc ~ distanciainc + (1 | cntryyear)

   Datos: votación

      Desviación de logLik de AIC BIC

 41998.96 42024.62 -20996.48 41992.96

Efectos aleatorios:

 Grupos Nombre Varianza Desv. Estándar

 cntryyear (intersección) 0.20663 0.45457

Número de obs: 38211, grupos: cntryyear, 30

Efectos fijos:

                Estimar Std. Error z valor Pr (> | z |)

(Intercepción) 0.161788717 0.085578393 1.89053 0.058687.

distanciainc -0.501250136 0.008875997 -56.47254 <2e-16 ***

---

Signif. códigos: 0 *** 0,001 ** 0,01 * 0,05. 0,1 1

Correlación de efectos fijos:

            (Intr.)

distanciainc -0.185

Para replicar un modelo más acorde con Singh (2014a), ahora ajustamos un modelo que incluye una intersección aleatoria y un coeficiente aleatorio de distancia ideológica, ambos condicionados por el país y el año de la elección. La sintaxis para estimar este modelo e imprimir el resultado es:

inc.linear.ml.2 <-glmer (votadainc ~ distanciainc +

     (distanciainc | cntryyear), family = binomial (link = "logit"),

     datos = votación)

resumen (incluido lineal.ml.2)

Observe que ahora hemos condicionado la variable distanciainc por cntryyear . Esto agrega un coeficiente aleatorio para la distancia ideológica. Además, de forma predeterminada, agregar este efecto aleatorio también agrega una intersección aleatoria. Nuestro resultado en este caso es:

Ajuste de modelo lineal mixto generalizado por Laplace

  aproximación

Fórmula: voteinc ~ distanciainc + (distanciainc |

  cntryyear)

   Datos: votación

   Desviación de logLik de AIC BIC

 41074 41117-20532 41064

Efectos aleatorios:

 Grupos Nombre Varianza Desv. Estándar Corr

 cntryyear (intersección) 0,616658 0,78528

           distanciainc 0.098081 0.31318 -0.808

Número de obs: 38211, grupos: cntryyear, 30

Efectos fijos:

            Estimar Std. Error z valor Pr (> | z |)

(Intercepción) 0.26223 0.14531 1.805 0.0711.

distanciainc -0.53061 0.05816 -9.124 <2e-16 ***

---

Signif. códigos: 0 *** 0,001 ** 0,01 * 0,05. 0,1 1

Correlación de efectos fijos:

            (Intr.)

distanciainc -0.808

Bajo efectos aleatorios, primero vemos la varianza para la intersección aleatoria con referencia a la elección y luego la varianza para el coeficiente con referencia a la elección para la distancia ideológica. Los efectos fijos de los coeficientes de regresión logística también se presentan de la forma habitual. El AIC indica que esta versión del modelo se ajusta mejor que el modelo con solo una intersección aleatoria o el modelo de la Tabla  7.1 que no incluyó efectos aleatorios, ya que el puntaje de 41,074 es menor que el AIC de cualquiera de esos modelos. En resumen, esta discusión debería ofrecer una idea de los tipos de modelos jerárquicos que R  puede estimar usando lme4 (Bates et al.  2014).

8.2 Métodos bayesianos con MCMCpack
El paquete MCMCpack permite a los usuarios realizar inferencias bayesianas en una variedad de modelos de regresión y modelos de medición comunes. El paquete incluso tiene un comando, MCMCmetrop1R , que construirá una muestra de MCMC a partir de una distribución definida por el usuario utilizando un algoritmo de Metropolis. Se anima a los lectores que deseen aprender más sobre los métodos bayesianos a consultar recursos como: Carlin y Louis (2009), Gelman et al. (2004), Branquias (2008) y Robert (2001).

Como una simple ilustración de cómo funciona el paquete, nos enfocamos en esta sección en algunos de los modelos de regresión comunes que están programados en el paquete. Esto es poderoso para el  usuario de R, ya que los investigadores que prefieren informar modelos bayesianos pueden hacerlo fácilmente si la especificación de su modelo se ajusta a una estructura común. Al ilustrar estas técnicas, revisaremos una vez más el modelo lineal de horas de evolución de Berkman y Plutzer (2010) y el modelo de regresión logística del apoyo del partido en el poder de Singh (2014a).

8.2.1 Regresión lineal bayesiana
Para estimar nuestro modelo de regresión lineal bayesiano, debemos volver a cargar los datos de la Encuesta Nacional de Maestros de Biología de Escuelas Secundarias, si aún no están cargados:

rm (lista = ls ())

biblioteca (extranjera)

evolución <-read.dta ("BPchap7.dta")

evolución $ mujer [evolución $ mujer == 9] <- NA

evolución <-subconjunto (evolución,! is.na (mujer))

Con los datos cargados, debemos instalar MCMCpack si este es el primer uso del paquete en la computadora. Una vez instalado el programa, debemos cargar la biblioteca:

install.packages ("MCMCpack")

biblioteca (MCMCpack)

Ahora podemos usar MCMC para ajustar nuestro modelo de iones de regresión lineal bayesiana con el comando MCMCregress :

mcmc.horas <-MCMCregress (hrs_allev ~ fase1 + senior_c + ph_senior +

     notest_p + ph_notest_p + female + biocred3 + degr3 +

     evol_course + certificado + idsci_trans + seguro, datos = evolución)

Esté preparado para que la estimación con MCMC generalmente toma más tiempo computacionalmente, aunque los modelos simples como este generalmente terminan con bastante rapidez. Además, debido a que MCMC es una técnica basada en simulación, es normal que los resúmenes de los resultados difieran ligeramente entre las repeticiones. Con este fin, si encuentra diferencias entre sus resultados y los impresos aquí después de usar el mismo código, no debe preocuparse a menos que los resultados sean marcadamente diferentes.

Si bien el código anterior se basa en los valores predeterminados del comando MCMCregress , algunas de las opciones de este comando son esenciales para resaltar: Una característica central de los métodos bayesianos es que el usuario debe especificar las prioridades para todos los parámetros que se estiman. Los valores predeterminados para MCMCregress son valores previos conjugados vagos para los coeficientes y la varianza de las perturbaciones. Sin embargo, el usuario tiene la opción de especificar sus propios antecedentes sobre estas cantidades. 5 Se anima a los usuarios a que revisen estas opciones y otros recursos sobre cómo establecer antecedentes (Carlin y Louis 2009; Gelman y col. 2004; Branquia 2008; Robert 2001). Los usuarios también tienen la opción de cambiar el número de iteraciones en la muestra de MCMC con la opción mcmc y el período de quemado (es decir, el número de iteraciones iniciales que se descartan) con la opción de quemado . Los usuarios siempre deben evaluar la convergencia del modelo después de estimar un modelo con MCMC (que se discutirá en breve) y considerar si el quemado o el número de iteraciones deben cambiarse si hay evidencia de no convergencia.

Después de estimar el modelo, escribiendo resumen (mcmc.hours) ofrecerá un resumen rápido de la muestra posterior :

Iteraciones = 1001: 11000

Intervalo de dilución = 1

Número de cadenas = 1

Tamaño de muestra por cadena = 10000

1. Media empírica y desviación estándar de cada

   variable, más el error estándar de la media:

               Media SD Naive SE Serie temporal SE

(Intercepción) 10,2353 1,1922 0,011922 0,011922

fase1 0,6346 0,3382 0,003382 0,003382

senior_c -0.5894 0.3203 0.003203 0.003266

ph_senior -0,5121 0,2713 0,002713 0,002713

notest_p 0,4828 0,7214 0,007214 0,007214

ph_notest_p -0.5483 0.6182 0.006182 0.006182

mujer -1,3613 0,5997 0,005997 0,006354

biocred3 0,5612 0,5100 0,005100 0,005100

grados3 -0,4071 0,3973 0,003973 0,003973

evol_course 2,5014 0,6299 0,006299 0,005870

certificado -0,4525 0,7194 0,007194 0,007194

idsci_trans 1.8658 1.1230 0.011230 0.010938

seguro 2.6302 0.4523 0.004523 0.004590

sigma2 70,6874 3,5029 0,035029 0,035619

2. Cuantiles para cada variable:

                2,5% 25% 50% 75% 97,5%

(Intercepción) 7.92359 9.438567 10.2273 11.03072 12.59214

fase1 -0.02787 0.405026 0.6384 0.86569 1.30085

senior_c -1.22527 -0.808038 -0.5885 -0.37351 0.04247

ph_senior -1.04393 -0.694228 -0.5105 -0.32981 0.03152

notest_p -0.92717 -0.006441 0.4863 0.97734 1.88868

ph_notest_p -1.75051 -0.972112 -0.5462 -0.13138 0.63228

femenino -2.52310 -1.771210 -1.3595 -0.96109 -0.18044

biocred3 -0,42823 0,212168 0,5558 0,90768 1,55887

grados3 -1.19563 -0.671725 -0.4048 -0.14536 0.38277

evol_course 1.26171 2.073478 2.5064 2.92601 3.73503

certificado -1.84830 -0.942671 -0.4477 0.03113 0.95064

idsci_trans -0.33203 1.107771 1.8667 2.63507 4.09024

confiado 1.73568 2.324713 2.6338 2.94032 3.48944

sigma2 64.12749 68.277726 70.5889 72.95921 77.84095

Dado que MCMC produce una muestra de valores de parámetros simulados, toda la información reportada se basa en estadísticas descriptivas simples de la salida simulada (que son 10,000 conjuntos de parámetros, en este caso). La parte 1 del resumen anterior muestra la media de la muestra para cada parámetro, la desviación estándar de la muestra de cada parámetro y dos versiones del error estándar de la media. La parte 2 del resumen muestra los percentiles de la muestra de cada parámetro. La Tabla  8.2 muestra un formato común para presentar los resultados de un modelo como este: informar la media y la desviación estándar de la distribución posterior marginal de cada parámetro y un intervalo de credibilidad del 95% basado en los percentiles. 6
Cuadro 8.2
Modelo lineal de horas de clase dedicadas a la evolución de la enseñanza por profesores de biología de secundaria (estimaciones de MCMC)

Vaticinador

Significar

Std. Dev.

[95% Cred. En t.]

 
Interceptar

10.2353

1.1922

[7,9236:

12.5921]

 
Índice de estándares 2007

0,6346

0.3382

[−0,0279:

1.3008]

 
Antigüedad (centrada)

−0,5894

0.3203

[−1,2253:

0.0425]

 
Estándares × antigüedad

−0,5121

0.2713

[−1.0439:

0.0315]

 
Cree que no hay prueba

0.4828

0,7214

[−0,9272:

1.8887]

 
Estándares × cree que no hay prueba

−0,5483

0,6182

[−1,7505:

0.6323]

 
La maestra es mujer

−1,3613

0.5997

[−2,5231:

−0,1804]

 
Créditos obtenidos en biología (0-2)

0.5612

0.5100

[−0,4282:

1.5589]

 
Grados en ciencias (0-2)

−0,4071

0.3973

[−1,1956:

0.3828]

 
Clase de evolución completada

2.5014

0,6299

[1.2617:

3.7350]

 
Tiene certificación normal

−0,4525

0,7194

[−1,8483:

0.9506]

 
Se identifica como científico

1.8658

1.1230

[−0,3320:

4.0902]

 
Experiencia autoevaluada (- 1 a + 2)

2.6302

0.4523

[1.7357:

3.4894]

 
Varianza de error de regresión

70.6874

3.5029

[64,1275:

77.8410]

 
Notas : N  = 841. Datos de Berkman y Plutzer (2010)

Cuando un usuario carga MCMCpack en R , también se cargará la biblioteca de coda . 7 coda es particularmente útil porque permite al usuario evaluar la convergencia de los modelos estimados con MCMC y reportar cantidades adicionales de interés. Como se mencionó anteriormente, cada vez que un investigador estima un modelo utilizando MCMC, debe evaluar si existe alguna evidencia de no convergencia. En el caso de que las cadenas de Markov no hayan convergido, el modelo debe ser muestreado para más iteraciones. MCMCregress estima el modelo utilizando una sola cadena. Por lo tanto, para nuestro modelo del número de horas de evolución que se enseñan en las aulas de secundaria, podemos evaluar la convergencia utilizando Geweke convergencia ‘sdiag nostic, que simplemente pregunta si los medios de la primera y última parte de la cadena son los mismos. Para calcular este diagnóstico, escribimos:

geweke.diag (mcmc.horas, frac1 = 0.1, frac2 = 0.5)

Aquí hemos especificado que queremos comparar la media del primer 10% de la cadena ( frac1 = 0.1 ) con la media del último 50% de la cadena ( frac2 = 0.5 ). La salida resultante presenta una relación z para esta prueba de diferencia de medias para cada parámetro:

Fracción en la 1ra ventana = 0.1

Fracción en la segunda ventana = 0.5 (Intercepción) fase1 senior_c ph_senior notest_p

   -1,34891 -1,29015 -1,10934 -0,16417 0,95397

ph_notest_p hembra biocred3 degr3 evol_course

    1,13720 -0,57006 0,52718 1,25779 0,62082

  certificado idsci_trans seguro sigma2

    1,51121 -0,87436 -0,54549 -0,06741

En este caso, ninguna de las estadísticas de prueba supera ningún umbral de significancia común para una estadística de prueba distribuida normalmente, por lo que no encontramos evidencia de no convergencia. Con base en esto, podemos estar contentos con nuestra muestra original de MCMC de 10,000.

Una cosa más que podemos desear hacer con nuestra salida de MCMC es trazar la función de densidad estimada general de nuestras distribuciones posteriores marginales. Podemos graficar estos uno a la vez utilizando la función densplot , aunque el analista deberá hacer referencia al parámetro de interés según su orden numérico de aparición en la tabla de resumen. Por ejemplo, si quisiéramos graficar el coeficiente para el indicador de si un docente completó una clase de evolución ( evol_course ), esa es la estimación del décimo parámetro reportado en la tabla. De manera similar, si quisiéramos informar la gráfica de densidad para el coeficiente de la experiencia autoevaluada del maestro ( confianza), que es el decimotercer parámetro informado en la tabla resumen. Por lo tanto, podríamos trazar cada uno de estos escribiendo:

densplot (mcmc.horas [, 10])

densplot (mcmc.horas [, 13])

Las gráficas de densidad resultantes se presentan en la Fig.  8.1 . Como muestran las figuras, ambas distribuciones posteriores marginales tienen una distribución normal aproximada y la moda se encuentra cerca de la media y la mediana informadas en nuestro resultado resumido.
Abrir imagen en nueva ventanaFigura 8.1
Figura 8.1
Gráficos de densidad de distribución marginal posterior de coeficientes para determinar si el maestro completó una clase de evolución y la pericia autoevaluada del maestro. Basado en una muestra de MCMC de 10,000 iteraciones (quemado de 1000 iteraciones). ( a ) Clase de evolución completa; ( b ) Experiencia autoevaluada

8.2.2 Regresión logística bayesiana
Como una ilustración adicional de que MCMCpack estima una variedad de modelos, ilustramos la regresión logística bayesiana, utilizando Singh (2014a) datos sobre la votación de los partidos en el poder por última vez. Si no tiene estos datos o la biblioteca cargada, asegúrese de hacerlo:

biblioteca (MCMCpack)

biblioteca (extranjera)

votando <-read.dta ("SinghJTP.dta")

Para estimar el modelo logit bayesiano usando MCMC , escribimos :

inc.linear.mcmc <-MCMClogit (votadainc ~ distanciainc, datos = votando)

Al igual que con el comando MCMCregress , hemos optado por utilizar los valores predeterminados en este caso, pero se recomienda a los usuarios que consideren establecer sus propios valores previos para satisfacer sus necesidades. De hecho, este es un caso en el que necesitaremos aumentar el número de iteraciones en nuestro modelo. Podemos verificar la convergencia de nuestro modelo usando el diagnóstico de Geweke:

geweke.diag (incluido linear.mcmc, frac1 = 0.1, frac2 = 0.5)

Nuestro resultado en este caso muestra una diferencia significativa entre las medias al principio y al final de la cadena para cada parámetro:

Fracción en la 1ra ventana = 0.1

Fracción en la segunda ventana = 0.5

(Intercepción) distanciainc

      2.680 -1.717

El valor absoluto de ambas razones z excede 1.645, por lo que podemos decir que la media es significativamente diferente para cada parámetro al nivel de confianza del 90%, lo cual es evidencia de no convergencia.

Como respuesta, podemos duplicar nuestro período de quemado y el número de iteraciones a 2,000 y 20,000, respectivamente. El código es:

inc.linear.mcmc.v2 <-MCMClogit (votadoinc ~ distanciainc,

     datos = votación, quemado = 2000, mcmc = 20000)

Ahora podemos verificar la convergencia de esta nueva muestra escribiendo:

geweke.diag (incluido linear.mcmc.v2, frac1 = 0.1, frac2 = 0.5)

Nuestra salida ahora muestra relaciones z no significativas para cada parámetro, lo que indica que ya no hay evidencia de no convergencia:

Fracción en la 1ra ventana = 0.1

Fracción en la segunda ventana = 0.5

(Intercepción) distanciainc

    -1,0975 0,2128

Continuando con esta muestra de 20.000, entonces, si escribimos

resumen (inc.linear.mcmc.v2) en la consola, el resultado es:

Iteraciones = 2001: 22000

Intervalo de dilución = 1

Número de cadenas = 1

Tamaño de muestra por cadena = 20000

1. Media empírica y desviación estándar de cada

   variable, más el error estándar de la media:

               Media SD Naive SE Serie temporal SE

(Intercepción) 0.1940 0.01846 1.305e-04 0.0003857

distanciainc -0.4946 0.00829 5.862e-05 0.0001715

2. Cuantiles para cada variable:

               2,5% 25% 50% 75% 97,5%

(Intercepción) 0,1573 0,1817 0,1944 0,2063 0,2298

distanciainc -0.5105 -0.5003 -0.4946 -0.4890 -0.4783

Estos resultados son similares a los que informamos en el último capítulo en la tabla  7.1 , aunque ahora tenemos la oportunidad de interpretar los hallazgos como bayesianos. Al igual que con la regresión lineal bayesiana, si quisiéramos informar las gráficas de densidad de cada parámetro, podríamos aplicar el comando densplot como antes. En general, esta breve ilustración debería mostrar a los investigadores lo fácil que es usar métodos bayesianos en R  con MCMCpack . Se recomienda a los lectores que deseen utilizar métodos bayesianos más avanzados que consulten el manual de referencia de MCMCpack y Martin et al. ( 2011).

8.3 Inferencia causal con cem
Una innovación destacada en la metodología política ha sido el desarrollo de varios métodos nuevos de emparejamiento. En resumen, el emparejamiento es una técnica diseñada para seleccionar un subconjunto de datos de campo para realizar una comparación justa de las personas que reciben un tratamiento para controlar a las personas que no recibieron el tratamiento. Con el emparejamiento, algunas observaciones se descartan para que las observaciones de control y tratamiento restantes sean similares en todas las covariables que se sabe que dan forma al resultado de interés. En ausencia de datos experimentales, los métodos de emparejamiento sirven para permitir al investigador aislar cómo la variable de tratamiento afecta las respuestas (ver Rubin 2006 para un tratamiento completo de la inferencia causal con emparejamiento).

Los científicos políticos han desarrollado varios métodos nuevos de emparejamiento (Imai y van Dyk 2004; Sekhon y Grieve 2012). Como ilustración de uno de ellos, y de cómo se implementa la técnica novedosa en R , recurrimos al método desarrollado por Iacus et al. (2009, 2011, 2012), C oarsened E xact M atching (CEM). En resumen, CEM procede recodificando temporalmente cada covariable en una variable ordenada que agrupa valores similares de la covariable. Luego, los datos se clasifican en estratos en función de sus perfiles de las variables aproximadas, y cualquier observación en un estrato que no contiene al menos un tratamiento y una unidad de control se desecha. La muestra resultante debe mostrar mucho más equilibrio en las variables de control entre las observaciones tratadas y de control. En R , esta técnica se implementa con el comando cem dentro del paquete cem .

8.3.1 Desequilibrio de covariables, implementación de CEM y ATT
Como datos de nuestro ejemplo, volvemos a LaLonde (1986) estudio de la Demostración Nacional de Trabajo Apoyado que examinamos anteriormente en los Capítulos. 4 y 5. Nuestra variable de tratamiento ( tratado ) indica si la persona recibió el tratamiento de la Demostración Nacional de Trabajo Apoyado, es decir, la persona fue colocada en un trabajo del sector privado durante un año con fondos públicos que cubren los costos laborales. Nos gustaría conocer el efecto causal de este tratamiento sobre los ingresos del individuo en 1978, una vez finalizado el tratamiento ( re78 ). En la Secta. 5.1.1  Hicimos una prueba ingenua de esta hipótesis simplemente usando una prueba de diferencia de medias entre los individuos tratados y el grupo de control sin controlar ninguna de las otras variables que probablemente afecten el ingreso anual. En nuestra prueba ingenua, encontramos que los ingresos eran más altos para el grupo tratado, pero ahora podemos preguntar cuál es el efecto estimado cuando contabilizamos otras covariables importantes.

Como se señaló en capítulos anteriores, los datos de LaLonde ya están incluidos en el paquete cem , por lo que podemos cargar estos datos fácilmente si ya hemos cargado la biblioteca cem . Las siguientes líneas de código limpian, instalan cem (en caso de que aún no lo haya hecho), abren cem y cargan los datos de LaLonde (llamados LL ): 8

rm (lista = ls ())

install.packages ("cem")

biblioteca (cem)

datos (LL)

Una tarea importante cuando se utilizan métodos de emparejamiento es evaluar el grado en que los datos están equilibrados o el grado en que los casos tratados tienen una distribución similar de valores de covariables en relación con el grupo de control. Podemos evaluar el grado en que nuestros grupos de tratamiento y control tienen distribuciones diferentes con el comando de desequilibrio . En el siguiente código, primero aumentamos la penalización por la notación científica (una opción si prefiere la notación decimal). Luego, creamos un vector que nombra las variables para las que no queremos evaluar el equilibrio: la variable de tratamiento ( tratada ) y el resultado de interés ( re78). Todas las demás variables del conjunto de datos son covariables que creemos que pueden dar forma a los ingresos en 1978, por lo que nos gustaría tener un equilibrio sobre ellas. En la última línea, en realidad llamamos al comando de desequilibrio .

opciones (scipen = 8)

todrop <- c ("tratado", "re78")

desequilibrio (grupo = LL $ tratado, datos = LL, drop = todrop)

Dentro del comando de desequilibrio , el argumento de grupo es nuestra variable de tratamiento que define los dos grupos para los que queremos comparar las distribuciones de covariables. El argumento de datos nombra el conjunto de datos que estamos usando y la opción de descartar nos permite omitir ciertas variables del conjunto de datos al evaluar el equilibrio de covariables. Nuestro resultado de este comando es el siguiente:

Medida de desequilibrio multivariante: L1 = 0,735

Porcentaje de apoyo común local: LCS = 12,4%

Medidas de desequilibrio univariante:

               tipo de estadística L1 min 25%

edad 0.179203803 (diff) 4.705882e-03 0 1

educación 0.192236086 (diff) 9.811844e-02 1 0

negro 0.001346801 (diff) 1.346801e-03 0 0

casado 0.010703110 (diff) 1.070311e-02 0 0

nodegree -0.083477916 (diff) 8.347792e-02 0-1

re74 -101.486184085 (diff) 5.551115e-17 0 0

re75 39.415450601 (diff) 5.551115e-17 0 0

hispano -0.018665082 (diff) 1.866508e-02 0 0

u74 -0.020099030 (diferencia) 2.009903e-02 0 0

u75 -0.045086156 (diferencia) 4.508616e-02 0 0

                50% 75% máximo

edad 0.00000 -1.0000 -6.0000

educación 1,00000 1,0000 2,0000

negro 0,00000 0,0000 0,0000

casado 0,00000 0,0000 0,0000

grado de nodo 0,00000 0,0000 0,0000

re74 69.73096 584.9160 -2139.0195

re75 294.18457 660.6865 490.3945

hispano 0,00000 0,0000 0,0000

u74 0,00000 0,0000 0,0000

u75 0,00000 0,0000 0,0000

La primera línea de nuestros informes de salida  L1, que es una medida del desequilibrio multivariado creado por Iacus et al. (2011). En ese artículo se encuentra disponible una explicación más completa, pero en general esta estadística varía de 0 a 1, y los valores más bajos indican un mejor equilibrio. Cuándo L1= 0 las dos distribuciones se superponen perfectamente, y cuando  L1= 1las dos distribuciones no se superponen en absoluto. Volviendo a la tabla, cada fila muestra varias estadísticas de saldo para una covariable individual. Para todas estas estadísticas, los valores más cercanos a cero son mejores. La columna etiquetada como estadístico muestra la diferencia de medias entre las variables, y la columna etiquetada como L1 calcula Lj1, que es la misma medida que  L1pero solo calculado para la covariable individual. Las columnas restantes muestran diferencias de cuantiles entre los dos grupos (por ejemplo, la diferencia en los mínimos respectivos de los dos grupos, la diferencia entre los percentiles 25 respectivos de los grupos, etc.).

A continuación, vamos a utilizar realmente el CEM de comandos para realizar C oarsened E xact M atching en nuestros datos. Dentro del comando cem , enumeramos nuestra variable de tratamiento con el argumento de tratamiento , nuestro conjunto de datos con el argumento de datos y cualquier variable que no queramos que coincida con el argumento de caída . La caída argumento debe incluir siempre nuestra variable de resultado, si está en el mismo conjunto de datos, así como los índices de datos o variables irrelevantes. Podríamos usar un vector para enumerar todas las variables que queremos que se ignoren, como hicimos antes con el comando de desequilibrio , pero en este caso, solo el resultadore78 debe omitirse . Escribimos:

cem.match.1 <- cem (tratamiento = "tratado", datos = LL, gota = "re78")

cem.match.1

Nuestro resultado inmediato de esto es simplemente el siguiente:

           G0 G1

Todos 425297

Emparejado 222163

Inigualable 203134

Esto nos dice que nuestros datos originales tenían 425 observaciones de control y 297 observaciones tratadas. El CEM incluyó 222 del control y 163 de las observaciones tratadas en la muestra emparejada, y el resto se eliminó. Para ser claros: todas las observaciones todavía están contenidas en el conjunto de datos LL original , pero ahora el objeto cem.match.1 detalla qué observaciones coinciden o no.

Dado que CEM procede agrupando valores similares de covariables en estratos, una característica importante de esto es cómo establecemos los intervalos ordenados de cada predictor en el engrosamiento. El comando cem tiene valores predeterminados razonables si el usuario no establece estos intervalos, pero es importante registrar cuáles son los intervalos. Para ver cuáles fueron nuestros intervalos para los valores de nuestros predictores, podríamos escribir: cem.match.1 $ breaks . Esto nos daría el siguiente resultado:

$ edad

 [1] 17,0 20,8 24,6 28,4 32,2 36,0 39,8 43,6 47,4 51,2 55,0

$ educación

 [1] 3,0 4,3 5,6 6,9 8,2 9,5 10,8 12,1 13,4 14,7 16,0

$ negro

 [1] 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0

$ casado

 [1] 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0

$ nodegree

 [1] 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0

$ re74

 [1] 0.000 3957.068 7914.136 11871.204 15828.272 19785.340

 [7] 23742.408 27699.476 31656.544 35613.612 39570.680

$ re75

 [1] 0.000 3743.166 7486.332 11229.498 14972.664 18715.830

 [7] 22458.996 26202.162 29945.328 33688.494 37431.660

$ hispano

 [1] 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0

$ u74

 [1] 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0

$ u75

 [1] 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0

Para ilustrar lo que esto significa, considere la edad . La categoría más baja de edad engrosada agrupa a todas las personas de 17 a 20,8 años, la segunda categoría agrupa a todas las personas de 20,8 a 24,6 años, y así sucesivamente. Variables como negro , casado , nodal , hispano , u74 y u75 son en realidad binarias, por lo que la mayoría de las categorías que se crean son innecesarias, aunque los contenedores vacíos no dañarán nuestro análisis. Por supuesto, los usuarios no están obligados a utilizar los valores predeterminados de software, y Iacus et al. instar a los investigadores a utilizar un conocimiento sustancial de la medición de cada variable para establecer los rangos de los contenedores de engrosamiento (2012, pag. 9). La sección  8.3.2 ofrece detalles sobre cómo hacer esto.

Ahora podemos evaluar el desequilibrio en la nueva muestra emparejada escribiendo:

desequilibrio (LL $ tratado [cem.match.1 $ igualado],

     LL [cem.match.1 $ coincidente,], drop = todrop)

Nuestro resultado de esto es el siguiente: Medida de desequilibrio multivariante: L1 = 0.592

Porcentaje de apoyo común local: LCS = 25,2%

Medidas de desequilibrio univariante:

              tipo de estadística L1 min 25% 50%

edad -0.42486044 (diff) 0.00000000 0-1 -2.0000

educación -0.10855027 (diff) 0.10902006 0 0 -1.0000

negro -0.01771403 (diff) 0.01771403 0 0 0.0000

casado -0.01630465 (diff) 0.01630465 0 0 0.0000

grado de nodo 0.09022827 (diff) 0.09022827 0 0 0.0000

re74 -119.33548135 (diff) 0.00000000 0 0 0.0000

re75 -50.01527694 (diff) 0.00000000 0 0 -49.3559

hispano 0.01561377 (diff) 0.01561377 0 0 0.0000

u74 0,01619411 (diferencia) 0,01619411 0 0 0,0000

u75 0,02310286 (dif.) 0,02310286 0 0 0,0000

              75% máximo

edad 0,00 1.000

educación 0,00 0,000

negro 0,00 0,000

casado 0,00 0,000

grado de nodo 0,00 0,000

re74 -492.95 416.416

re75 -136.45 -852.252

hispano 0,00 0,000

u74 0,00 0,000

u75 0,00 0,000

Compare esto con los datos originales. Ahora tenemos L1= 0,592, que es menor que nuestra puntuación de 0,735 para los datos brutos, lo que indica que el equilibrio multivariado es mejor en la muestra emparejada. En cuanto a las covariables individuales, puede ver una especie de bolsa mixta, pero en general el equilibrio se ve mejor. Por ejemplo, con la edad, la diferencia de medias es en realidad un poco mayor en valor absoluto con la muestra emparejada (0,42) que con los datos brutos (0,18). Sin embargo, La g e1ahora es minúsculo en la muestra emparejada y menor que el valor 0.0047 para los datos sin procesar. Esto probablemente se deba al hecho de que los datos brutos tienen una mayor discrepancia en el extremo superior que la muestra emparejada. El usuario ahora debe decidir si los casos tratados y de control están lo suficientemente equilibrados o si probar otros engrosamientos para mejorar el equilibrio.

Si el usuario está satisfecho con el nivel de equilibrio, él o ella puede proceder a estimar el A verage T ratamiento efecto sobre la T reated (ATT) con el comando att . Esta cantidad representa el efecto causal sobre el tipo de individuo que recibió el tratamiento. En este comando especificamos cuál es nuestra muestra emparejada usando el argumento obj , la variable de resultado ( re78 ) y el tratamiento ( tratado ) usando el argumento de fórmula , y nuestros datos usando el argumento de datos . Esto nos da:

est.att.1 <- att (obj = cem.match.1, fórmula = re78 ~ tratado, datos = LL)

est.at.1

Nuestro resultado de esto es:

           G0 G1

Todos 425297

Emparejado 222163

Inigualable 203134

Modelo de regresión lineal sobre datos emparejados CEM:

Estimación puntual SATT: 550,962564 (valor p = 0,368242)

95% conf. intervalo: [-647.777701, 1749.702830]

El resultado resume las características de nuestra muestra emparejada y luego informa nuestra estimación del efecto del tratamiento promedio de la muestra en los tratados (SATT): Estimamos en nuestra muestra que los individuos que recibieron el tratamiento ganaron $ 551 más en promedio que aquellos que no recibieron el tratamiento. . Sin embargo, este efecto no es estadísticamente discernible de cero ( p  = 0. 368). Ésta es una conclusión marcadamente diferente de la que extrajimos en el cap. 5 , cuando observamos una diferencia de $ 886 que fue estadísticamente significativa. Esto ilustra la importancia del control estadístico. 

8.3.2 Explorando diferentes soluciones CEM
Como punto final, si un investigador no está satisfecho con el nivel de equilibrio o el tamaño de la muestra en la muestra emparejada, entonces una herramienta para encontrar un mejor equilibrio es el comando cemspace . Este comando produce aleatoriamente varios grosores diferentes para las variables de control (250 diferentes grosores por defecto). Luego, el comando traza el nivel de equilibrio contra el número de observaciones tratadas incluidas en la muestra emparejada. El siguiente código llama a este comando:

cem.explore <-cemspace (tratamiento = "tratado", datos = LL, gota = "re78")

La sintaxis de cemspace es similar a cem , aunque dos opciones más son importantes: mínima y máxima . Estos establecen cuál es el número mínimo y máximo permitido de intervalos aproximados para las variables. El comando anterior usa los valores predeterminados de 1 y 5, lo que significa que no se pueden incluir más de cinco intervalos para una variable. Por lo tanto, todas las muestras coincidentes de este comando serán más burdas que las que usamos en la sección. 8.3.1 y, por tanto, menos equilibrado. Sin embargo, el usuario podría aumentar el máximo a 12 o incluso un número más alto para crear intervalos más finos y potencialmente mejorar el equilibrio con respecto a nuestro resultado anterior.

Nuestra salida de cemspace se muestra en la figura  8.2 . Debido al elemento aleatorio en la elección de grosores, sus resultados no coincidirán exactamente con esta cifra. La figura  8.2 a muestra la figura interactiva que se abre. El eje horizontal de esta figura muestra el número de unidades de tratamiento emparejadas en orden descendente, mientras que el eje vertical muestra el nivel de desequilibrio. En general, una muestra emparejada en la esquina inferior izquierda del gráfico sería ideal, ya que indicaría el mejor equilibrio (reducción del sesgo) y la muestra más grande (aumento de la eficiencia). Normalmente, sin embargo, tenemos que tomar una decisión sobre esta compensación, generalmente poniendo un poco más de peso en minimizar el desequilibrio. Al hacer clic en diferentes puntos del gráfico, la segunda ventana quecemspace crea, que se muestra en la figura  8.2 b mostrará los intervalos utilizados en ese engrosamiento particular. El usuario puede copiar los vectores de los puntos de corte del intervalo y pegarlos en su propio código. Nota: R  no continuará con nuevos comandos hasta que estas dos ventanas estén cerradas.
Abrir imagen en nueva ventanaFigura 8.2
Figura 8.2
Gráfica de las estadísticas de equilibrio para 250 muestras emparejadas de groserías aleatorias contra el número de observaciones tratadas incluidas en la muestra emparejada respectiva. ( a ) Ventana de trazado; ( b ) Ventana X11

En la Fig.  8.2 a, se ha elegido uno de los posibles grosores, y está resaltado y en amarillo. Si queremos implementar este engrosamiento, podemos copiar los vectores que se muestran en la segunda ventana ilustrada en la figura  8.2 b. Pegarlos en nuestro propio  script R produce el siguiente código:

age.cut <-c (17, 26.5, 36, 45.5, 55)

educación.corte <-c (3, 6.25, 9.5, 12.75, 16)

black.cut <-c (0, 0.5, 1)

casado.cut <-c (0, 0.5, 1)

nodogree.cut <-c (0, 0.5, 1)

re74.cut <-c (0, 19785.34, 39570.68)

re75.cut <-c (0, 9357.92, 18715.83, 28073.75, 37431.66)

corte.hispano <-c (0, 0.5, 1)

u74.cut <-c (0, 0.5, 1)

u75.cut <-c (0, 1)

new.cuts <-list (age = age.cut, education = education.cut,

     negro = corte negro, casado = corte casado,

     nodegree = nodegree.cut, re74 = re74.cut, re75 = re75.cut,

     hispano = hispano.cut, u74 = u74.cut, u75 = u75.cut)

Terminamos este código creando una lista de todos estos vectores. Si bien nuestros vectores aquí se han creado utilizando un engrosamiento creado por cemspace , este es el procedimiento que usaría un programador para crear sus propios puntos de corte para los intervalos. Sustituyendo los vectores anteriores con vectores de punto de corte creados por el usuario, un investigador puede usar su propio conocimiento de la medición de las variables para engrosar.

Una vez que hemos definido nuestros propios puntos de corte , ya sea usando cemspace o conocimiento sustantivo, ahora podemos aplicar CEM con el siguiente código:

cem.match.2 <- cem (tratamiento = "tratado", datos = LL, gota = "re78",

     puntos de corte = new.cuts)

Nuestra adición clave aquí es el uso de la opción de puntos de corte , donde ingresamos nuestra lista de intervalos. Al igual que en la Secta. 8.3.1 , ahora podemos evaluar las cualidades de la muestra emparejada, los niveles de desequilibrio y calcular el ATT si lo deseamos:

cem.match.2

desequilibrio (LL $ tratado [cem.match.2 $ igualado],

     LL [cem.match.2 $ coincidente,], drop = todrop)

est.att.2 <- att (obj = cem.match.2, fórmula = re78 ~ tratado, datos = LL)

est.at.2

En este caso, en parte debido a los bins más gruesos que estamos usando, el saldo es peor que el que encontramos en la sección anterior. Por lo tanto, estaríamos mejor en este caso si nos quedamos con nuestro primer resultado. Se anima al lector a intentar encontrar un engrosamiento que produzca un mejor equilibrio que los valores predeterminados del software.

8.4 Análisis de lista legislativa con wnominate
Metodólogos en Ciencias Políticas y otras disciplinas han desarrollado una amplia gama de modelos de medición, varios de los cuales están disponibles para la aplicación de usuario en R . Sin duda, una de las mayoría de los modelos de medición prominentes en la disciplina es designar, de corto para nomina l t hree paso e stimation (Poole y Rosenthal 1997; McCarty y col. 1997; Poole y col. 2011). El modelo NOMINATE analiza los datos de la votación nominal de las votaciones legislativas, ubicando a los legisladores y las alternativas políticas que votan en el espacio ideológico. El modelo es particularmente prominente porque Poole, Rosenthal y sus colegas hacen que los puntajes DW-NOMINATE estén disponibles tanto para la Cámara de Representantes como para el Senado de EE. UU. Para cada período del Congreso. Innumerables autores han utilizado estos datos, interpretando típicamente la puntuación de la primera dimensión como una escala de ideología liberal-conservadora.

En resumen, la lógica básica del modelo se basa en el modelo de proximidad espacial de la política, que esencialmente establece que tanto las preferencias ideológicas de los individuos como las alternativas políticas disponibles pueden representarse en un espacio geométrico de una o más dimensiones. Un individuo generalmente votará por la opción de política que más se acerque en el espacio a su propio punto ideológico ideal (Hotelling 1929; Downs 1957; Negro 1958). El modelo NOMINATE se basa en estos supuestos y coloca a los legisladores y las opciones de políticas en el espacio ideológico según la forma en que los votos de los legisladores se dividen en el transcurso de muchas votaciones nominales y cuando los legisladores se comportan de manera impredecible (produciendo errores en el modelo). Por ejemplo, en el Congreso de los Estados Unidos, los miembros liberales suelen votar de manera diferente a los miembros conservadores, y es más probable que los ideólogos extremos estén en una pequeña minoría siempre que haya un amplio consenso sobre un tema. Antes de aplicar el método NOMINATE a sus propios datos, e incluso antes de descargar datos DW-NOMINATE premedidos para incluirlos en un modelo que calcule, asegúrese de leer más sobre el método y sus suposiciones porque comprender a fondo cómo funciona un método es esencial antes usándolo. En particular, el Cap. 2  y el Apéndice A de Poole y Rosenthal (1997) y el Apéndice A de McCarty et al. (1997) ofrecen descripciones detalladas, pero intuitivas, de cómo funciona el método.

En R , el paquete wnominate implementa W-NOMINATE, que es una versión del algoritmo NOMINATE que solo debe aplicarse a una sola legislatura. Los puntajes de W-NOMINATE son válidos internamente, por lo que es justo comparar los puntajes de los legisladores dentro de un solo conjunto de datos. Sin embargo, los puntajes no se pueden comparar externamente con los puntajes cuando W-NOMINATE se aplica a un período diferente de la legislatura oa un cuerpo de actores completamente diferente. Por tanto, es un buen método para intentar hacer comparaciones transversales entre legisladores de un mismo organismo.

Si bien la aplicación más común para W-NOMINATE ha sido el Congreso de los Estados Unidos, el método podría aplicarse a cualquier cuerpo legislativo. Con ese fin, el ejemplo práctico de esta sección se centra en las votaciones nominales emitidas en las Naciones Unidas. Este conjunto de datos de la ONU está disponible en el paquete wnominate y reúne 237 votaciones nominales emitidas en las tres primeras sesiones de la ONU (1946-1949) por 59 países miembros. Las variables están etiquetadas como V1 a V239 . V1 es el nombre de la nación miembro y V2 es una variable categórica codificada como "WP" para un miembro del Pacto de Varsovia, u "Otro" para todas las demás naciones. Las variables restantes identifican secuencialmente las votaciones nominales.

Para comenzar, limpiamos, instalamos el paquete wnominate la primera vez que lo usamos, cargamos la biblioteca y cargamos los datos UN : 9

rm (lista = ls ())

install.packages ("wnominate")

biblioteca (wnominate)

datos (ONU)

Una vez que se cargan los datos, se pueden ver con los comandos estándar como fix , pero para una vista rápida de cómo se ven los datos, simplemente podríamos escribir: head (UN [, 1: 15]) . Esto mostrará la estructura de los datos a través de las primeras 13 votaciones nominales.

Antes de que podamos aplicar W-NOMINATE, tenemos que reformatear los datos a un objeto de clase rollcall . Para hacer esto, primero tenemos que redefinir nuestro conjunto de datos de la ONU como una matriz y dividir los nombres de los países, si el país estaba en el Pacto de Varsovia y el conjunto de llamadas de lista en tres partes separadas:

UN <-as.matrix (UN)

UN.2 <-UN [, - c (1,2)]

UNnames <-UN [, 1]

legData <-matrix (UN [, 2], length (UN [, 2]), 1)

La primera línea convirtió el marco de datos de la ONU en una matriz. (Para obtener más información sobre los comandos de la matriz en R , consulte el capítulo  10 ) La segunda línea creó una nueva matriz, que hemos denominado UN.2 , que ha eliminado las dos primeras columnas (nombre del país y miembro del Pacto de Varsovia) para dejar solo el pasa lista. La tercera línea exporta los nombres de las naciones al vector UNnames . (En muchos otros entornos, este sería el nombre del legislador o una variable de identificación). Por último, nuestra variable de si una nación estaba en el Pacto de Varsovia se ha guardado como una matriz de una columna denominada legData . (En muchos otros entornos, este sería el partido político de un legislador). Una vez que tengamos estos componentes juntos, podemos usar el comando rollcall para definir un objeto rollcall -class que llamamos rc de la siguiente manera:

rc <-rollcall (data = UN.2, yea = c (1,2,3), nay = c (4,5,6),

     missing = c (7,8,9), notInLegis = 0, legis.names = UNnames,

     legis.data = legData, desc = "UN Votes", source = "voteview.com")

Especificamos nuestra matriz de votaciones nominales con el argumento de datos . Basándonos en cómo se codifican los datos en la matriz de votación nominal, usamos los argumentos sí , no y faltantes para traducir los códigos numéricos a su significado sustantivo. Además, notInLegis nos permite especificar un código que significa específicamente que el legislador no era miembro en el momento de la votación (por ejemplo, un legislador falleció o renunció). No tenemos tal caso en estos datos, pero el valor predeterminado es notInLegis = 9 , y 9 significa algo más para nosotros, por lo que debemos especificar un código no utilizado de 0. Con legis.names especificamos los nombres de los votantes y con legis.dataespecificamos variables adicionales sobre nuestros votantes. Por último, desc y source nos permiten registrar información adicional sobre nuestros datos.

Con nuestros datos formateados correctamente, ahora podemos aplicar el modelo W-NOMINATE. el comando simplemente se llama wnominate :

resultado <-wnominate (rcObject = rc, polarity = c (1,1))

Con el argumento rcObject , simplemente nombramos nuestros datos formateados correctamente. El argumento de la polaridad , por el contrario, requiere una aportación sustancial del investigador: el usuario debe especificar un vector que enumere qué observación debe caer claramente en el lado positivo de cada dimensión estimada. Dada la política de la Guerra Fría, usamos la observación n. ° 1, EE. UU., Como ancla en ambas dimensiones que estimamos. Por defecto, wnominate coloca a los votantes en un espacio ideológico bidimensional (aunque esto podría cambiarse especificando la opción de atenuación ).

Para ver los resultados de nuestra estimación, podemos comenzar escribiendo: resumen (resultado) . Esto imprime la siguiente salida.

RESUMEN DEL OBJETO NOMINADO W

----------------------------

Número de legisladores: 59 (0 legisladores eliminados)

Número de votos: 219 (18 votos eliminados)

Número de dimensiones: 2

Sí pronosticado: 4693 de 5039 (93,1%) predicciones

 correcto

Negativos previstos: 4125 de 4488 (91,9%) predicciones

 correcto

Clasificación correcta: 89,5% 92,56%

APRE: 0,574 0,698

BPF: 0,783 0,841

Las primeras 10 estimaciones de los legisladores son:

              coord1D coord2D

Estados Unidos 0,939 0,344

Canadá 0,932 0,362

Cuba 0,520 -0,385

Haití 0,362 -0,131

República Dominicana 0,796 -0,223

México 0,459 0,027

Guatemala 0,382 0,364

Honduras 0,588 -0,266

El Salvador 0,888 -0,460

Nicaragua 0,876 -0,301

La salida comienza recapitulando varias características descriptivas de nuestros datos y luego cambia para ajustar índices. Enumera, por ejemplo, la predicción correcta de sí y no, y luego, en “Clasificación correcta”, enumera el porcentaje predicho correctamente por la primera dimensión sola y luego ambas dimensiones juntas. Con un 89,5%, la primera dimensión puede explicar mucho por sí misma. El resultado finaliza enumerando las estimaciones de nuestras primeras 10 observaciones. Como puede verse, EE. UU. Tiene un valor positivo en ambas dimensiones, según nuestra especificación en el modelo.

Podemos obtener resultados adicionales escribiendo: plot (result) . El resultado de este comando se presenta en la Fig.  8.3 . El panel superior izquierdo visualiza los puntajes W-NOMINATE para las 59 naciones votantes. El eje horizontal es la primera dimensión de la puntuación, el eje vertical es la segunda dimensión y cada punto representa la posición de una nación. Por lo tanto, en el espacio ideológico bidimensional definido internamente en los procedimientos de la ONU, aquí es donde cae cada nación. El panel de la parte inferior izquierda muestra un gráfico de pantalla, que enumera el valor propio asociado con cada dimensión. Los valores propios más grandes indican que una dimensión tiene más poder explicativo. Como en todas las gráficas de pedregal, cada dimensión adicional tiene un valor explicativo menor que la anterior. 10El panel superior derecho muestra la distribución de los ángulos de las líneas de corte. Las líneas de corte dividen el sí de los votos negativos sobre un tema determinado. El hecho de que tantas líneas de corte estén cerca de la marca de 90 ∘ indica que la primera dimensión es importante para muchos de los votos. Finalmente, el panel inferior derecho muestra la malla de Coombs de este modelo, una visualización de cómo todas las líneas de corte de los 237 votos se unen en un solo espacio.
Abrir imagen en nueva ventanaFigura 8.3
Figura 8.3
Gráfico de salida de la estimación de las puntuaciones de W-NOMINATE de las tres primeras sesiones de las Naciones Unidas

Si el usuario está satisfecho con los resultados de este modelo de medición, entonces es sencillo escribir las puntuaciones en un formato de datos utilizable. Dentro de nuestra wnominate llamado de salida resultado que podemos llamar el atributo llamado a los legisladores , lo que ahorra los puntos ideales para todos los países, las variables de llamada no rollo que hemos especificado (por ejemplo, Pacto de Varsovia o no), y una variedad de otras medidas. Guardamos esto como un nuevo marco de datos llamado puntuaciones y luego lo escribimos en un archivo CSV:

puntuaciones <-resultado $ legisladores

write.csv (puntuaciones, "UNscores.csv")

Solo recuerde usar el comando setwd para especificar la carpeta en la que desea guardar el archivo de salida.

Una vez que tenemos nuestros puntos ideales W-NOMINAR en un marco de datos separado, podemos hacer cualquier cosa que haríamos normalmente con los datos en R , como dibujar nuestros propios gráficos. Supongamos que quisiéramos reproducir nuestro propio gráfico de los puntos ideales, pero queríamos marcar qué naciones eran miembros del Pacto de Varsovia frente a las que no lo eran. Podríamos hacer esto fácilmente usando nuestros datos de puntajes . La forma más fácil de hacer esto podría ser usar el comando subconjunto para crear marcos de datos separados de nuestros dos grupos:

wp.scores <-subconjunto (puntuaciones, V1 == "WP")

other.scores <-subconjunto (puntuaciones, V1 == "Otro")

Una vez que tengamos estos subconjuntos en la mano, podemos crear el gráfico relevante en tres líneas de código.

plot (x = other.scores $ coord1D, y = other.scores $ coord2D,

     xlab = "Primera dimensión", ylab = "Segunda dimensión",

     xlim = c (-1,1), ylim = c (-1,1), asp = 1)

puntos (x = wp.scores $ coord1D, y = wp.scores $ coord2D,

     pch = 3, col = 'rojo')

leyenda (x = -1, y = -. 75, leyenda = c ("Otro", "Pacto de Varsovia"),

     col = c ("negro", "rojo"), pch = c (1,3))

En el llamado a la parcela , graficamos las 53 naciones que no eran miembros del Pacto de Varsovia, colocando la primera dimensión en el eje horizontal y la segunda en el eje vertical. Etiquetamos nuestros ejes apropiadamente usando xlab e ylab . También establecemos que los límites de nuestro gráfico van de -1 a 1 en ambas dimensiones, ya que los puntajes están restringidos a caer en estos rangos. Es importante destacar que, garantizamos que la escala de las dos dimensiones es el mismo, ya que en general debe para este tipo de modelo de medición, mediante el establecimiento de la asp relación ect a 1 ( asp = 1 ). En la segunda línea de código, usamos los puntoscomando para agregar las seis observaciones que estaban en el Pacto de Varsovia, coloreando estas observaciones de rojo y usando un carácter de trama diferente. Por último, agregamos una leyenda.

La figura  8.4 presenta la salida de nuestro código. Este gráfico transmite inmediatamente que la primera dimensión está capturando la división de la Guerra Fría entre los EE. UU. Y sus aliados versus la Unión Soviética y sus aliados. Especificamos que EE. UU. Tomaría coordenadas positivas en ambas dimensiones, por lo que podemos ver que los aliados soviéticos (representados con cruces rojas) están en los extremos de los valores negativos en la primera dimensión.
Abrir imagen en nueva ventanaFigura 8.4
Figura 8.4
Gráfico de la primera y segunda dimensiones de las puntuaciones de W-NOMINATE de las tres primeras sesiones de las Naciones Unidas. Una cruz roja indica un miembro del Pacto de Varsovia y un círculo negro indica todos los demás miembros de la ONU (figura en color en línea)

En resumen, este capítulo ha ilustrado cuatro ejemplos de cómo usar  paquetes R para implementar métodos avanzados. El hecho de que estos paquetes estén disponibles gratuitamente hace que el trabajo de vanguardia en metodología política y de una variedad de disciplinas esté fácilmente disponible para cualquier  usuario de R. Ningún libro podría mostrar todos los paquetes aportados por los investigadores que están disponibles, sobre todo porque los nuevos paquetes están disponibles de forma regular. La próxima vez que se enfrente a un problema metodológico exigente, es posible que desee comprobar los servidores de CRAN para ver si alguien ya ha escrito un programa que proporcione lo que necesita.

8.5 Problemas de práctica
Este conjunto de problemas de práctica considera cada una de las bibliotecas de ejemplo por turno, y luego sugiere que intente usar un paquete nuevo que no se ha discutido en este capítulo. Cada pregunta requiere un conjunto de datos único.
1.
Regresión logística multinivel: revise Singh's (2015) datos sobre la participación electoral en función de las reglas de votación obligatorias y varios otros predictores. Si no tiene el archivo stdSingh.dta , descárguelo del Dataverse (consulte la página vii) o del contenido del capítulo (consulte la página 125). (Estos datos se introdujeron por primera vez en la Sección  7.4 ) Vuelva a ajustar este modelo de regresión logística mediante el comando glmer e incluya una intersección aleatoria por país-año ( cntryyear ). Recuerde que el resultado es la participación ( votada ). La severidad de las reglas de votación obligatoria ( severidad ) interactúa con los primeros cinco predictores: edad ( edad ), conocimiento político ( polinfrel ), ingresos ( ingresos ), eficacia ( eficacia ) y partidismo ( partyID ). Se deben incluir cinco predictores más solo para efectos aditivos: magnitud del distrito ( dist_magnitude ), número de partidos ( enep ), margen de victoria ( vicmarg_dist ), sistema parlamentario ( parlamentario ) y PIB per cápita ( desarrollo ). Nuevamente, todas las variables predictoras se han estandarizado. ¿Qué aprende de este modelo de regresión logística multinivel estimado con glmer que no aprende de un modelo de regresión logística agrupado estimado con glm ?

 
2.
Modelo Bayesiano de Poisson con MCMC: Determine cómo estimar un modelo de Poisson con MCMC usando MCMCpack . Recargar Peake y Eshbaugh-Soha's (2008) datos sobre la cobertura de noticias sobre política energética, discutidos por última vez en la sec. 7.3 Si no tiene el archivo PESenergy.csv , puede descargarlo del Dataverse (ver página vii) o del contenido del capítulo (ver página 125). Estimar un modelo Bayesiano de Poisson en el que el resultado es la cobertura energética ( Energía ), y las entradas son seis indicadores para los discursos presidenciales ( rmn1173 , grf0175 , grf575 , jec477 , jec1177 y jec479 ), un indicador del embargo petrolero árabe ( embargo ). , un indicador de la crisis de los rehenes de Irán ( rehenes ), el precio del petróleo ( oilc ), aprobación presidencial ( Aprobación ) y la tasa de desempleo ( Desempleo ). Utilice una prueba de Geweke para determinar si existe alguna evidencia de no convergencia. ¿Cómo debería cambiar su código en R  si la no convergencia es un problema? Resuma sus resultados en una tabla y muestre una gráfica de densidad del coeficiente parcial en el discurso de Richard Nixon de noviembre de 1973 ( rmn1173 ).

 
3.
Coincidencia exacta tosca: en el cap. 5 , los problemas de práctica introducidos por Álvarez et al. ( 2013) datos de un experimento de campo en Salta, Argentina, en el que algunos votantes emitieron su voto a través del voto electrónico y otros votaron en el entorno tradicional. Cargue la biblioteca externa y abra los datos en formato Stata. Si no tiene el archivo alpl2013.dta , puede descargarlo del Dataverse (consulte la página vii) o del contenido del capítulo (consulte la página 125). En este ejemplo, la variable de tratamiento es si el votante utilizó el voto electrónico o el voto tradicional ( EV ). Las covariables son grupo de edad ( age_group ), educación ( educ ), trabajador de cuello blanco ( white_collar ), no un trabajador de tiempo completo ( not_full_time ), hombre ( hombre), una variable de conteo para el número de seis posibles dispositivos tecnológicos utilizados ( tecnología ) y una escala ordinal para el conocimiento político ( pol_info ). Utilice la biblioteca cem para responder lo siguiente:
un.
¿Cuán equilibradas están las observaciones de tratamiento y control en los datos brutos?

 
B.
Realice una coincidencia exacta aproximada con el comando cem . ¿Cuánto ha mejorado el equilibrio como resultado?

 
C.
Considere tres posibles variables de respuesta: si el votante evaluó positivamente la experiencia de votación ( eval_voting ), si el votante evaluó la velocidad de la votación como rápida ( velocidad ) y si el votante está seguro de que su voto está siendo contado ( sure_counted ). ¿Cuál es el efecto del tratamiento promedio en los tratados (ATT) en su conjunto de datos emparejados para cada una de estas tres respuestas?

 
D.
¿En qué se diferencian sus estimaciones de los efectos promedio del tratamiento en los tratados de las pruebas simples de diferencia de medias?

 
 
4.
W-NOMINATE: De vuelta en la Secta. 2.1 , presentamos los datos del pase de lista de Lewis y Poole para el 113º Senado de los Estados Unidos. Consulte el código allí para leer estos datos, que están en formato de ancho fijo. El nombre del archivo es sen113kh.ord y está disponible en Dataverse (consulte la página vii) y el contenido del capítulo (consulte la página 125).  
un.
Formatee los datos como una matriz y cree lo siguiente: una matriz separada solo de los 657 pases de lista, un vector de los números de identificación de ICPSR y una matriz de las variables que no pasan de lista. Utilice todos estos para crear un objeto de clase rollcall . Las votaciones nominales se codifican de la siguiente manera: 1 = Sí, 6 = No, 7 y 9 = falta y 0 = no es miembro.

 
B.
Estime un modelo W-NOMINATE bidimensional para este objeto de lista. Del resumen de sus resultados, informe lo siguiente: ¿Cuántos legisladores fueron eliminados? ¿Cuántos votos se eliminaron? ¿Fue la clasificación general correcta?

 
C.
Examine la gráfica de salida de su modelo estimado, incluidas las coordenadas W-NOMINATE y la gráfica de la pantalla. Con base en el diagrama de pantalla, ¿cuántas dimensiones cree que son suficientes para caracterizar el comportamiento de votación en el 113 ° Senado? ¿Por qué?

 
 
5.
Bonificación: intente aprender a usar un paquete que nunca antes haya usado. Instale el paquete Amelia , que realiza una imputación múltiple de los datos faltantes. Eche un vistazo a Honaker et al. (2011) Artículo en el Journal of Statistical Software para tener una idea de la lógica de imputación múltiple y para aprender cómo hacer esto en R . Ajuste un modelo lineal en conjuntos de datos imputados utilizando los datos de comercio libre de la biblioteca Amelia . ¿Qué encuentras?

 
Notas al pie
1 .
Tenga en cuenta que este enfoque multinivel de datos de panel es más sensato para paneles cortos como estos, donde hay muchos individuos en relación con el número de puntos de tiempo. Para paneles largos en los que hay muchos puntos de tiempo en relación con el número de individuos, los modelos más apropiados se describen como métodos de sección transversal de series de tiempo agrupadas. Para obtener más información sobre el estudio de paneles cortos, consulte Monogan (2011) y Fitzmaurice et al. (2004).

2 .
Si no tiene estos datos de antes, puede descargar el archivo BPchap7.dta del Dataverse en la página vii o el contenido del capítulo en la página 125.

3 .
Consulte también la biblioteca nlme , que fue un predecesor de lme4 .

4 .
Si no tiene estos datos de antes, puede descargar el archivo SinghJTP.dta del Dataverse en la página vii o el contenido del capítulo en la página 125.

5 .
Para los priores de los coeficientes, la opción b0 establece el vector de medias de un anterior gaussiano multivariado y B0 establece la matriz de varianza-covarianza del anterior gaussiano multivariante. La distribución previa de la varianza del error de la regresión es Gamma inversa, y esta distribución puede manipularse estableciendo su parámetro de forma con la opción c0 y el parámetro de escala con la opción d0 . Alternativamente, la distribución Gamma inversa se puede manipular cambiando su media con la opción sigma.mu y su varianza con la opción sigma.var .

6 .
Para escribir una tabla similar a la Tabla  8.2 en LaTeX, cargue la biblioteca xtable en R  y escriba lo siguiente en la consola:

xtable (cbind (resumen (mcmc.horas) $ estadísticas [, 1: 2], resumen (mcmc.horas) $ cuantiles [, c (1,5)]), dígitos = 4)

7 .
Esto ocurre con frecuencia cuando un paquete depende del código de otro.

8 .
Los datos de LaLonde también están disponibles en el archivo LL.csv , disponible en el Dataverse (ver página vii) o el contenido del capítulo (ver página 125).

9 .
Los datos de la ONU también están disponibles en el archivo UN.csv , disponible en el Dataverse (ver página vii) o el contenido del capítulo (ver página 125).

10 .
Al elegir cuántas dimensiones incluir en un modelo de medición, muchos académicos usan la "regla del codo", lo que significa que no incluyen ninguna dimensión más allá de un codo visual en el diagrama de la pantalla. En este caso, un erudito ciertamente no incluiría más de tres dimensiones y podría contentarse con dos. Otro límite común es incluir cualquier dimensión para la cual el valor propio exceda 1, lo que nos haría detenernos en dos dimensiones.

Material suplementario
318886_1_En_8_MOESM1_ESM.zip (2.5 mb)
Dataverse (2,154 KB)
Referencias
Alvarez RM, Levin I, Pomares J, Leiras M (2013) Votar de forma segura y sencilla: el impacto del voto electrónico en la percepción ciudadana. Polit Sci Res Methods 1 (1): 117-137
CrossRefGoogle Académico
Bates D, Maechler M, Bolker B, Walker S (2014) lme4 : modelos lineales de efectos mixtos que utilizan Eigen y S4. Versión del paquete R 1.1-7. http://www.CRAN.R-project.org/package=lme4
Berkman M, Plutzer E (2010) Evolución, creacionismo y la batalla por controlar las aulas de Estados Unidos. Cambridge University Press, Nueva York
CrossRefGoogle Académico
Black D (1958) La teoría de los comités y las elecciones. Cambridge University Press, Londres
zbMATHGoogle Académico
Carlin BP, Louis TA (2009) Métodos bayesianos para el análisis de datos. Chapman & Hall / CRC, Boca Ratón, FL
zbMATHGoogle Académico
Downs A (1957) Una teoría económica de la democracia. Harper and Row, Nueva York
Google Académico
Fitzmaurice GM, Laird NM, Ware JH (2004) Análisis longitudinal aplicado. Wiley-Interscience, Hoboken, Nueva Jersey
zbMATHGoogle Académico
Gelman A, Hill J (2007) Análisis de datos utilizando modelos de regresión y multinivel / jerárquicos. Cambridge University Press, Nueva York
Google Académico
Gelman A, Carlin JB, Stern HS, Rubin DB (2004) Análisis de datos bayesianos, 2ª ed. Chapman & Hall / CRC, Boca Ratón, FL
zbMATHGoogle Académico
Gill J (2008) Métodos bayesianos: un enfoque de las ciencias sociales y del comportamiento, 2ª ed. Chapman & Hall / CRC, Boca Ratón, FL
zbMATHGoogle Académico
Honaker J, King G, Blackwell M (2011) Amelia II: un programa para datos faltantes. J Stat Softw 45 (7): 1–47
CrossRefGoogle Académico
Hotelling H (1929) Estabilidad en competencia. Econ J 39 (153): 41–57
CrossRefGoogle Académico
Iacus SM, King G, Porro G (2009) cem : software para emparejamiento exacto aproximado. J Stat Softw 30 (9): 1–27
CrossRefGoogle Académico
Iacus SM, King G, Porro G (2011) Métodos de emparejamiento multivariante que limitan el desequilibrio monótono. J Am Stat Assoc 106 (493): 345–361
CrossRefMathSciNetzbMATHGoogle Académico
Iacus SM, King G, Porro G (2012) Inferencia causal sin verificación de saldo: coincidencia exacta aproximada. Polit Anal 20 (1): 1–24
CrossRefGoogle Académico
Imai K, van Dyk DA (2004) Inferencia causal con regímenes de tratamiento general: generalización de la puntuación de propensión. J Am Stat Assoc 99 (467): 854–866
CrossRefMathSciNetzbMATHGoogle Académico
Laird NM, Fitzmaurice GM (2013) Modelado de datos longitudinal. En: Scott MA, Simonoff JS, Marx BD (eds) El manual Sage de modelado multinivel. Sage, Thousand Oaks, CA
Google Académico
LaLonde RJ (1986) Evaluación de las evaluaciones econométricas de programas de formación con datos experimentales. Am Econ Rev 76 (4): 604–620
Google Académico
Martin AD, Quinn KM, Park JH (2011) MCMCpack : Markov chain Monte Carlo en R. J Stat Softw 42 (9): 1–21
CrossRefGoogle Académico
McCarty NM, Poole KT, Rosenthal H (1997) Redistribución de la renta y realineamiento de la política estadounidense. Estudios del instituto empresarial estadounidense sobre la comprensión de la desigualdad económica. AEI Press, Washington, DC
Google Académico
Monogan JE III (2011) Análisis de datos de panel. En: Badie B, Berg-Schlosser D, Morlino L (eds) Enciclopedia internacional de ciencia política. Sage, Thousand Oaks, CA
Google Académico
Peake JS, Eshbaugh-Soha M (2008) El impacto en el establecimiento de la agenda de los principales discursos televisivos presidenciales. Polit Commun 25: 113-137
CrossRefGoogle Académico
Poole KT, Rosenthal H (1997) Congreso: una historia político-económica de la votación nominal. Oxford University Press, Nueva York
Google Académico
Poole KT, Lewis J, Lo J, Carroll R (2011) Escala de votos nominales con wnominate en R. J Stat Softw 42 (14): 1–21
CrossRefGoogle Académico
Robert CP (2001) La elección bayesiana: de los fundamentos de la teoría de la decisión a la implementación computacional, 2ª ed. Springer, Nueva York
zbMATHGoogle Académico
Rubin DB (2006) Muestreo emparejado para efectos causales. Cambridge University Press, Nueva York
CrossRefzbMATHGoogle Académico
Scott MA, Simonoff JS, Marx BD (eds) (2013) El manual Sage de modelado multinivel. Sage, Thousand Oaks, CA
Google Académico
Sekhon JS, Grieve RD (2012) Un método de emparejamiento para mejorar el equilibrio de covariables en los análisis de rentabilidad. Health Econ 21 (6): 695–714
CrossRefGoogle Académico
Singh SP (2014a) Funciones de pérdida de utilidad lineales y cuadráticas en la investigación del comportamiento electoral. J Theor Polit 26 (1): 35–58
CrossRefGoogle Académico
Singh SP (2015) Voto obligatorio y cálculo de decisiones de participación. Polit Stud 63 (3): 548–568
CrossRefGoogle Académico
