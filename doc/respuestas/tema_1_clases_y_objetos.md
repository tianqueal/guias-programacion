# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Las cuatro características fundamentales de la programación orientada a objetos son: la **encapsulación**, **la abstracción**, la **herencia** y el **polimorfismo**. Estas características permiten estructurar el código de manera más modular, reutilizable y mantenible que en paradigmas anteriores como la programación estructurada.

- La **encapsulación** consiste en agrupar datos (atributos) y las operaciones que los manipulan (métodos) dentro de una misma unidad llamada clase, además de controlar el acceso a esos elementos mediante modificadores de visibilidad (público, privado, protegido). Esto permite ocultar los detalles internos de implementación y exponer solo las interfaces necesarias, protegiendo así la integridad de los datos. Por ejemplo, en C se pueden utilizar variables globales accesibles desde cualquier parte del código, mientras que en POO (Programación Orientada a Objetos) los atributos privados solo son accesibles mediante métodos específicos de la clase.

- La **abstracción** se refiere a la capacidad de representar conceptos del mundo real mediante clases que capturan solo las características esenciales, ignorando detalles irrelevantes para el contexto del programa. Permite trabajar con interfaces simplificadas sin necesidad de conocer la complejidad interna.

- La **herencia** permite crear nuevas clases basadas en clases existentes, reutilizando y extendiendo su comportamiento sin duplicar código. Una clase hija hereda atributos y métodos de su clase padre, pudiendo añadir nuevos o modificar los heredados.

- El **polimorfismo** posibilita que objetos de diferentes clases respondan al mismo mensaje (llamada a método) de maneras distintas, según su tipo específico. Esto se logra principalmente mediante sobrecarga de métodos (mismo nombre, diferentes parámetros) y sobrescritura de métodos heredados. Esta característica permite escribir código más flexible y genérico, donde una referencia de un tipo padre puede apuntar a instancias de diferentes clases hijas, ejecutando el comportamiento apropiado en tiempo de ejecución.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Cuatro lenguajes populares que soportan programación orientada a objetos son Java, Python, C++ y C#. Java es un lenguaje puramente orientado a objetos donde prácticamente todo debe estar dentro de una clase, mientras que Python es multi-paradigma y permite combinar POO con programación funcional y procedural de manera flexible. C++ extiende el lenguaje C añadiendo características de POO como clases, herencia y polimorfismo, manteniendo compatibilidad con código C tradicional. Por su parte, C# es un lenguaje desarrollado por Microsoft que combina lo mejor de Java y C++, siendo fuertemente orientado a objetos y muy utilizado en el desarrollo de aplicaciones empresariales y videojuegos.

Estos lenguajes difieren en su enfoque hacia la POO. Java y C# son lenguajes con POO más estricta donde todo el código debe organizarse en clases, mientras que Python y C++ son más flexibles y permiten mezclar estilos de programación. Además, otros lenguajes populares con soporte POO incluyen JavaScript (su *superset*, TypeScript, es más estricto), Ruby, Swift (para desarrollo en sistemas Apple) y Kotlin (moderno lenguaje para Android y multiplataforma).

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La **programación estructurada** es un paradigma de programación que surgió en los años 60-70 para mejorar la claridad y calidad del código, basándose en el uso de tres estructuras de control fundamentales: secuencia (ejecución lineal de instrucciones), selección (condicionales como `if-else` y `switch`) y repetición (bucles como `for`, `while` y `do-while`). Este paradigma eliminó el uso del infame `goto`, que generaba código difícil de seguir conocido como "código espagueti", promoviendo en su lugar funciones y procedimientos que dividen el programa en bloques lógicos más pequeños y comprensibles. El lenguaje C es un ejemplo clásico de lenguaje estructurado, donde el código se organiza en funciones que operan sobre datos, pero sin agrupar datos y funciones en una unidad cohesiva como en POO.

La programación modular representa un paso evolutivo más allá de la programación estructurada, enfocándose en dividir el programa en módulos o unidades independientes que encapsulan datos y funciones relacionadas. Cada módulo debe tener una interfaz bien definida (funciones públicas) y ocultar sus detalles de implementación interna (funciones y variables privadas), permitiendo que los módulos sean desarrollados, probados y mantenidos de forma independiente. En C, esto se logra mediante la combinación de archivos `.h` (cabeceras con declaraciones públicas) y archivos `.c` (implementación con detalles privados usando `static`), aunque sin las garantías de encapsulación que ofrecen las clases en POO.

