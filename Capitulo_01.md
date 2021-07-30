# **Capítulo 1: Obtener R y descargar paquetes** {#Capítulo1}

```{r, 390, eval=knitr::opts_knit$get("rmarkdown.pandoc.to") == "html", results='asis', echo=FALSE}
cat('<hr style="background-color:#03193b;height:2px">')
```

Este capítulo está escrito para el usuario que nunca ha descargado R en su computadora, mucho menos el programa. El capítulo ofrece algunos breves antecedentes sobre lo que es el programa y luego describe cómo se puede descargar e instalar R de forma totalmente gratuita. Además, el capítulo enumera algunos recursos ​​en internet sobre R que los usuarios pueden desear consultar siempre que tengan preguntas que no se abordan en este libro.

## **1.1 Antecedentes e instalación** {-}

R es una plataforma para el lenguaje de programación estadística orientado a objetos S. S fue desarrollado inicialmente por John Chambers en Bell Labs, mientras que R fue creado por Ross Ihaka y Robert Gentleman. R se utiliza ampliamente en estadísticas y se ha vuelto bastante popular en Ciencias Políticas durante la última década. El programa también se ha vuelto más utilizado en el mundo empresarial y en el trabajo gubernamental, por lo que la formación como usuario de R se ha convertido en una habilidad comercializable. R, que es shareware, es similar a S-plus, que es la plataforma comercial de S. Esencialmente, R se puede usar como un lenguaje de programación basado en matrices o como un paquete estadístico estándar que opera de manera muy similar a los programas vendidos comercialmente Stata. SAS y SPSS.

<center><img style='float: center; width:50%' src='images/imagen-1.png'/></center>
**Fig. 1.1** La página de inicio de la red de archivos comprensivos (CRAN). El cuadro superior, "Descargar e instalar R", ofrece enlaces de instalación para los tres sistemas operativos principales.

## **1.2 ¿Dónde puedo conseguir *R*?** {-}

