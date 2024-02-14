# Guía de estilo

Con el fin de reducir el esfuerzo necesario para leer y entender el código fuente, así como de mejorar la apariencia del código fuente se define un conjunto de reglas para la elección de los nombres de variables, clases, funciones, etc. Estas normas están orientadas a una uniformización del código que repercuta en una mejor legibilidad del mismo por parte de todos los desarrolladores.

## Índice
- [Convención de nombres](#convención-de-nombres)
  + [Variables y datos miembro](#variables-y-datos-miembro)
  + [Nombre de los namespaces](#nombre-de-los-namespaces)
  + [Nombre de las clases](#nombre-de-las-clases)
  + [Métodos miembro](#métodos-miembro)
  + [Constantes](#constantes)
- [Formato](#formato)
  + [Ajuste](#ajuste)
  + [Espaciado](#espaciado)
  + [Nuevas líneas](#nuevas-líneas)
- [Jerarquías](#jerarquías)
  + [Orden de los #include](#orden-de-los-include)
  + [Clases](#clases)
- [Ejemplo](#ejemplo)
  + [Ejemplo Jerarquías](#ejemplo-jerarquías)
  + [Ejemplo estructuras de control](#ejemplo-estructuras-de-control)

# Convención de nombres

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

# Formato

El formato de código en programación se refiere a la manera en que se estructura el código fuente de un programa para que sea fácil de leer y entender por otros programadores.

## Ajuste

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

## Espaciado

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

## Nuevas líneas

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

# Jerarquías

Orden en el que se escribe el código

## Orden de los #include

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

## Clases

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

# Ejemplo

Ejemplo básico de formato.

## Ejemplo Jerarquías
```cpp
#pragma once

namespace ns
{

class Shape // Shape interface
{

public:

    enum class ShapeType : int
    {
        None = 0,
        Triangle = 1,
        Circle = 2,
        Ellipse = 3
    };

private:

  ... // Private vars

protected:

  ShapeType shapeType;

public:

  ... // Public vars

public:

    Shape() = default;
    Shape(ShapeType _shapeType);
    virtual ~Shape() = default;

private:

  ... // Private methods

protected:

  ... // Protected methods

public:

  virtual double area() const = 0;

public: // Getters and Setters

  inline void setType(ShapeType shapeType) { this->shapeType = shapeType; } // Redundant inline keyword
  ShapeType getType() const { return shapeType; }
};

template<typename T>
class Circle
  : public Shape
{

public:

    typedef T value_type;
    Point<T> center;
    T radius;

public:

    Circle();
    Circle(const Point<T> &center, T radius);
    Circle(const Circle<T> &circle);
    Circle(Circle<T> &&circle) noexcept;
    ~Circle() override = default;

    Circle<T> &operator = (const Circle<T> &circle);
    Circle<T>& operator = (Circle<T> &&circle) noexcept;

public:

    double area() const override;
    double length() const;

};

typedef Circle<int> CircleI;
using CircleD = Circle<double>; // Equivalent to: typedef Circle<double> CircleD;

}
```

## Ejemplo estructuras de control

```cpp
#include <iostream>

template <typename T>
struct Vec3 
{
    union { T x; T r; };
    union { T y; T g; };
    union { T z; T b; };

    Vec3(T _x, T _y, T _z) : x(_x), y(_y), z(_z) { }
    Vec3() = default;
    ~Vec3() = default;

    Vec3& operator+=(const Vec3& rhs)
    {
        x += rhs.x;
        y += rhs.y;
        z += rhs.z;
        return *this;
    }
};
typedef Vec3<float> Vec3f;
using Vec3d = Vec3<double>;

int main() {

    size_t length = 10;
    for (size_t i = 0; i < length; i ++) 
        std::cout << "Index: " << i << std::endl;

    bool running = true;
    while(running) {
        static int index = 0;

        std::cout << "Index: " << index << std::endl;

        if(index >= 10) running = false;
        index ++;
    }

    auto dot = [&] (const Vec3f& u, const Vec3f& v) {
      return u.x * v.x + u.y * v.y + u.z * v.z;
    };

    Vec3f u(1, 0, 0);
    Vec3f v(0, 1, 0);

    float result = dot(u, v);

    return 0;
}

```