Mientras que la programación estructurada se centra en el flujo de control y la organización lógica del código, la programación modular añade el concepto de separación en unidades cohesivas y la ocultación de información. La programación orientada a objetos hereda y perfecciona estos conceptos, llevando la modularidad al siguiente nivel al combinar datos y funciones en clases con mecanismos formales de encapsulación, además de añadir herencia y polimorfismo que no existen en paradigmas anteriores.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Los tres elementos fundamentales que definen a un objeto en programación orientada a objetos son su **identidad**, su **estado** y su **comportamiento**. Estos tres aspectos permiten distinguir, caracterizar y operar con objetos de manera única dentro de un programa, estableciendo las bases conceptuales de la POO.

La **identidad** es aquello que distingue inequívocamente a un objeto de cualquier otro, incluso si ambos tienen exactamente los mismos valores en sus atributos. En la práctica, la identidad suele estar representada por la dirección de memoria donde reside el objeto o por una referencia única proporcionada por el lenguaje. Por ejemplo, si se crean dos objetos `Punto` con coordenadas `(5, 10)`, ambos tienen el mismo estado pero identidades diferentes, similar a como dos variables en C ocupan posiciones de memoria distintas aunque contengan el mismo valor.

El **estado** se refiere al conjunto de valores que tienen los atributos o propiedades del objeto en un momento determinado. Este estado puede cambiar a lo largo de la vida del objeto mediante la ejecución de sus métodos. En el ejemplo anterior del `Punto`, su estado está definido por los valores de `x` e `y`, que podrían cambiar si existieran métodos para mover el punto. Esto es análogo a los campos de un `struct` en C, donde cada instancia del `struct` mantiene sus propios valores independientes.

El **comportamiento** representa las operaciones o acciones que el objeto puede realizar, implementadas mediante métodos. Estos métodos pueden consultar el estado (por ejemplo, `calculaDistanciaAOrigen`), modificarlo (como un hipotético método `mover`), o interactuar con otros objetos. Mientras que en C las funciones operan sobre datos pasados como parámetros, en POO los métodos están vinculados al objeto y operan sobre su propio estado, siendo invocados a través de la identidad del objeto (su referencia).

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una **clase** es una *plantilla* o *molde* que define la estructura y comportamiento común para un conjunto de objetos, especificando qué atributos tendrán y qué métodos podrán ejecutar. Es un concepto abstracto que describe las características generales, similar a cómo en C se define un struct que declara los campos pero no ocupa memoria por sí mismo hasta que se crea una variable de ese tipo. Por ejemplo, una clase Punto define que todo punto tendrá coordenadas `x` e `y` y un método `calculaDistanciaAOrigen`, pero la clase en sí no es un punto concreto.

Un *objeto* o *instancia* es la materialización concreta de una clase, es decir, una entidad específica creada en memoria que tiene valores concretos para sus atributos y sobre la cual se pueden invocar los métodos definidos en la clase. La relación entre clase y objeto es análoga a la que existe entre un tipo de datos (`int`, `struct Punto`) y una variable de ese tipo: la clase describe qué es, mientras que el objeto es una realización específica que ocupa memoria. Los términos "objeto" e "instancia" se usan indistintamente, aunque "instancia" enfatiza que es un ejemplar específico creado a partir de una clase mediante el proceso de **instanciación** (usando típicamente `new` en Java).

No todos los lenguajes orientados a objetos utilizan el concepto de clase. Existen lenguajes con **programación orientada a objetos basada en prototipos**, como JavaScript (antes de ES6), donde los objetos se crean directamente o clonando otros objetos existentes sin necesidad de definir una clase previa. En estos lenguajes, un objeto puede servir como prototipo para crear otros objetos similares mediante herencia prototípica. Sin embargo, los lenguajes más populares como Java, C++, C#, Python y Ruby sí utilizan clases como mecanismo fundamental de POO, siguiendo el modelo clásico de programación orientada a objetos.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

Los objetos se almacenan típicamente en el **heap** (o **montículo**), una región de memoria dinámica donde se asigna espacio en tiempo de ejecución cuando se crean mediante operadores como `new`. A diferencia de la *pila* o *stack*, donde se almacenan variables locales y parámetros de funciones con gestión automática de memoria, el heap requiere reservar explícitamente el espacio y, en algunos lenguajes, también liberarlo manualmente. Esto es similar a usar `malloc()` en C para reservar memoria dinámica, aunque en POO el operador `new` no solo reserva memoria sino que también inicializa el objeto llamando a su constructor.

