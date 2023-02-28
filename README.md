# Guía de estilo

Con el fin de reducir el esfuerzo necesario para leer y entender el código fuente, así como de mejorar la apariencia del código fuente se define un conjunto de reglas para la elección de los nombres de variables, clases, funciones, etc. Estas normas están orientadas a una uniformización del código que repercuta en una mejor legibilidad del mismo por parte de todos los desarrolladores.

## Convención de nombres ##

Como criterio general se usará el estilo CamelCase o capitalización medial ya que si el identificador está compuesto de más de una palabra al separarlas con mayúsculas aumenta la legibilidad. Existen dos tipos de CamelCase:

* **UpperCamelCase**, cuando la primera letra de cada una de las palabras es mayúscula. Ejemplo: UpperCamelCase.
* **lowerCamelCase**, igual que la anterior con la excepción de que la primera letra es minúscula. Ejemplo: lowerCamelCase.

Se procurará evitar abreviaturas en los nombres a menos que éstas identifiquen de manera inequívoca al elemento al que hacemos referencia, en cuyo caso se aconseja su uso. Por ejemplo xml, wms, etc, son abreviaturas cuyo uso está más ampliamente extendido que su forma larga. Otros casos que se puede abreviar sin pérdida de claridad son por ejemplo:

* config o cfg por configuration.
* dist por distance.

## Variables y datos miembro ##

Se deben evitar los nombres de variables de un solo carácter excepto para las variables temporales que se limitan a una zona muy concreta del código como bucles *for*. Ejemplos comunes de este tipo de variables son *i*, *j*, *k*, *m*, *n*, *x*, *y* o *z*. Aún así siempre es preferible usar algún identificador que aporte más claridad ya que podemos tener varias estructuras de control anidadas. En este caso si son válidos e incluso recomendables los identificadores cortos:

## Clases ##

Los nombres de las clases irán en formato **UpperCamelCase**.

## Métodos miembro ##

Los métodos miembro deben ser verbos (sólos o acompañados de una o varias palabras) en estilo **lowerCamelCase**. Es decir que deben ser de la forma:

```cpp
run();
close();
open();
readXml();
writeXml();
```

Encontramos tres casos especiales en los cuales se utilizarán unos prefijos preestablecidos:

1. A los que se les pase algún parámetro utilizarán el prefijo set: *void setParameters( int param1, double param2 );*
1. Los que devuelvan algún valor utilizarán el prefijo get: *int getValue( );* o simplemente *int value();*
1. Los métodos que comprueban una condición o un valor booleano se  precederán del prefijo is: *bool isBoolean();*

En casos de funciones que realicen algún tipo de conversión se empleará como separador To en lugar del también frecuentemente utilizado 2. Es decir se usará:

```cpp
convertFloatToDoble();
```
 
en lugar de:

```cpp
convertFloat2Doble(); 
```

## Constantes ##

Los nombres de las constantes se deben escribir en mayúsculas separadas por guiones bajos.

```cpp
const char *LOG_FILE = “[nombre_fichero_log]”; 
```

# Estructura de los archivos de cabecera #

En los archivos de cabecera se añadirá la declaración de las clases, de las estructuras, funciones, enumeraciones y constantes.

## Include guards ##

Los archivos de cabecera (.h) comenzarán siempre con un *include guards* o *guardián de inclusión múltiple*, que es algo como: 

```cpp
#ifndef PREFIX_FILENAME_H
#define PREFIX_FILENAME_H
```

Y terminarán cerrando con *#endif* y con el nombre de la macro comentado para saber que bloque estamos cerrando:

```cpp
#endif // PREFIX_FILENAME_H
```

Un *include guards* no es más que una forma de evitar nuevas definiciones si un archivo de cabecera se incluye mas de una vez. La primera vez que se incluye la macro *PREFIX_FILENAME_H* no está definida aún y por tanto el preprocesador entra en el bloque de código definiendo la macro en la siguiente línea. La siguiente vez que se incluya el archivo de cabecera, como ya existe dicha macro, el preprocesador salta hasta al **#endif** ignorando todo el código encerrado en el bloque.

La alternativa **recomendable** es usar **#pragma once**. Sirve para indicar al compilador que el fichero, en que está incluida dicha directiva, debe ser incluido solo una vez en cada compilación. Se añadirá al principio de cada cabecera.

```cpp
#pragma once
```

## #include ##

La inclusión de las cabeceras se hace mediante la directiva del preprocesador **#include**. Las inclusiones se harán al comienzo del archivo justo detrás del *include guard*. Primero se añadirán las cabeceras estándar, ya sean de C++ o de C, después las de las librerías de terceros y finalmente las propias. 

```cpp
#pragma once

// std headers
#include <iostream>
#include <string>

// Local headers
#include "core/defs.h"
```