La belleza de R es que es shareware, por lo que es gratis para cualquiera. Para obtener R para Windows, Mac o Linux, simplemente visite la completa red de archivos de R (CRAN) en [http://www.cran.r-project.org/.](http://www.cran.r- project.org/) La Figura 1.1 muestra la página de inicio de este sitio web. Como puede verse, en la parte superior de la página hay un recuadro con la etiqueta *Descargar e instalar R*. Dentro de este hay enlaces para la instalación usando los sistemas operativos Linux, Mac OS X y Windows. En el caso de Mac, al hacer clic en el enlace aparecerá un archivo descargable con el sufijo pkg que instalará la última versión. Para Windows, se presentará un enlace llamado base, que conduce a un archivo exe para descargar e instalar. En cada sistema operativo, abrir el archivo respectivo guiará al usuario a través del proceso de instalación automatizado^[Los nombres de estos archivos cambian a medida que se lanzan nuevas versiones de R. En el momento de esta impresión, los archivos respectivos son R-3.1.2-snowleopard.pkg o R-3.1.2-mavericks.pkg para varias versiones de Mac OS X y R-3.1.2-win.exe para Windows. A los usuarios de Linux les resultará más fácil de instalar desde una terminal. El código de terminal está disponible siguiendo el enlace * Descargar * R * para Linux * en la página de CRAN, luego eligiendo una distribución de Linux en la página siguiente y usando el código de terminal que aparece en la página resultante. En el momento de la impresión, se admiten Debian, varias formas de Red Hat, OpenSUSE y Ubuntu.]. En este sencillo procedimiento, un usuario puede instalar R en su máquina personal en cinco minutos.

A medida que pasan los meses y los años, los usuarios observarán el lanzamiento de nuevas versiones de R. No hay parches de actualización para R, por lo que a medida que se lanzan nuevas versiones, debe instalar completamente una nueva versión siempre que desee actualizar a la última edición. Los usuarios no necesitan reinstalar todas las versiones que se lanzan. Sin embargo, a medida que pasa el tiempo, las bibliotecas complementarias (que se describen más adelante en este capítulo) dejarán de admitir versiones anteriores de R.Una guía potencial sobre este punto es actualizar a la versión más reciente siempre que no se pueda instalar una biblioteca de interés debido a la falta. de apoyo. El único inconveniente importante que plantea la reinstalación completa es que las bibliotecas complementarias creadas por el usuario tendrán que reinstalarse, pero esto se puede hacer según sea necesario.

## **1.3 Introducción: Una primera sesión en *R*** {-}

Una vez que haya instalado R, habrá un ícono en el menú Inicio de Windows (con la opción de colocar un acceso directo en el Escritorio) o en la carpeta Aplicaciones de Mac (con la opción de mantener un ícono en el Dock del área de trabajo). Al hacer clic o hacer doble clic en el icono se iniciará R. La figura 1.2 muestra la ventana asociada con la versión Mac del software. Notará que R tiene algunas opciones de botones en la parte superior de la ventana. Dentro de Mac, la barra de menú en la parte superior del espacio de trabajo también incluirá algunos menús desplegables. En Windows, los menús desplegables también se presentarán dentro de la ventana R. Sin embargo, con solo un puñado de menús y botones, los comandos en R se ingresan principalmente a través del código de usuario. Los usuarios que deseen la opción alternativa de tener más menús y botones disponibles pueden desear instalar RStudio o un programa similar que agregue una interfaz de apuntar y hacer clic a R, pero el conocimiento de la sintaxis es esencial. La Figura 1.3 muestra la ventana asociada con la versión Mac de RStudio^[RStudio está disponible en [http://www.rstudio.com.](Http://www.rstudio.com)].

Los usuarios pueden enviar su código a través de archivos de script (la opción recomendada, descrita en la Sección 1.3) o en la línea de comando que se muestra en la parte inferior de la consola R. En la Fig. 1.2, el indicador se ve así:

<center><img style='float: center; width:50%' src='images/imagen-2.png'/></center>
**Fig. 1.2** R Console para una nueva sesión

Siempre que escriba código directamente en el símbolo del sistema, si un usuario escribe un solo comando que abarca varias líneas, el símbolo del sistema se convierte en un signo más (+) para indicar que el comando no está completo. El signo más no indica ningún problema o error, solo le recuerda al usuario que el comando anterior aún no está completo. El cursor se coloca automáticamente allí para que el usuario pueda ingresar comandos.

En la edición electrónica de este libro, la sintaxis de entrada y las impresiones de salida de R estarán codificadas por colores para ayudar a distinguir lo que el usuario debe escribir en un archivo de secuencia de comandos de los resultados esperados. El código de entrada se escribirá en <tt  style="color:blue">fuente de teletipo azul</tt>. La salida R se escribirá en <tt  style="color:black">fuente de teletipo negro</tt>. Los mensajes de error que devuelva R se escribirán en <tt  style="color:red">fuente de teletipo rojo</tt>. Estos colores corresponden a la codificación de colores que utiliza R para el texto de entrada y salida. Si bien es posible que los colores no sean visibles en la edición impresa, el texto del libro también distinguirá las entradas de las salidas. Además, los nombres de las variables se escribirán en **negrita**. Las palabras clave conceptuales de estadística y programación, así como el texto enfatizado, se escribirán en *cursiva*. Finalmente, cuando el significado de un comando no sea evidente, las iniciales identificativas estarán subrayadas y en negrita en el texto. Por ejemplo, el comando <tt>lm</tt> significa **l**inear **m**odel.

<center><img style='float: center; width:50%' src='images/imagen-3.png'/></center>
**Fig. 1.3** Ventana abierta desde una sesión de RStudio

Al escribir la sintaxis R en la línea de comando o en un archivo de script, los usuarios deben tener en cuenta algunos preliminares importantes:

- Las expresiones y comandos en R distinguen entre mayúsculas y minúsculas. Por ejemplo, la función var devuelve la varianza de una variable: una función simple discutida en el Capítulo 4. Por el contrario, el comando <tt>VAR</tt> de la biblioteca vars estima un modelo de autorregresión vectorial, una técnica avanzada que se analiza en el capítulo 9. De manera similar, si un nombre de usuario es un conjunto de datos <tt>mydata</tt>, entonces no se puede llamar con los nombres <tt>MyData</tt>, <tt>MyData</tt>, <tt>MYDATA</tt> o <tt>myData</tt>. R supondría que cada uno de estos nombres indica un significado diferente.
- Las líneas de comando no necesitan estar separadas por ningún carácter especial como un punto y coma como en Limdep, SAS o Gauss. Una simple devolución dura servirá.
- R ignora todo lo que sigue al carácter de almohadilla (#) como comentario. Esto se aplica cuando se utiliza la línea de comandos o archivos de secuencia de comandos, pero es especialmente útil al guardar notas en archivos de secuencia de comandos para su uso posterior.
- El nombre de un objeto debe comenzar con un carácter alfabético, pero puede contener caracteres numéricos a partir de entonces. Un punto también puede formar parte del nombre de un objeto. Por ejemplo, <tt>x.1</tt> es un nombre válido para un objeto en R.
- Puede utilizar las teclas de flecha del teclado para desplazarse hacia atrás a los comandos anteriores. Una pulsación de la flecha hacia arriba recupera el comando introducido anteriormente y lo coloca en la línea de comandos. Cada pulsación adicional de la flecha se mueve a un comando anterior al que aparece en la línea de comando, mientras que la flecha hacia abajo llama al comando que sigue al que aparece en la línea de comando.

Aparte de este puñado de reglas importantes, el símbolo del sistema en R tiende a comportarse de una manera intuitiva, devolviendo respuestas a los comandos de entrada que podrían adivinarse fácilmente. Por ejemplo, en su nivel más básico, R funciona como una calculadora de gama alta. Algunos de los comandos *aritméticos* clave son: suma (+), resta (-), multiplicación (\ *), división (/), exponenciación (^), la función módulo (%%) y división entera (% / %). Los paréntesis ( ) especifican el orden de las operaciones. Por ejemplo, si escribimos la siguiente entrada:

```{r, warning=F, message=F}
(3+5/78)^3*7
```

Luego, R imprime la siguiente salida:

[1] 201.3761

Como otro ejemplo, podríamos preguntarle a R cuál es el resto al dividir 89 entre 13 usando la función módulo:

```{r, warning=F, message=F}
89%%13
```

Entonces R proporciona la siguiente respuesta: 

[1] 11 

Si quisiéramos que R realizara una división de enteros, podríamos escribir:

```{r, warning=F, message=F}
89%/%13
```

Nuestra respuesta de salida a esto es: 

[1] 6

El comando de <tt>opciones</tt> permite al usuario modificar los atributos de la salida. Por ejemplo, el argumento de <tt>dígitos</tt> ofrece la opción de ajustar cuántos dígitos se muestran. Esto es útil, por ejemplo, cuando se considera la precisión con la que desea presentar los resultados que sean razonables. Otras infunciones construidas útiles de una álgebra y una trigonometría incluyen: <tt>sin(x)</tt>, <tt>cos(x)</tt>, <tt>tan(x)</tt>, <tt>exp(x)</tt>, <tt>log(x)</tt>, <tt>sqrt(x)</tt> y <tt>pi</tt>. Para aplicar algunas de estas funciones, primero podemos expandir el número de dígitos impresos y luego pedir el valor de la constante $\pi$:

```{r, warning=F, message=F}
options(digits=16)
pi
```

En consecuencia, R imprime el valor de $\pi$ hasta 16 dígitos: 

[1] 3,141592653589793

También podemos usar comandos como <tt>pi</tt> para insertar el valor de dicha constante en una función. Por ejemplo, si quisiéramos calcular el seno de un ángulo $\frac{\pi}{2}$ en radianes (o 90°), podríamos escribir:

```{r, warning=F, message=F}
sin(pi/2)
```

R imprime correctamente $\sin \left(\frac{\pi}{2}\right)=1$: 

[1] 1

## **1.4 Guardar entrada y salida** {-}

Al analizar datos o programar en R, un usuario nunca se meterá en problemas serios siempre que siga dos reglas básicas:

1. Deje siempre intactos los archivos de datos originales. Cualquier versión revisada de los datos debe escribirse en un archivo *nuevo*. Si está trabajando con un conjunto de datos particularmente grande y difícil de manejar, escriba un programa corto que se adapte a lo que necesita, guarde el archivo limpiado por separado y luego escriba el código que funcione con el nuevo archivo.
1. Escriba todo el código de entrada en una secuencia de comandos que se guarda. Los usuarios generalmente deben evitar escribir código directamente en la consola. Esto incluye código para limpiar, recodificar y remodelar datos, así como para realizar análisis o desarrollar nuevos programas.

Si se siguen estas dos reglas, entonces el usuario siempre recuperará su trabajo hasta el punto de que se produzca algún error u omisión. Entonces, incluso si, en la administración de datos, se pierde o pierde información esencial, o incluso si un revisor de una revista nombra un predictor que un modelo debe agregar, el usuario siempre puede volver sobre sus pasos. Al llamar al conjunto de datos original con el programa guardado, el usuario puede realizar ajustes *menores* en el código para incorporar una nueva función de análisis o recuperar información perdida. Por el contrario, si los datos originales se sobrescriben o el código de entrada no se guarda, entonces el usuario probablemente tendrá que iniciar el proyecto completo desde el principio, lo que es una pérdida de tiempo.

Un *archivo de script* en R es simplemente texto sin formato, generalmente guardado con el sufijo <tt>.R</tt>. Para crear un nuevo archivo de script en R, simplemente elija *Archivo $\rightarrow$ Nuevo documento* en el menú desplegable para abrir el documento. Alternativamente, la ventana de la consola que se muestra en la Fig. 1.2 muestra un ícono que parece una página en blanco en la parte superior de la pantalla (segundo ícono de la derecha). Al hacer clic en esto, también se creará un nuevo archivo de script R. Una vez abierto, se aplican los comandos normales *Guardar* y *Guardar como* del menú *Archivo*. Para abrir un script existente, seleccione *Archivo $\rightarrow$ Abrir documento* en el menú desplegable, o haga clic en el icono en la parte superior de la pantalla que parece un página con escritura (tercer icono desde la derecha en la Fig. 1.2). Cuando se trabaja con un archivo de secuencia de comandos, cualquier código dentro del archivo se puede ejecutar en la consola simplemente resaltando el código de interés y escribiendo el atajo de teclado <tt>Ctrl + R</tt> en Windows o <tt>Cmd + Return</tt> en Mac. Además del editor de archivos de script predeterminado, también están disponibles editores de texto más sofisticados como Emacs y RWinEdt.

El producto de cualquier sesión de R se guarda en el *directorio de trabajo*. El directorio de trabajo es la ruta de archivo predeterminada para todos los archivos que el usuario desea leer o escribir. El comando getwd (que significa ** get w ** orking ** d ** irectory) imprimirá el directorio de trabajo actual de R, mientras que setwd (** set w ** orking ** d ** irectory) le permite cambiar el directorio de trabajo como se desee. Dentro de una máquina con Windows, la sintaxis para verificar y luego configurar, el directorio de trabajo se vería así:

```{r, warning=F, message=F}
getwd() 
setwd("C:/temp/")
```

This now writes any output files, be they data sets, figures, or printed output to the folder temp in the C: drive. Observe that R expects forward slashes to designate subdirectories, which contrasts from Windows’s typical use of backslashes. Hence, specifying C:/temp/ as the working directory points to C:\temp\ in normal Windows syntax. Meanwhile for Mac or Unix, setting a working directory would be similar,andthepathdirectoryisprintedexactlyastheseoperatingsystemsdesignate them with forward slashes:

```{r, warning=F, message=F}
setwd("/Volumes/flashdisk/temp")
```

Note that setwd can be called multiple times in a session, as needed. Also, specifying the full path for any file overrides the working directory.

To *save output* from your session in R, try the sink command. As a general computing term, a **sink** is an output point for a program where data or results are written out. In R, this term accordingly refers to a file that records all of our printed output. To save your session’s ouput to the file Rintro.txt within the working directory type:

```{r, warning=F, message=F}
sink("Rintro.txt")
```

Alternatively, if we wanted to override the working directory, in Windows for instance, we could have instead typed:

```{r, warning=F, message=F}
sink("C:/myproject/code/Rintro.txt")
```

Now that we have created an output file, any output that normally would print to the console will instead print to the file Rintro.txt. (For this reason, in a first runofnewcode,itisusuallyadvisable toallowoutputtoprinttothescreenandthen rerun the code later to print to a file.) The print command is useful for creating output that can be easily followed. For instance, the command:

```{r, warning=F, message=F}
print("The mean of variable x is...")
```

will print the following in the file Rintro.txt: 

[1] "The mean of variable x is..."

Another useful printing command is the cat command (short for **cat**enate, to connect things together), which lets you mix objects in R with text. As a preview of simulation tools described in Chap.11, let us create a variable named x by means of simulation:

```{r, warning=F, message=F}
x <- rnorm(1000)
```

By way of explanation: this syntax draws randomly 1000 times from a standard normal distribution and assigns the values to the vector x. Observe the arrow (<-), formed with a *less than* sign and a *hyphen*, which is R’s assignment operator. Any time we assign something with the arrow (<-) the name on the left (x in this case) allows us to recall the result of the operation on the right (rnorm(1000) in this case)^[The arrow (<-) is the traditional assignment operator, though a single equals sign (=) also can serve for assignments.]. Now we can print the mean of these 1000 draws (which should be close to 0 in this case) to our output file as follows:

```{r, warning=F, message=F}
cat("The mean of variable x is...", mean(x), "\n")
```

With this syntax, objects from R can be embedded into the statement you print. The character \n puts in a carriage return. You also can print any statistical output using the either print or cat commands. Remember, your output does not go to the log file unless you use one of the print commands. Another option is to simply copy and paste results from the R console window into Word or a text editor. To turn off the sink command, simply type:

```{r, warning=F, message=F}
sink()
```

4. **Work Session Management**

A key feature of R is that it is an *object-oriented* programming language. Variables, data frames, models, and outputs are all stored in memory as *objects*, or identified (and named) locations in memory with defined features. R stores in working memory any object you create using the name you define whenever you load data into memory or estimate a model. To **l**i**s**t the objects you have created in a session use either of the following commands:

```{r, warning=F, message=F}
objects() 
ls()
```

To **r**e**m**ove all the objects in R type:

```{r, warning=F, message=F}
rm(list=ls(all=TRUE))
```

As a rule, it is a good idea to use the rm command at the start of any new program. If the previous user saved his or her workspace, then they may have used objects sharing the same name as yours, which can create confusion.

To **q**uit R either close the console window or type:

```{r, warning=F, message=F}
q()
```

At this point, R will ask if you wish to save the workspace image. Generally, it is advisable not to do this, as starting with a clean slate in each session is more likely to prevent programming errors or confusion on the versions of objects loaded in memory.

Finally, in many R sessions, we will need to load *packages*, or batches of code and data offering additional functionality not written in R’s base code. Throughout this book we will load several packages, particularly in Chap.8, where our focus will be on example packages written by prominent Political Scientists to implement cutting-edge methods. The necessary commands to load packages are install.packages, a command that automatically downloads and installs a package on a user’s copy of R, and library, a command that loads the package in a given session. Suppose we wanted to install the package MCMCpack. This package provides tools for Bayesian modeling that we will use in Chap.8. The form of the syntax for these commands is:

```{r, warning=F, message=F}
install.packages("MCMCpack") 
library(MCMCpack)
```

Package installation is case and spelling sensitive. R will likely prompt you at this point to choose one of the CRAN mirrors from which to download this package: For faster downloading, users typically choose the mirror that is most geographically proximate. The install.packages command only needs to be run once per R installation for a particular package to be available on a machine. The library command needs to be run for every session that a user wishes to use the package. Hence, in the next session that we want to use MCMCpack, we need only type: library(MCMCpack).

5. **Resources**

Given the wide array of base functions that are available in R, much less the even wider array of functionality created by R packages, a book such as this cannot possibly address everything R is capable of doing. This book should serve as a resource introducing how a researcher can use R as a basic statistics program and offer some general pointers about the usage of packages and programming features. As questions emerge about topics not covered in this space, there are several other resources that may be of use:

- Within R, the *Help* pull down menu (also available by typing help.start() in the console) offers several manuals of use, including an “Introduction to R” and “Writing R Extensions.” This also opens an HTML-based search engine of the help files.
- UCLA’s Institute for Digital Research and Education offers several nice tutorials [(http://www.ats.ucla.edu/stat/r/)](http://www.ats.ucla.edu/stat/r/). The CRAN website also includes a variety of online manuals [(http://www.cran.r-project.org/other-docs.html)](http://www.cran.r-project.org/other-docs.html).
- Some nice interactive tutorials include swirl, which is a package you install in your own copy of R (more information:[ http://www.swirlstats.com/)](http://www.swirlstats.com/), and Try R, which is completed online [(http://tryr.codeschool.com/)](http://tryr.codeschool.com/).
- Within the R console, the commands ?, help(), and help.search() all serve to find documentation. For instance, ?lm would find the documenta- tion for the linear model command. Alternatively, help.search("linear model") would search the documentation for a phrase.
6. Practice Problems 11
- To search the internet for information,Rseek [(http://www.rseek.org/,](http://www.rseek.org/) powered by Google) is a worthwhile search engine that searches only over websites focused on R.
- Finally, Twitter users reference R through the hashtag #rstats.

At this point, users should now have R installed on their machine, hold a basic sense of how commands are entered and output is generated, and recognize where to find the vast resources available for R users. In the next six chapters, we will see how R can be used to fill the role of a statistical analysis or econometrics software program.

**1.6 Practice Problems**

Each chapter will end with a few practice problems. If you have tested all of the code from the in-chapter examples, you should be able to complete these on your own. If you have not done so already, go ahead and install R on your machine for free and try the in-chapter code. Then try the following questions.

1. Compute the following in R:
1) 7  23
1) 8

82C 1

3) cpos 
3) 81
3) ln*e*4
2. Whatdoesthecommandcor do?Finddocumentationaboutitanddescribewhat the function does.
2. What does the command runif do? Find documentation about it and describe what the function does.
2. Create a vector named x that consists of 1000 draws from a standard normal distribution, using code just like you see in Sect.[1.3.](#_page6_x53.00_y53.15) Create a second vector named y in the same way. Compute the correlation coefficient between the two vectors. What result do you get, and why do you get this result?
2. Get a feel for how to decide when add-on packages might be useful for you. Log in to[ http://www.rseek.org ](http://www.rseek.org)and look up what the stringr package does. What kinds of functionality does this package give you? When might you want to use it?