No todos los lenguajes gestionan la memoria de objetos de la misma manera. En C++, los objetos pueden crearse tanto en el stack (declarándolos como variables locales normales) como en el heap (usando `new`), siendo responsabilidad del programador liberar la memoria con `delete` cuando ya no se necesite. En cambio, lenguajes como Java, C# y Python almacenan todos los objetos en el heap y las variables contienen referencias a esos objetos, no los objetos en sí mismos, de forma que no existe la posibilidad de crear objetos en el stack como ocurre en C++ o con `struct` en C.

La **recolección de basura** (o **garbage collection**) es un mecanismo automático de gestión de memoria que libera los objetos que ya no están siendo utilizados por el programa, evitando *memory leaks* (fugas de memoria). Este proceso se ejecuta periódicamente en segundo plano, identificando objetos inaccesibles (sin referencias desde el código) y recuperando su memoria automáticamente. Java, C#, Python y JavaScript incluyen recolectores de basura, mientras que C++ no lo tiene y requiere liberación manual con `delete` (o uso de punteros inteligentes), similar a cómo en C se debe usar `free()` para liberar memoria reservada con `malloc()`. La recolección de basura simplifica la programación pero puede introducir pausas impredecibles en la ejecución del programa.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un **método** es una función asociada a una clase que define el comportamiento de los objetos de esa clase, operando sobre sus atributos y proporcionando las operaciones que dichos objetos pueden realizar. A diferencia de las funciones en C que son independientes y reciben todos los datos necesarios como parámetros, los métodos están vinculados a un objeto específico y tienen acceso directo a su estado interno mediante la referencia implícita `this`. Por ejemplo, un método `calculaDistanciaAOrigen()` en la clase `Punto` opera sobre los atributos `x` e `y` del objeto desde el cual se invoca, sin necesidad de pasarlos explícitamente como parámetros.

La **sobrecarga de métodos** es la capacidad de definir múltiples métodos con el mismo nombre dentro de una clase, siempre que tengan diferentes **listas de parámetros** (diferente número, tipo o orden de parámetros). Esto permite que una misma operación conceptual se adapte a distintas situaciones de uso, mejorando la legibilidad del código. Por ejemplo, en la clase `Punto` se podría tener un método `distancia()` sin parámetros que calcule la distancia al origen, y otro método `distancia(Punto otro)` que calcule la distancia a otro punto específico, siendo el compilador quien determina cuál método invocar según los argumentos proporcionados.

La sobrecarga se resuelve en **tiempo de compilación** analizando la firma del método (nombre + tipos de parámetros), no el tipo de retorno. En C no existe sobrecarga de funciones, obligando a usar nombres diferentes como `distanciaAlOrigen()` y `distanciaEntrePuntos()`, aunque C++ sí la soporta como una de sus extensiones orientadas a objetos. Java y otros lenguajes POO modernos incluyen sobrecarga de métodos como característica estándar, permitiendo incluso sobrecargar constructores para ofrecer múltiples formas de inicializar un objeto.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

La clase `Punto` en Java representa un punto en el plano cartesiano con coordenadas `x` e `y`. El método `calculaDistanciaAOrigen` implementa la fórmula de distancia euclidiana para calcular la distancia desde el punto actual hasta el origen `(0,0)`, utilizando el teorema de Pitágoras: $\sqrt{x^{2} + y^{2}}$. Para realizar el cálculo de la raíz cuadrada se emplea el método estático `Math.sqrt()`, similar a la función `sqrt()` de la librería `math.h` en C.