Cuando las cabeceras se incluyan con ruta relativa (core/defs.h) se utilizará siempre (/) en lugar de (\). En Visual Studio la última opción es la que reconoce (nos muestra las sugerencias de los archivos que hay en esa ruta) pero esa barra da problemas en Linux y por tanto es mejor usar la otra opción.

### La diferencia entre “” y <> ###

```cpp
#include <iostream>

#include “core.h”
```

Como se puede ver en los *includes* de arriba de estas líneas se pueden incluir los archivos con dos delimitadores diferentes. En el primer caso la directiva **#include** lo que hace es buscar entre todos los directorios que se han especificado en apartado *include* del compilador. En el caso de las comillas la diferencia es que la búsqueda se realiza en primer lugar en el mismo directorio del fichero que se está compilando y posteriormente en el resto de directorios.

En el caso de nuestras propias cabeceras se utilizará la versión con comillas.

### Cabeceras estándar ###

Las cabeceras estándar de C++ van sin el .h. Las de la librería estándar de C, por razones de estandarización, se deben incluir sin el .h, y con la letra c como prefijo.

Cabecera antigua  | Nueva cabecera
----------------- | --------------
iostream.h        | iostream
string.h          | string
stdlib.h          | cstdlib
stdio.h           | cstdio

## forward declarations ##

La declaración *forward* o declaración incompleta de clases se usa para evitar tener que incluir una definición de tipo que sólo aparezca como puntero o como referencia en la cabecera.

```cpp

class Center;
 
class Circle 
{
  Center *getCenter();
};
```

# Namespaces #

Los namespaces, siempre que se usen, se definen con siglas o nombres cortos y minúsculas.
El contenido de un namespace no debe de ir tabulado.

## No usar using namespaces en las cabeceras ##

Quizás por comodidad, la gente se acostumbra a usar esta sintaxis en los archivos de cabecera, así el acceso a las clases incluidas en ese espacio de nombres es más corto y limpio.

El problema es que al incluir un using en un archivo de cabecera automáticamente se propaga a todos los archivos que tengan dependencias de dicha cabecera.

En caso de usar esta característica, añádela únicamente a los cpp, aunque mi consejo personal es no usar "using namespace" como norma general. La razón es que al perder la clase su espacio de nombres se desvirtúa el código. "std::vector" te da mucha más información que "vector" a secas, además evitas colisiones por coincidencia de nombres.


# Clases #

La estructura de una clase seguirá una serie de reglas de formato:

* La llave de apertura “{” se añade en la línea siguiente al nombre de la clase.
* Primero se añadirán los datos miembro de la clase. Dentro de estos primero irán los miembros *private*, seguidos de los *protected* y por último los *public*.
* A continuación de los datos miembro se situaran las constructoras y la destructora de la clase.
* Por último se añaden las declaraciones de los métodos miembro de la clase siguiendo el mismo orden que los datos miembro de *private*, *protected* y *public*.

Ejemplo de clase:

```cpp

#pragma once

// std headers
#include <iostream>

// OpenCV
#include "opencv2/core/core.hpp"
#include "opencv2/imgproc/imgproc.hpp"

// Local headers
#include "core/defs.h"

// Optional: not compulsory
namespace ns 
{

/*!
 * \brief Brief class description
 * 
 * Extended class description
 */
class Clazz
{

// Variables
private:

  int var;

protected:

public:

public:

  /*!
   * \brief Constuctora
   */
  Clazz(){};

  /*!
   * \brief ~Destuctora
   */
  ~Clazz(){};

// Methods
private:

public:

}; // End Clazz

} // End namespace
```

## Constructores y destructores ##

....

## Clases virtuales ##

### Destructores virtuales ### 

Es importante que los destructores de las clases que tengan métodos virtuales sean virtuales.

### Métodos y funciones ###

...

# Paso de parámetros a funciones y métodos miembro #

## Paso de parámetros por referencia ##

El paso de parámetros a una función o un método miembro de una clase se hará siempre que sea un valor de entrada con el modificador de acceso *const* y como una referencia mediante el operador de referencia(&).

```cpp

void function1(const Obj& obj);
```

Por defecto en C++ los parámetros se pasan por valor, es decir que se pasa una copia del objeto. Esto en los tipos básicos no tiene importancia pero si estamos pasando objetos mas complejos no estamos siendo eficientes ya que al hacer una copia de un objeto, a parte de ocupar espacio en memoria, se tiene que llamar a la constructora del objeto. En lugar de esto si pasamos el objeto como referencia no se crea un objeto nuevo mejorando la eficiencia del código.

Al ser un parámetro de entrada (no modificable) para asegurarnos que no se modifica internamente su valor utilizamos el modificador de acceso *const*. El pasar un objeto como *const* a una función (o declarar un objeto en su creación como constante) tiene consecuencias en cuanto a que su valor no puede ser modificado. Los métodos miembros de ese objeto que no estén declarados como const no se podrán utilizar (esto lo veremos posteriormente).