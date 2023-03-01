# Guía de estilo

Con el fin de reducir el esfuerzo necesario para leer y entender el código fuente, así como de mejorar la apariencia del código fuente se define un conjunto de reglas para la elección de los nombres de variables, clases, funciones, etc. Estas normas están orientadas a una uniformización del código que repercuta en una mejor legibilidad del mismo por parte de todos los desarrolladores.

## Índice
- [Estilo](#estilo)
 * [Convención de nombres](#convención-de-nombres)
  + [Variables y datos miembro](#variables-y-datos-miembro)
  + [Nombre de los namespaces](#nombre-de-los-namespaces)
  + [Nombre de las clases](#nombre-de-las-clases)
  + [Métodos miembro](#métodos-miembro)
  + [Constantes](#constantes)
 * [Formato](#formato)
  + [Ajuste](#ajuste)
  + [Espaciado](#espaciado)
  + [Nuevas líneas](#nuevas-líneas)
 * [Estructura de los archivos de cabecera](#estructura-de-los-archivos-de-cabecera)
  + [Include guards](#include-guards)
  + [#include](##include)

# Estilo

Aquí se definirán el conjunto de normas que dan forma al estilo

## Convención de nombres

Como criterio general se usará el estilo CamelCase o capitalización medial ya que si el identificador está compuesto de más de una palabra al separarlas con mayúsculas aumenta la legibilidad. Existen dos tipos de CamelCase:

* **UpperCamelCase**, cuando la primera letra de cada una de las palabras es mayúscula. Ejemplo: UpperCamelCase.
* **lowerCamelCase**, igual que la anterior con la excepción de que la primera letra es minúscula. Ejemplo: lowerCamelCase.

Se procurará evitar abreviaturas en los nombres a menos que éstas identifiquen de manera inequívoca al elemento al que hacemos referencia, en cuyo caso se aconseja su uso. Por ejemplo xml, wms, etc, son abreviaturas cuyo uso está más ampliamente extendido que su forma larga. Otros casos que se puede abreviar sin pérdida de claridad son por ejemplo:

* config o cfg por configuration.
* dist por distance.

### Variables y datos miembro

Se deben evitar los nombres de variables de un solo carácter excepto para las variables temporales que se limitan a una zona muy concreta del código como bucles *for*. Ejemplos comunes de este tipo de variables son *i*, *j*, *k*, *m*, *n*, *x*, *y* o *z*. Aún así siempre es preferible usar algún identificador que aporte más claridad ya que podemos tener varias estructuras de control anidadas. En este caso si son válidos e incluso recomendables los identificadores cortos:

### Nombre de los namespaces

Los namespaces, siempre que se usen, se definen con siglas o nombres cortos y minúsculas.

### Nombre de las clases

Los nombres de las clases irán en formato **UpperCamelCase**.

### Métodos miembro

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

### Constantes

Los nombres de las constantes se deben escribir en mayúsculas separadas por guiones bajos.

```cpp
const char *LOG_FILE = “[nombre_fichero_log]”; 
```

## Formato

El formato de código en programación se refiere a la manera en que se estructura el código fuente de un programa para que sea fácil de leer y entender por otros programadores.

### Ajuste

```cpp
void function(int a, int b)
{
    if (a < b) return true;

    if (a >= b) {
    	a += b * 5;
        return false;
    }
}
```

En condiciones y bucles, cuando se tiene una sola línea de código se omitirán las llaves y si, la línea es corta se pondrá en el mismo el bloque. Si hay más líneas, se pondrán las llaves y se escribirán en otro bloque.

### Espaciado

Configurar el IDE para que las tabulaciones se conviertan en cuatro espacios.

Espaciado para paréntesis

```cpp
void function(int a)
{
	int b = 5;
    int result = sum(a, b);
}
```

Espaciado para llaves

```cpp
void function2(int a, int b)
{
	if(a >= b) {
    	std::cout << "Error" << std::endl;
        return;
    }
    
    for(int i = a; i < b; i++) {
    	
    }
}
```

Espaciado corchetes
```cpp
int front(const std::vector<int> &numbers)
{
    return numbers[0];
}
```

### Nuevas líneas

Aplicar sangría en todos los bloques excepto en los namespaces.
En el caso de estructuras y clases se utilizarán los modificadores private, protected, public también como separadores (sin tabular).

```cpp
namespace ns
{

class Clazz
{

public:

	Clazz() 
    {
    	...
    }
    
public:

	void functionA()
    {
    	...
    }
    
}

}
```

## Jerarquías

Orden en el que se escribe el código

### Orden de los #include

En los archivos de cabecera se añadirá la declaración de las clases, de las estructuras, funciones, enumeraciones y constantes.

En la primera línea se pondrá **#pragma once** para evitar que se incluya la cabecera en múltiples ficheros.
En el caso de haber licencia, se incluirá el texto de la licencia encima del *#pragma once* con una nueva línea de separación.

La inclusión de las cabeceras se hace mediante la directiva del preprocesador **#include**. Las inclusiones se harán al comienzo del archivo justo detrás del *#pragma once*. Primero se añadirán las cabeceras estándar, ya sean de C++ o de C, después las de las librerías de terceros y finalmente las propias. 

```cpp
#pragma once

// std headers
#include <iostream>
#include <string>

// Local headers
#include "core/defs.h"
```

Cuando las cabeceras se incluyan con ruta relativa (core/defs.h) se utilizará siempre (/) en lugar de (\). En Visual Studio la última opción es la que reconoce (nos muestra las sugerencias de los archivos que hay en esa ruta) pero esa barra da problemas en Linux y por tanto es mejor usar la otra opción.

### Clases

La estructura de una clase seguirá una serie de reglas de formato:

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

  int var1;

protected:

  int var2;

public:

  int var3;

// Constructors and assignment operators
public:

  Clazz() = default; // Equivalente a Clazz() {}
  Clazz(const Clazz& clazz);
  Clazz(Clazz&& clazz) noexcept;
  ~Clazz() = default;
  
  Clazz& operator=(const Clazz& clazz);
  Clazz& operator=(Clazz&& clazz) noexcept;
  
// Methods
private:

	void function1();

public:

	void function2();
    
    Clazz& operator+=(const Clazz& clazz);
    Clazz operator+(const Clazz& clazz);

}; // End Clazz

} // End namespace
```














# Prácticas para un código limpio

### La diferencia entre “” y <>

```cpp
#include <iostream>

#include “core.h”
```

Como se puede ver en los *includes* de arriba de estas líneas se pueden incluir los archivos con dos delimitadores diferentes. En el primer caso la directiva **#include** lo que hace es buscar entre todos los directorios que se han especificado en apartado *include* del compilador. En el caso de las comillas la diferencia es que la búsqueda se realiza en primer lugar en el mismo directorio del fichero que se está compilando y posteriormente en el resto de directorios.

En el caso de nuestras propias cabeceras se utilizará la versión con comillas.

### Cabeceras estándar

Las cabeceras estándar de C++ van sin el .h. Las de la librería estándar de C, por razones de estandarización, se deben incluir sin el .h, y con la letra c como prefijo.

Cabecera antigua  | Nueva cabecera
----------------- | --------------
iostream.h        | iostream
string.h          | string
stdlib.h          | cstdlib
stdio.h           | cstdio

### No usar using namespaces en las cabeceras

Quizás por comodidad, la gente se acostumbra a usar esta sintaxis en los archivos de cabecera, así el acceso a las clases incluidas en ese espacio de nombres es más corto y limpio.

El problema es que al incluir un using en un archivo de cabecera automáticamente se propaga a todos los archivos que tengan dependencias de dicha cabecera.

En caso de usar esta característica, añádela únicamente a los cpp, aunque mi consejo personal es no usar "using namespace" como norma general. La razón es que al perder la clase su espacio de nombres se desvirtúa el código. "std::vector" te da mucha más información que "vector" a secas, además evitas colisiones por coincidencia de nombres.

### forward declarations

La declaración *forward* o declaración incompleta de clases se usa para evitar tener que incluir una definición de tipo que sólo aparezca como puntero o como referencia en la cabecera.

```cpp

class Center;
 
class Circle 
{
  Center *getCenter();
};
```

### Constructores y destructores

....

### Clases virtuales

### Destructores virtuales

Es importante que los destructores de las clases que tengan métodos virtuales sean virtuales.

### Métodos y funciones

...

### Paso de parámetros a funciones y métodos miembro

### Paso de parámetros por referencia

El paso de parámetros a una función o un método miembro de una clase se hará siempre que sea un valor de entrada con el modificador de acceso *const* y como una referencia mediante el operador de referencia(&).

```cpp

void function1(const Obj& obj);
```

Por defecto en C++ los parámetros se pasan por valor, es decir que se pasa una copia del objeto. Esto en los tipos básicos no tiene importancia pero si estamos pasando objetos mas complejos no estamos siendo eficientes ya que al hacer una copia de un objeto, a parte de ocupar espacio en memoria, se tiene que llamar a la constructora del objeto. En lugar de esto si pasamos el objeto como referencia no se crea un objeto nuevo mejorando la eficiencia del código.

Al ser un parámetro de entrada (no modificable) para asegurarnos que no se modifica internamente su valor utilizamos el modificador de acceso *const*. El pasar un objeto como *const* a una función (o declarar un objeto en su creación como constante) tiene consecuencias en cuanto a que su valor no puede ser modificado. Los métodos miembros de ese objeto que no estén declarados como const no se podrán utilizar (esto lo veremos posteriormente).