```java
class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

El ejemplo de uso muestra cómo crear una instancia de la clase `Punto` mediante el operador `new`, asignar valores a sus atributos (accesibles directamente por tener visibilidad por defecto) e invocar el método para obtener la distancia. La sintaxis `objeto.metodo()` es la forma estándar de invocar métodos en POO, donde el método opera sobre el estado interno del objeto específico.

```java
public class EjemploPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3.0;
        p.y = 4.0;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia);  // Imprime: 5.0
    }
}
```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada en un programa Java es el método `main`, que debe tener la firma exacta `public static void main(String[] args)`. Este método es invocado automáticamente por la JVM (Java Virtual Machine) cuando se ejecuta el programa, de manera análoga a cómo la función `main()` en C es el punto de inicio de la ejecución. El parámetro `String[] args` representa los argumentos de línea de comandos pasados al programa, similar al `int argc, char *argv[]` de C, aunque en Java se omite el contador de argumentos ya que los arrays conocen su propia longitud mediante `.length`.

La palabra clave `static` indica que un método o atributo pertenece a la **clase en sí misma** y no a una instancia particular de la clase. Los miembros estáticos pueden ser accedidos directamente usando el nombre de la clase sin necesidad de crear un objeto, similar a las variables globales o funciones en C que existen independientemente de cualquier estructura. Por ejemplo, `Math.sqrt()` es un método estático de la clase `Math`, accesible sin crear una instancia. El método `main` debe ser estático porque la JVM necesita invocarlo antes de crear cualquier objeto del programa, evitando el problema del "huevo y la gallina" de necesitar un objeto para ejecutar código que cree objetos.

El uso de `static` **no se limita al método** `main`, sino que puede aplicarse a otros métodos y atributos según las necesidades del diseño. Los métodos estáticos son útiles para operaciones utilitarias que no dependen del estado de un objeto particular (como `Math.sqrt()`, `Math.max()`, etc.), mientras que los atributos estáticos sirven para compartir información entre todas las instancias de una clase. Por ejemplo, se podría tener un atributo `static int contadorPuntos` en la clase Punto para llevar la cuenta de cuántos puntos se han creado en total, siendo este valor compartido por todas las instancias.

La combinación `static final` se utiliza para declarar **constantes de clase**, es decir, valores que son compartidos por todas las instancias (static) y que no pueden ser modificados después de su inicialización (final). Por ejemplo, `public static final double PI = 3.14159;` define una constante compartida por toda la aplicación, análogo a `#define PI 3.14159` en C o `const double PI = 3.14159;` en C++. Las constantes `static final` suelen nombrarse en mayúsculas por convención y son accesibles mediante `NombreClase.NOMBRE_CONSTANTE`, como `Math.PI` en Java.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar y ejecutar el ejemplo de la clase `Punto` desde línea de comandos, primero se utiliza el comando `javac` (Java Compiler) para compilar el código fuente. Suponiendo que el archivo se llama `EjemploPunto.java`, se ejecutaría `javac EjemploPunto.java`, lo cual genera dos archivos `.class`: `EjemploPunto.class` y `Punto.class` (uno por cada clase definida en el archivo). Para ejecutar el programa se usa el comando `java` seguido del nombre de la clase que contiene el método `main`, sin la extensión `.class`: `java EjemploPunto`. Este proceso es similar a compilar con `gcc` en C (`gcc programa.c -o programa`) y luego ejecutar (`./programa`), aunque en Java la ejecución no genera un ejecutable nativo sino que **interpreta el bytecode**.

Java es un lenguaje **híbrido** entre **compilado e interpretado**. El código fuente `.java` se compila a un formato intermedio llamado **bytecode**, almacenado en archivos `.class`, pero este bytecode no es código máquina nativo del procesador como lo sería un ejecutable de C. En su lugar, el bytecode es un conjunto de instrucciones diseñadas para ser ejecutadas por la **Máquina Virtual de Java (JVM)**, que actúa como una capa intermedia entre el bytecode y el sistema operativo/hardware. La JVM es diferente para cada plataforma (Windows, Linux, macOS), pero el bytecode es el mismo, implementando el principio "write once, run anywhere" (escribe una vez, ejecuta en cualquier lugar).

El **bytecode** es un código intermedio portable, independiente de la arquitectura del procesador, que contiene instrucciones de más alto nivel que el ensamblador pero más bajo que Java. Los archivos `.class` contienen este bytecode junto con metadatos sobre las clases (nombres de métodos, tipos, etc.). Durante la ejecución, la JVM puede interpretar el bytecode línea por línea o, más comúnmente en implementaciones modernas, usar un compilador JIT (Just-In-Time) que traduce el bytecode a código máquina nativo en tiempo de ejecución para mejorar el rendimiento. Esto contrasta con C, donde el compilador genera directamente código máquina específico para la plataforma objetivo y no existe una máquina virtual intermediaria.

