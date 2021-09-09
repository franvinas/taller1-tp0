# Contador de Palabras

Alumno: Francisco Viñas

Padrón: 103207

## Paso 0
#### a. Capturas de pantalla de la ejecución del aplicativo (con y sin Valgrind).
![Captura Hola Mundo](paso0.png)

#### b. ¿Para qué sirve Valgrind? ¿Cuáles son sus opciones más comunes?

Valgrind es una herramienta que sirve para detectar si hay memoria que fue alocada pero nunca se liberó. Al ejecutar un programa con valgrind vamos a obtener una salida con un **HEAP SUMMARY**. Esta sección de la salida de valgrind nos indica la cantidad de *allocs* y la cantidad de *frees* que se realizar durante la ejecución del programa. 

#### c. ¿Qué representa sizeof()? ¿Cuál sería el valor de salida de sizeof(char) y sizeof(int)?

La función *sizeof()* sirve para obtener la cantidad de bytes alocados para una variable o tipo de dato. La salida de esta función para un mismo tipo de dato puede variar según la arquitectura.

El valor de sizeof(char) es 1 y el de sizeof(char) es 4.

#### d. ¿El sizeof() de una struct de C es igual a la suma del sizeof() de cada uno sus elementos? Justifique mediante un ejemplo.

Esto es falso. Si al compilador no le damos ninguna instrucción indicando como queremos que sea el layout, los campos del struct estarán alineados a 4 bytes. 

Como ejemplo, veamos el struct que se muestra a continuación:

~~~c
struct ejemplo {
    char a;
    int b;
    char c;
} ejemplo_t;
~~~

La suma de los sizeof de cada campo es 1 + 4 + 1 = 6. Sin embargo, si calculamos el sizeof de ejemplo_t obtenemos el valor 12. Esto se debe a que los campos se alinean a 4 bytes para que el acceso sea más rápido.



#### e. Investigar la existencia de los archivos estándar: STDIN, STDOUT, STDERR. Explicar brevemente su uso y cómo redirigirlos en caso de ser necesario (caracteres > y <) y como conectar la salida estándar de un proceso a la entrada estándar de otro con un pipe (carácter | ).

Cuando se invoca la función main, ya se encuentran disponibles los tres canales estandar de entrada y salida. Estos son:

* **stdin**: Entrada estandar (canal por cual ingresan datos al programa, en general a través del teclado o tomando la salida de otro programa)

* **stout**: Salida estandar (canal por el cual programa devuelve los datos, en general por la pantalla)

* **stderr**: Error estandar (canal por el cual se envían los mensajes de error en caso de que exista alguno)

Es posible que un programa reciba el contenido de un archivo a través de la entrada estandar. Para esto se hace uso del caractér **<**. Por ejemplo, si tenemos un archivo entrada.txt podemos hacer que el programa lo lea como entrada estandar de la siguiente forma:

~~~
$ ./programa < entrada.txt
~~~

También podemos redireccionar la salida estandar y el error estandar. Supongamos que queremos escribir la salida en un archivo *salida.txt* y los errores en un archivo *errores.txt*. Para esto podemos hacer lo siguiente:

~~~
$ ./programa 2> errores.txt > salida.txt
~~~

Si nos interesa escribir en un archivo la salida estandar y que los errores se impriman por pantalla:

~~~
$ ./programa > salida.txt
~~~

Para escribir los errores en un archivo e imprimir por pantalla la salida estandar:

~~~
$ ./programa 2> errores.txt
~~~

Un programa puede tomar como entrada estandar a la salida estandar de otro programa haciendo uso de los pipes (caractér |). Por ejemplo, si tenemos un programa llamado programa1 que tiene una salida y queremos que esta salida sea recibida por un programa llamado programa2 como entrada estandar, luego podemos realilzar lo siguiente:

~~~
$ ./programa1 | ./programa2
~~~

## Paso 1: SERCOM - Errores de generación y normas de programación

#### a. Captura de pantalla mostrando los problemas de estilo detectados. Explicar cada uno.