Esta arquitectura de dos fases (compilación a bytecode + ejecución en JVM) ofrece portabilidad a costa de cierta sobrecarga (overhead) de rendimiento inicial, aunque los compiladores JIT modernos pueden generar código tan eficiente como el C en muchos casos. El bytecode también permite características avanzadas como la reflexión (inspeccionar clases en tiempo de ejecución) y la carga dinámica de clases, imposibles con ejecutables nativos de C.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

El operador `new` es el responsable de crear instancias de una clase, reservando memoria en el heap para el nuevo objeto e invocando automáticamente su constructor para inicializarlo. Este proceso es análogo a `malloc()` en C combinado con la inicialización manual de los campos, pero `new` realiza ambas operaciones de forma atómica y segura. Por ejemplo, al ejecutar `Punto p = new Punto();`, se reserva espacio para los atributos `x` e `y`, se inicializan con valores por defecto (`0.0` para tipos numéricos), y se devuelve una referencia al objeto creado que se asigna a la variable `p`.

Un **constructor** es un método especial que se ejecuta automáticamente cuando se crea un objeto mediante `new`, y su propósito es inicializar el estado del objeto con valores apropiados. Los constructores tienen el mismo nombre que la clase, no tienen tipo de retorno (ni siquiera `void`), y pueden recibir parámetros para personalizar la inicialización. Si no se define ningún constructor explícitamente, Java proporciona un **constructor por defecto** sin parámetros que inicializa los atributos con valores predeterminados (`0` para números, `null` para referencias, `false` para booleanos). En C++ los constructores funcionan de manera similar, mientras que en C este concepto no existe y se requieren funciones de inicialización manuales o inicializadores de struct.

Ejemplo de la clase `Empleado` con un constructor que recibe DNI, nombre y apellidos como parámetros. El constructor utiliza la referencia `this` para diferenciar los parámetros de los atributos cuando tienen el mismo nombre, aunque podría omitirse si se usan nombres diferentes. Es posible definir múltiples constructores mediante sobrecarga, por ejemplo, uno sin parámetros para crear empleados "vacíos" y otro con parámetros para inicializarlos completamente.

```java
class Empleado {
        String dni;
        String nombre;
        String apellidos;

        // Constructor con parámetros
        Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }

    // Ejemplo de uso:
    // Empleado emp = new Empleado("12345678A", "Juan", "García López");
}
```

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia `this` es una variable implícita disponible dentro de los métodos no estáticos de una clase, que apunta al objeto actual sobre el cual se está ejecutando el método. Esta referencia permite acceder explícitamente a los atributos y métodos del objeto desde dentro de la propia clase, siendo especialmente útil cuando existe ambigüedad entre nombres de parámetros y nombres de atributos. Es conceptualmente similar a pasar el puntero a una estructura como primer parámetro en funciones de C que operan sobre structs, pero en POO este paso es implícito y automático.

La referencia `this` no se llama igual en todos los lenguajes orientados a objetos. En Java, C++, C# y JavaScript se utiliza `this`, mientras que en Python se usa explícitamente `self` como primer parámetro de los métodos (aunque puede llamarse de otra manera, `self` es la convención universal). En otros lenguajes como Ruby se usa `self`, y en algunos lenguajes más antiguos como Smalltalk se usa `self` también. La diferencia principal es que en Python `self` debe declararse explícitamente como parámetro en la firma del método, mientras que en Java y C++ `this` está disponible implícitamente sin necesidad de declararlo.

Ejemplo de la clase `Punto` con un constructor que utiliza `this` para resolver la ambigüedad entre los parámetros y los atributos de instancia. Sin `this`, el compilador interpretaría `x = x;` como una asignación del parámetro a sí mismo, sin afectar al atributo de la clase. También se incluye un método `distanciaA` que usa `this` para pasar el objeto actual como parámetro a otro método, demostrando que `this` es una referencia completa al objeto.

```java
class Punto {
    double x;
    double y;

    // Constructor con uso de this para desambiguar
    Punto(double x, double y) {
        this.x = x;  // this.x es el atributo, x es el parámetro
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
        // En este caso, this es opcional pero hace el código más explícito
    }

    // Ejemplo de uso:
    // Punto p = new Punto(3.0, 4.0);
    // double dist = p.calculaDistanciaAOrigen();  // this dentro del método refiere a p
}
```

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

```java
class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

        // Nuevo método que calcula distancia a otro punto
    double distanciaA(Punto otro) {
        double deltaX = this.x - otro.x;
        double deltaY = this.y - otro.y;
        return Math.sqrt(deltaX * deltaX + deltaY * deltaY);
    }

    // Ejemplo de uso:
    // Punto p1 = new Punto(3.0, 4.0);
    // Punto p2 = new Punto(6.0, 8.0);
    // double dist = p1.distanciaA(p2);  // Distancia entre p1 y p2: 5.0
}
```

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, **todos los parámetros se pasan por valor**, pero es importante entender qué se copia exactamente. Cuando se pasa un objeto como `Punto`, lo que se copia es la **referencia** al objeto (similar a copiar un puntero en C), no el objeto completo. Esto significa que si se modifican los atributos del objeto a través del parámetro, los cambios **SÍ afectan al objeto original** fuera del método, porque tanto el parámetro como la variable original apuntan al mismo objeto en memoria. Sin embargo, si se intenta reasignar la referencia dentro del método (por ejemplo, `otro = new Punto(0, 0)`), esto **no afecta** a la variable original, solo cambia a dónde apunta la copia local de la referencia.

Cuando se pasa un tipo primitivo como `int`, se copia el **valor literal** del número, similar al comportamiento por defecto en C al pasar variables simples a funciones. Por tanto, cualquier modificación al parámetro dentro del método **no afecta** a la variable original fuera del método, ya que se está trabajando con una copia independiente. Si en C se quisiera modificar un `int` desde una función, sería necesario pasar su dirección mediante un puntero (`void func(int *num)`), pero en Java no existen punteros explícitos para tipos primitivos, por lo que no hay forma de modificar un `int` pasado como parámetro.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método `toString()` es un método especial en Java que devuelve una representación en forma de cadena de texto (`String`) de un objeto, proporcionando una forma legible de visualizar su estado. Este método pertenece a la clase `Object`, que es la clase base de todas las clases en Java, por lo que todos los objetos heredan automáticamente una versión por defecto de `toString()`. Sin embargo, la implementación por defecto solo devuelve información poco útil como el nombre de la clase seguido del código hash del objeto (por ejemplo, `Punto@15db9742`), por lo que es recomendable sobrescribirlo en cada clase para proporcionar información significativa sobre los atributos del objeto.

Este método se invoca automáticamente en varias situaciones, como cuando se concatena un objeto con un `String` usando el operador `+`, o cuando se pasa un objeto a `System.out.println()`. Por ejemplo, `System.out.println(punto)` internamente llama a `punto.toString()` para obtener la representación textual del objeto. En C no existe un concepto equivalente nativo, aunque se podría emular creando funciones que generen cadenas representando structs, pero sin la integración automática que ofrece Java. En C++ existe el concepto similar mediante la sobrecarga del operador `<<` para trabajar con streams de salida.

Existen conceptos similares en otros lenguajes orientados a objetos. En Python se usa el método `__str__()` para representación legible y `__repr__()` para representación técnica. En C# existe `ToString()` con exactamente la misma función que en Java. En JavaScript se tiene `toString()`, y en Ruby `to_s`. Todos estos métodos comparten el mismo propósito: proporcionar una representación textual del objeto para debugging, logging o visualización.

Clase `Punto` con una implementación de `toString()` que devuelve las coordenadas en un formato legible:

```java
class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    double distanciaA(Punto otro) {
        double deltaX = this.x - otro.x;
        double deltaY = this.y - otro.y;
        return Math.sqrt(deltaX * deltaX + deltaY * deltaY);
    }

    // Sobrescritura del método toString()
    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }

    // Ejemplo de uso:
    // Punto p = new Punto(3.0, 4.0);
    // System.out.println(p);  // Imprime: Punto(3.0, 4.0)
    // String texto = "El punto es: " + p;  // También invoca toString()
}
```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase se asemeja a un `struct` de C en que ambos permiten agrupar datos relacionados bajo un mismo tipo definido por el usuario, creando una estructura lógica para organizar información. Sin embargo, un `struct` en C es únicamente una agrupación pasiva de datos sin comportamiento asociado, mientras que una clase en POO integra tanto datos (atributos) como comportamiento (métodos) en una misma unidad cohesiva. En C, si se tiene un `struct Punto`, las funciones que operan sobre puntos deben definirse externamente y recibir la estructura como parámetro, mientras que en una clase los métodos están encapsulados dentro de la definición misma de la clase.