~~~
/task/student//source_unsafe/paso1_wordscounter.c:27:  Missing space before ( in while(  [whitespace/parens] [5]
/task/student//source_unsafe/paso1_wordscounter.c:41:  Mismatching spaces inside () in if  [whitespace/parens] [5]
/task/student//source_unsafe/paso1_wordscounter.c:41:  Should have zero or one spaces inside ( and ) in if  [whitespace/parens] [5]
/task/student//source_unsafe/paso1_wordscounter.c:47:  An else should appear on the same line as the preceding }  [whitespace/newline] [4]
/task/student//source_unsafe/paso1_wordscounter.c:47:  If an else has a brace on one side, it should have it on both  [readability/braces] [5]
/task/student//source_unsafe/paso1_wordscounter.c:48:  Missing space before ( in if(  [whitespace/parens] [5]
/task/student//source_unsafe/paso1_wordscounter.c:53:  Extra space before last semicolon. If this should be an empty statement, use {} instead. [whitespace/semicolon] [5]
/task/student//source_unsafe/paso1_main.c:12:  Almost always, snprintf is better than strcpy  [runtime/printf] [4]
/task/student//source_unsafe/paso1_main.c:15:  An else should appear on the same line as the preceding }  [whitespace/newline] [4]
/task/student//source_unsafe/paso1_main.c:15:  If an else has a brace on one side, it should have it on both  [readability/braces] [5]
/task/student//source_unsafe/paso1_wordscounter.h:5:  Lines should be <= 80 characters long  [whitespace/line_length] [2]
Done processing /task/student//source_unsafe/paso1_wordscounter.c
Done processing /task/student//source_unsafe/paso1_main.c
Done processing /task/student//source_unsafe/paso1_wordscounter.h
Total errors found: 11
~~~

El primer error se debe a que a que no se dejó un epacio despues del while. Lo correcto hubiera sido:

~~~c
    } while (state != STATE_FINISHED);
~~~

Los siguientes dos errores (2 y 3) indican que se dejaron dos espacios despues del parentesis. Lo correcto es:

~~~c
    if (c == EOF) {
~~~

Los siguientes dos errores (4 y 5) se deben a que hay un salto de linea despues del caractér } y antes del else. Lo correcto es:

~~~c
    } else if (state == STATE_IN_WORD) {
~~~

El sexto error igual al primero pero ahora sobre un if en lugar de un while. Se soluciona agregando un espacio despues de la palabra reservada if

El último problema de estilo del archivo paso1_wordscounter.c se debe a que se dejo un espacio antes del punto y coma. Lo correcto es:

~~~c
    return next_state;
~~~

En el archivo paso1_main.c tenemos otros 3 problemas de estilo. El primero se debe a que se hace uso de la función strcpy en lugar de snprintf. Reemplazando la primera por la segunda se soluciona este problema. Los otros dos problemas dentro de este archivo se deben a que hay un salto de linea despues del caractér { y antes del else. 

El último problema de estilo se encuentra en el archivo paso1_wordscounter.h. Hay una linea que tiene más de 80 caracteres. Esta linea corresponde a comentantario, para solucionar este problema podemos reescribir el comentario de forma tal que sea más corto o usar dos lineas.

#### b. Captura de pantalla indicando los errores de generación del ejecutable. Explicar cada uno e indicar si se trata de errores del compilador o del linker.

~~~
Descomprimiendo el codigo 'source_unsafe.zip'...
Archive:  source_unsafe.zip
  inflating: source_unsafe/README.md
  inflating: source_unsafe/paso0.png
  inflating: source_unsafe/paso1_main.c
  inflating: source_unsafe/paso1_wordscounter.c
  inflating: source_unsafe/paso1_wordscounter.h
Compilando el codigo...
cc -Wall -Werror -pedantic -pedantic-errors -O3 -ggdb -DDEBUG -fno-inline -D _POSIX_C_SOURCE=200809L -Dwrapsocks=1 -std=c11 -o paso1_main.o -c paso1_main.c
paso1_main.c: In function 'main':
paso1_main.c:22:9: error: unknown type name 'wordscounter_t'
   22 |         wordscounter_t counter;
      |         ^~~~~~~~~~~~~~
paso1_main.c:23:9: error: implicit declaration of function 'wordscounter_create' [-Wimplicit-function-declaration]
   23 |         wordscounter_create(&counter);
      |         ^~~~~~~~~~~~~~~~~~~
paso1_main.c:24:9: error: implicit declaration of function 'wordscounter_process' [-Wimplicit-function-declaration]
   24 |         wordscounter_process(&counter, input);
      |         ^~~~~~~~~~~~~~~~~~~~
paso1_main.c:25:24: error: implicit declaration of function 'wordscounter_get_words' [-Wimplicit-function-declaration]
   25 |         size_t words = wordscounter_get_words(&counter);
      |                        ^~~~~~~~~~~~~~~~~~~~~~
paso1_main.c:27:9: error: implicit declaration of function 'wordscounter_destroy' [-Wimplicit-function-declaration]
   27 |         wordscounter_destroy(&counter);
      |         ^~~~~~~~~~~~~~~~~~~~
make: *** [/task/student/MakefileTP0:145: paso1_main.o] Error 1

real    0m0.040s
user    0m0.018s
sys     0m0.013s
[Error] Fallo la compilacion del codigo en 'source_unsafe.zip'. Codigo de error 2
~~~

Hay 5 errores correspondientes al proceso de compilación. Estos errores se deben a que no se hace el #include correspondiente a la librería paso1_wordscounter.h

#### c. ¿El sistema reportó algún WARNING? ¿Por qué?

El sistema no reportó ningún WARNING porque estamos usando el flag -Werror, por lo tanto todos los WARNINGS son considerados errores.

## Paso 2: SERCOM - Errores de generación 2

#### a. Describa en breves palabras las correcciones realizadas respecto de la versión anterior.

Haciendo uso del comando diff podemos ver las modificaciones realizadas:

~~~
diff paso1_main.c paso2_main.c || diff paso1_wordscounter.c paso2_wordscounter.c || diff paso1_wordscounter.h paso2_wordscounter.h
~~~

Las modificaciones fueron:

* Se incluye la libreria paso2_wordscounter.h en el archivo con el programa principal
* Se reemplaza el srtcpy por memcpy
* Se solucionan todos los problemas de estilo mencionados anteriormente

#### b. Captura de pantalla indicando la correcta ejecución de verificación de normas de programación.

~~~
Done processing /task/student//source_unsafe/paso2_wordscounter.h
Done processing /task/student//source_unsafe/paso2_main.c
Done processing /task/student//source_unsafe/paso2_wordscounter.c
~~~

#### c. Captura de pantalla indicando los errores de generación del ejecutable. Explicar cada uno e indicar si se trata de errores del compilador o del linker.

~~~
Descomprimiendo el codigo 'source_unsafe.zip'...
Archive:  source_unsafe.zip
  inflating: source_unsafe/README.md
  inflating: source_unsafe/paso0.png
  inflating: source_unsafe/paso2_main.c
  inflating: source_unsafe/paso2_wordscounter.c
  inflating: source_unsafe/paso2_wordscounter.h
Compilando el codigo...
cc -Wall -Werror -pedantic -pedantic-errors -O3 -ggdb -DDEBUG -fno-inline -D _POSIX_C_SOURCE=200809L -Dwrapsocks=1 -std=c11 -o paso2_wordscounter.o -c paso2_wordscounter.c
In file included from paso2_wordscounter.c:1:
paso2_wordscounter.h:7:5: error: unknown type name 'size_t'
    7 |     size_t words;
      |     ^~~~~~
paso2_wordscounter.h:20:1: error: unknown type name 'size_t'
   20 | size_t wordscounter_get_words(wordscounter_t *self);
      | ^~~~~~
paso2_wordscounter.h:1:1: note: 'size_t' is defined in header '<stddef.h>'; did you forget to '#include <stddef.h>'?
  +++ |+#include <stddef.h>
    1 | #ifndef __WORDSCOUNTER_H__
paso2_wordscounter.h:25:49: error: unknown type name 'FILE'
   25 | void wordscounter_process(wordscounter_t *self, FILE *text_file);
      |                                                 ^~~~
paso2_wordscounter.h:1:1: note: 'FILE' is defined in header '<stdio.h>'; did you forget to '#include <stdio.h>'?
  +++ |+#include <stdio.h>
    1 | #ifndef __WORDSCOUNTER_H__
paso2_wordscounter.c:17:8: error: conflicting types for 'wordscounter_get_words'
   17 | size_t wordscounter_get_words(wordscounter_t *self) {
      |        ^~~~~~~~~~~~~~~~~~~~~~
In file included from paso2_wordscounter.c:1:
paso2_wordscounter.h:20:8: note: previous declaration of 'wordscounter_get_words' was here
   20 | size_t wordscounter_get_words(wordscounter_t *self);
      |        ^~~~~~~~~~~~~~~~~~~~~~
paso2_wordscounter.c: In function 'wordscounter_next_state':
paso2_wordscounter.c:30:25: error: implicit declaration of function 'malloc' [-Wimplicit-function-declaration]
   30 |     char* delim_words = malloc(7 * sizeof(char));
      |                         ^~~~~~
paso2_wordscounter.c:30:25: error: incompatible implicit declaration of built-in function 'malloc' [-Werror]
paso2_wordscounter.c:5:1: note: include '<stdlib.h>' or provide a declaration of 'malloc'
    4 | #include <stdbool.h>
  +++ |+#include <stdlib.h>
    5 |
cc1: all warnings being treated as errors
make: *** [/task/student/MakefileTP0:145: paso2_wordscounter.o] Error 1

real    0m0.036s
user    0m0.024s
sys     0m0.008s
[Error] Fallo la compilacion del codigo en 'source_unsafe.zip'. Codigo de error 2
~~~

Los errores que se muestran son errores de compilación y son consecuencia de hacer uso de la función malloc y de los tipos size_t y FILE sin antes incluir las librerias que corresponden. Las librerias necesarias son:

~~~c
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>
~~~

## Paso 3: SERCOM - Errores de generación 3

#### a. Describa en breves palabras las correcciones realizadas respecto de la versión anterior.

Nuevamente, haciendo uso del comando diff podemos ver las modificaciones realizadas:

~~~
diff paso2_main.c paso3_main.c || diff paso2_wordscounter.c paso3_wordscounter.c || diff paso2_wordscounter.h paso3_wordscounter.h
~~~

Las únicas modificaciones que se aplicaron son las siguientes incluciones de librerías:

~~~c
#include <stdlib.h>
#include <string.h>
#include <stdio.h>
~~~

#### b. Captura de pantalla indicando los errores de generación del ejecutable. Explicar cada uno e indicar si se trata de errores del compilador o del linker.

~~~
Descomprimiendo el codigo 'source_unsafe.zip'...
Archive:  source_unsafe.zip
  inflating: source_unsafe/README.md
  inflating: source_unsafe/paso0.png
  inflating: source_unsafe/paso3_main.c
  inflating: source_unsafe/paso3_wordscounter.c
  inflating: source_unsafe/paso3_wordscounter.h
Compilando el codigo...
cc -Wall -Werror -pedantic -pedantic-errors -O3 -ggdb -DDEBUG -fno-inline -D _POSIX_C_SOURCE=200809L -Dwrapsocks=1 -std=c11 -o paso3_wordscounter.o -c paso3_wordscounter.c
cc -Wall -Werror -pedantic -pedantic-errors -O3 -ggdb -DDEBUG -fno-inline -D _POSIX_C_SOURCE=200809L -Dwrapsocks=1 -std=c11 -o paso3_main.o -c paso3_main.c
cc paso3_wordscounter.o paso3_main.o -o tp -lm -Wl,--wrap=send -Wl,--wrap=recv
/usr/bin/ld: paso3_main.o: in function `main':
/task/student/source_unsafe/paso3_main.c:27: undefined reference to `wordscounter_destroy'
collect2: error: ld returned 1 exit status
make: *** [/task/student/MakefileTP0:136: tp] Error 1

real    0m0.130s
user    0m0.088s
sys     0m0.036s
[Error] Fallo la compilacion del codigo en 'source_unsafe.zip'. Codigo de error 2
~~~

Es decir, el único error es el siguiente:

~~~
paso3_main.c:27: undefined reference to `wordscounter_destroy'
~~~

Este es un error del linker y se debe a que la funcion wordscounter_destroy está declarada en paso3_wordscounter.h pero no se define en ninguno de los archivos.