Las principales **carencias del** `struct` **de C** frente a una clase son: (1) **ausencia de métodos internos** - no puede contener funciones como miembros; (2) **falta de constructores y destructores** - la inicialización y limpieza deben hacerse manualmente mediante funciones externas o inicializadores; (3) **no hay control de acceso** - todos los miembros son públicos por defecto y no existen modificadores como `private` o `protected` para encapsular; (4) **sin soporte para herencia** - no puede heredar de otros structs ni servir como base para otros tipos; (5) **ausencia de polimorfismo** - no puede tener métodos virtuales ni sobrecarga de operadores (aunque C++ sí permite esto último); y (6) **no existe la referencia** `this` **implícita** - cada función debe recibir explícitamente el puntero a la estructura sobre la que opera.

Es interesante notar que en C++, **los** `struct` **son prácticamente idénticos a las clases**, con la única diferencia de que en un `struct` los miembros son públicos por defecto mientras que en una `class` son privados por defecto. En C++, un `struct` puede tener constructores, métodos, herencia y todas las características de POO, lo que demuestra que la distinción entre estructura de datos pasiva y claseactiva es conceptual más que sintáctica.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Para emular la clase `Punto` en C, se define un `struct` con los atributos `x` e `y`, y se crean funciones separadas que reciben un **puntero a la estructura** como primer parámetro, simulando así los métodos de la clase. Este puntero explícito es precisamente lo que se oculta detrás del `this` implícito en Java: cuando se invoca `punto.calculaDistanciaAOrigen()` en Java, internamente se está pasando la referencia `punto` como parámetro oculto al método. En C esta mecánica se hace explícita, revelando que la "magia" de POO es básicamente azúcar sintáctico que hace el código más legible y estructurado.

El `this` **de Java se convierte en un puntero explícito** en C, tradicionalmente llamado `this` o `self` por convención, aunque puede tener cualquier nombre. Este puntero permite acceder a los miembros de la estructura usando el operador `->` en lugar del `.` que se usa con variables locales de tipo struct. La principal diferencia es que en C el programador debe recordar pasar explícitamente el puntero como primer argumento en cada llamada (`Punto_calculaDistanciaAOrigen(&p)` en lugar de `p.calculaDistanciaAOrigen()`), mientras que en Java este paso es automático e implícito, lo que reduce errores y mejora la legibilidad.

Ejemplo de la emulación completa en C. Se utiliza la convención de prefijo `Punto_` en los nombres de funciones para simular el "namespace" que proporciona la clase en Java, evitando conflictos de nombres con funciones de otras "clases" emuladas. Este patrón era común en bibliotecas de C antes de que C++ popularizara las clases.

```c
#include <stdio.h>
#include <math.h>

// Definición del "struct" que equivale a la clase
struct Punto {
    double x;
    double y;
};

// Constructor emulado: función que inicializa un Punto
void Punto_init(struct Punto *this, double x, double y) {
    this->x = x;
    this->y = y;
}

// Método emulado: función que recibe el puntero como primer parámetro
double Punto_calculaDistanciaAOrigen(struct Punto *this) {
    return sqrt(this->x * this->x + this->y * this->y);
}

// Método emulado: distancia entre dos puntos
double Punto_distanciaA(struct Punto *this, struct Punto *otro) {
    double deltaX = this->x - otro->x;
    double deltaY = this->y - otro->y;
    return sqrt(deltaX * deltaX + deltaY * deltaY);
}

// toString emulado: función que "imprime" el punto
void Punto_toString(struct Punto *this) {
    printf("Punto(%.1f, %.1f)", this->x, this->y);
}

int main() {
    struct Punto p1, p2;

    // Inicialización (equivalente al constructor)
    Punto_init(&p1, 3.0, 4.0);
    Punto_init(&p2, 6.0, 8.0);

    // Uso de "métodos" - notar que se pasa &p1 explícitamente
    double distOrigen = Punto_calculaDistanciaAOrigen(&p1);
    printf("Distancia al origen: %.1f\n", distOrigen);  // 5.0

    double dist = Punto_distanciaA(&p1, &p2);
    printf("Distancia entre puntos: %.1f\n", dist);  // 5.0

    Punto_toString(&p1);  // Punto(3.0, 4.0)
    printf("\n");

    return 0;
}
```
