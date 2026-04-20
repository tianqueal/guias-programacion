# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

Para lograr que una estructura de datos pueda almacenar cualquier tipo de elemento sin conocerlo de antemano, se recurre a los tipos más genéricos disponibles en el lenguaje. En C, esto se consigue utilizando punteros genéricos `void*`, mientras que en Java se emplea la superclase universal `Object`. Esto permite crear una única implementación de la estructura, por ejemplo un array dinámico, que es capaz de guardar referencias a cualquier dato.

En Java, al crear una clase `Contenedor` respaldada por un array de `Object`, se aprovecha el polimorfismo (upcasting automático) para insertar cualquier instancia. Todo objeto en Java hereda de `Object`, por lo que el array puede contener simultáneamente cadenas de texto, enteros encapsulados o instancias de clases personalizadas, comportándose como una colección heterogénea.

```java
public class ContenedorGen {
    private Object[] elementos;
    private int cantidad;

    public ContenedorGen(int capacidad) {
        // Se instancia un array del tipo más genérico posible
        elementos = new Object[capacidad];
        cantidad = 0;
    }

    public void add(Object elemento) {
        if (cantidad < elementos.length) {
            elementos[cantidad++] = elemento;
        }
    }

    public Object get(int indice) {
        return elementos[indice];
    }
}
```

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un paradigma que permite escribir algoritmos y estructuras de datos de forma que operen sobre tipos de datos que se especifican posteriormente. Su objetivo principal es maximizar la reutilización del código sin sacrificar la seguridad de tipos, evitando la duplicación de lógica para cada tipo de dato diferente (por ejemplo, evitar programar una lista para enteros, otra idéntica para cadenas, etc.).

El ejemplo anterior, basado en `void*` o `Object`, sí constituye una forma primitiva y básica de programación genérica, ya que cumple con el objetivo de reutilizar la misma estructura de datos para distintos tipos. Históricamente, en C y en las primeras versiones de Java, este era el único mecanismo disponible para implementar colecciones de propósito general.

Sin embargo, a esta aproximación clásica se la considera hoy en día un enfoque inseguro o incompleto. Al borrar la información del tipo real y tratar todo como un bloque de memoria (`void*`) o un objeto opaco (`Object`), el compilador pierde la capacidad de verificar qué se está almacenando o extrayendo, delegando toda la responsabilidad y el riesgo al programador durante la ejecución del programa.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal problema de utilizar `void*` o `Object` es la pérdida total del chequeo de tipos estático (en tiempo de compilación). Cuando se inserta un dato en la estructura, el compilador acepta cualquier cosa sin rechistar. Esto facilita la introducción accidental de tipos incompatibles dentro de una colección que conceptualmente debería ser homogénea (por ejemplo, meter un entero en una lista pensada para almacenar solo cadenas de texto).

El segundo gran inconveniente se manifiesta al extraer los datos de la estructura. Como el tipo de retorno es `Object` (o `void*`), es obligatorio realizar un moldeado explícito (*downcasting*) para recuperar el tipo original y poder operar con él. Esta operación resulta tediosa, ensucia el código y, lo que es más crítico, es propensa a errores. Si el programador se equivoca al adivinar el tipo almacenado, en C se producirá un comportamiento indefinido o una violación de segmento, y en Java se lanzará una excepción `ClassCastException` que interrumpirá abruptamente la ejecución del programa.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son un mecanismo moderno introducido en los lenguajes de programación para resolver los problemas de seguridad del enfoque basado en `Object` o `void*`. Consisten en variables especiales (generalmente denotadas con letras mayúsculas entre símbolos de menor y mayor, como `<T>`, `<E>` o `<K, V>`) que representan un tipo de dato desconocido en el momento de escribir la clase o el método.

En lugar de trabajar con un tipo duro y genérico como `Object`, la clase se programa utilizando el parámetro de tipo `T`. Posteriormente, cuando un programador decide instanciar la clase (por ejemplo, creando una lista), proporciona un "argumento de tipo" real (como `String` o `Integer`) que sustituye a `T`. Esto instruye al compilador sobre el tipo exacto que esa instancia concreta va a manejar, permitiéndole realizar un chequeo estricto y automático durante la compilación.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

Tanto los *generics* de Java como las *templates* de C++ permiten instanciar estructuras de datos seguras. Al especificar el tipo de dato entre corchetes angulares (`<String>` o `<std::string>`), el compilador bloquea cualquier intento de insertar un tipo diferente, garantizando la homogeneidad de la colección.

Además, al recorrer la colección o extraer elementos, el tipo devuelto es exactamente el que se especificó. Desaparece por completo la necesidad de realizar moldeados (*casts*) manuales, lo que resulta en un código más limpio, seguro y menos propenso a errores en tiempo de ejecución.

**Ejemplo en Java:**
```java
import java.util.ArrayList;
import java.util.List;

public class EjemploGenerics {
    public static void main(String[] args) {
        // Se instancia una lista que SOLO admite String
        List<String> nombres = new ArrayList<>();
        nombres.add("Ana");
        nombres.add("Carlos");
        // nombres.add(42); // Error de compilación instantáneo

        // El bucle for-each sabe automáticamente que cada elemento es un String
        for (String nombre : nombres) {
            System.out.println(nombre.toUpperCase()); // Uso seguro de métodos de String
        }
    }
}
```

**Ejemplo equivalente en C++:**
```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Se instancia un vector que SOLO admite std::string
    std::vector<std::string> nombres;
    nombres.push_back("Ana");
    nombres.push_back("Carlos");
    // nombres.push_back(42); // Error de compilación instantáneo

    // Recorrido seguro: cada elemento se trata como std::string
    for (const std::string& nombre : nombres) {
        std::cout << nombre << std::endl;
    }
    return 0;
}
```

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Los compiladores de Java y C++ abordan la programación genérica mediante estrategias radicalmente distintas. Cuando en C++ se instancia una clase plantilla (*template*) con un tipo concreto (ej. `vector<int>`), el compilador aplica la **instanciación de plantillas**. Esto significa que genera y compila una copia física de todo el código de la clase, sustituyendo el parámetro de tipo por `int`. Si luego se usa `vector<double>`, genera otra clase completamente nueva en el código máquina. Esto maximiza el rendimiento, pero puede aumentar significativamente el tamaño del ejecutable (fenómeno conocido como *code bloat*).

En contraste, Java utiliza un mecanismo llamado **borrado de tipos** (*type erasure*). Para asegurar la compatibilidad hacia atrás con versiones antiguas del lenguaje, el compilador de Java no genera múltiples clases. En su lugar, verifica que los tipos sean correctos durante la compilación y, acto seguido, elimina (borra) toda la información de los parámetros de tipo (`<T>`), sustituyéndolos internamente por `Object`. Además, inserta automáticamente los *casts* (moldeados) necesarios en los lugares donde se extraen los datos.

Por lo tanto, en Java, una `List<String>` y una `List<Integer>` comparten exactamente la misma clase y el mismo código compilado (el bytecode) en tiempo de ejecución. El polimorfismo paramétrico de Java es esencialmente una ilusión sintáctica controlada por el compilador, mientras que las plantillas de C++ son macros de generación de código a nivel de lenguaje.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Para definir una clase que opere con múltiples tipos desconocidos, se declaran múltiples parámetros de tipo en la cabecera de la clase, típicamente separados por comas (por ejemplo, `<T, U>`). Estos parámetros actúan como comodines para los tipos de los atributos internos, del constructor y de los valores de retorno de los métodos.

En el siguiente ejemplo, la clase genérica `Par` se utiliza para emular el retorno de múltiples valores desde un método. Al calcular estadísticas sobre un array de números, el método devuelve una única instancia de `Par` instanciada con el tipo `Double` para ambos parámetros. Al recuperar los valores mediante los *getters*, se obtienen directamente variables de tipo `Double` sin necesidad de moldeado.

```java
class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

public class Estadisticas {
    // Método que devuelve dos valores Double encapsulados en un Par
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        double suma = 0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;

        double sumaCuadrados = 0;
        for (double d : datos) sumaCuadrados += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        return new Par<>(media, desviacion); // Se infieren los tipos Double
    }

    public static void main(String[] args) {
        double[] array = {2.0, 4.0, 4.0, 4.0, 5.0, 5.0, 7.0, 9.0};
        Par<Double, Double> resultado = calcularMediaYDesviacion(array);
        
        // Se extraen los valores tipados correctamente sin casting
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
```

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Los métodos genéricos permiten aplicar parámetros de tipo de forma local a una función específica, independientemente de si la clase que los contiene es genérica o no. El parámetro de tipo se declara justo antes del tipo de retorno del método (por ejemplo, `<T> T seleccionaUno(...)`).

Si el método se implementa utilizando `Object`, el compilador permitirá pasar un `String` como primer argumento y un `Integer` como segundo, ya que ambos cumplen la condición de ser `Object`. Además, al invocar el método, el valor devuelto será de tipo `Object`, forzando al programador a realizar un *downcasting* arriesgado.

Por el contrario, al utilizar un método genérico con el parámetro `<T>`, se impone una restricción estructural: ambos argumentos deben unificar bajo un mismo tipo `T`. Si se intenta pasar un `String` y un `Integer`, el compilador detectará la inconsistencia y lanzará un error. Asimismo, el tipo de retorno se adapta automáticamente al tipo proporcionado, eliminando la necesidad de realizar moldes de tipo.

```java
import java.util.Random;

public class Utilidades {
    private static Random rand = new Random();

    // Versión insegura con Object
    public static Object seleccionaUnoInseguro(Object a, Object b) {
        return rand.nextBoolean() ? a : b;
    }

    // Versión segura con parámetro de tipo a nivel de método
    public static <T> T seleccionaUnoSeguro(T a, T b) {
        return rand.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        // (i) Evitar downcasting
        // Con Object: obliga a moldear y asume el riesgo
        String ganadorInseguro = (String) seleccionaUnoInseguro("Rojo", "Azul");
        // Con Generics: el compilador infiere T como String y devuelve un String
        String ganadorSeguro = seleccionaUnoSeguro("Rojo", "Azul");

        // (ii) Forzar que ambos sean del mismo tipo
        // Con Object: esto compila sin errores, mezclando peras con manzanas
        Object mezcla = seleccionaUnoInseguro("Rojo", 42);
        // Con Generics: esto provoca un ERROR DE COMPILACIÓN, protegiendo el código
        // String fallo = seleccionaUnoSeguro("Rojo", 42);
    }
}
```

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, es posible restringir los tipos que puede adoptar un parámetro genérico utilizando **límites superiores** (*bounded type parameters*). Mediante la sintaxis `<T extends ClaseBase>`, se fuerza a que el argumento de tipo suministrado sea obligatoriamente la clase `ClaseBase` o cualquiera de sus subclases. Esta restricción aporta una ventaja crucial: dentro del código genérico, el compilador sabe que `T` es, como mínimo, de tipo `ClaseBase`, lo que permite invocar los métodos definidos en dicha clase base sobre las variables de tipo `T`.

En la solución sin *generics*, las coordenadas se declaran directamente como la clase abstracta `Number` (superclase de `Integer`, `Double`, etc.). En la solución con *generics*, se emplea la restricción `<T extends Number>`. Debido al mecanismo de borrado de tipos (*type erasure*) de Java, tras la compilación, el parámetro acotado `T` no se sustituye por `Object`, sino por su límite superior, es decir, `Number`.

```java
// Solución SIN Generics (Polimorfismo clásico)
class PuntoNumber {
    private Number x, y;
    public PuntoNumber(Number x, Number y) { this.x = x; this.y = y; }
    public Number getX() { return x; }
    public Number getY() { return y; }
    
    public double distanciaA(PuntoNumber p) {
        // Uso de métodos de Number
        return Math.sqrt(Math.pow(this.x.doubleValue() - p.getX().doubleValue(), 2) +
                         Math.pow(this.y.doubleValue() - p.getY().doubleValue(), 2));
    }
}

// Solución CON Generics y restricciones (Bounded Type Parameter)
class PuntoGen<T extends Number> {
    private T x, y;
    public PuntoGen(T x, T y) { this.x = x; this.y = y; }
    public T getX() { return x; }
    public T getY() { return y; }
    
    // Solo acepta puntos que compartan el mismo tipo numérico exacto T
    public double distanciaA(PuntoGen<T> p) {
        return Math.sqrt(Math.pow(this.x.doubleValue() - p.getX().doubleValue(), 2) +
                         Math.pow(this.y.doubleValue() - p.getY().doubleValue(), 2));
    }
}
```

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

La principal diferencia entre ambas aproximaciones radica en la rigidez estructural que imponen. La solución sin *generics* (usando `Number` directamente) es extremadamente permisiva. Permite instanciar un punto mezclando un `Integer` para la coordenada X y un `Double` para la coordenada Y en el mismo objeto, ya que ambos cumplen con ser `Number`. Por el contrario, la solución con *generics* tipada como `PuntoGen<Integer>` fuerza estrictamente a que ambas coordenadas (X e Y) sean del tipo `Integer`. El parámetro `T` unifica los tipos internos de toda la instancia, impidiendo mezclas incoherentes.

Respecto al valor de retorno de los métodos de acceso (*getters*), la solución polimórfica sin *generics* siempre devuelve una referencia genérica de tipo `Number`. Si el programador necesita utilizar una funcionalidad específica del entero o del doble original, estará obligado a realizar un *downcasting*. Sin embargo, en la solución genérica instanciada como `PuntoGen<Double>`, el compilador sabe con precisión que `getX()` devuelve un `Double`. Esto elimina la necesidad de conversiones manuales y transfiere la responsabilidad de la seguridad de tipos del programador al compilador.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

```java
public interface Punto {
    public double distanciaA(Punto p);
}

public class Punto2D implements Punto {
     private final double x, y;
     public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    @Override
    public double distanciaA(Punto p) {
        if (p instanceof Punto2D) {
            Punto2D p2d = (Punto2D) p;
            return Math.sqrt(Math.pow(x - p2d.x, 2)
                    + Math.pow(y - p2d.y, 2));
        } else {
            throw new RuntimeException("p debe ser Punto 2D");
        }
    }
}
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
}
```

Para eliminar el *downcasting* y el uso frágil de `instanceof` al operar entre objetos de una misma jerarquía que necesitan interactuar entre sí (como calcular la distancia entre puntos equivalentes), se introduce un parámetro de tipo recursivo en la interfaz base. Este patrón de diseño genérico se conoce como *Curiously Recurring Template Pattern* (CRTP) adaptado a Java.

La interfaz se define como `Punto<T extends Punto<T>>`. Esto establece un contrato rígido: cualquier clase que implemente la interfaz debe pasarse a sí misma como argumento de tipo. En consecuencia, el método `distanciaA` queda tipado fuertemente para aceptar exclusivamente instancias de la misma clase exacta, permitiendo al compilador detectar incoherencias e impidiendo, por ejemplo, intentar medir la distancia entre un punto 2D y uno 3D.

```java
// La interfaz se parametriza a sí misma
public interface Punto<T extends Punto<T>> {
    public double distanciaA(T p);
}

// Punto2D amarra el parámetro T a su propio tipo (Punto2D)
public class Punto2D implements Punto<Punto2D> {
     private final double x, y;
     public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    @Override
    // El método ahora recibe explícitamente un Punto2D.
    // Adiós instanceof y adiós downcasting.
    public double distanciaA(Punto2D p2d) {
        return Math.sqrt(Math.pow(x - p2d.x, 2) + Math.pow(y - p2d.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;
    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p3d) {
        return Math.sqrt(Math.pow(x - p3d.x, 2) + Math.pow(y - p3d.y, 2) + Math.pow(z - p3d.z, 2));
    }
}
```

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

No, en Java una `List<String>` **no** es subtipo de una `List<Object>`. Los tipos genéricos en Java son **invariantes**. Esto significa que no existe ninguna relación de herencia entre las colecciones parametrizadas, aunque sí exista entre los tipos que las parametrizan. Esta decisión de diseño se tomó deliberadamente para garantizar la seguridad de tipos estática: si se permitiera asignar una lista de cadenas a una variable de lista de objetos, se podría insertar posteriormente un `Integer` en ella, corrompiendo silenciosamente la estructura y provocando un fallo en otra parte del programa.

Por el contrario, un array `String[]` **sí** es un subtipo de `Object[]`. Los arrays en Java son **covariantes** por motivos históricos (se introdujeron en la primera versión del lenguaje antes de que existieran los genéricos, y se necesitaba un mecanismo para escribir métodos de ordenación universales). Sin embargo, esta covarianza está rota en términos de seguridad: permite referenciar un array de cadenas como un array de objetos, pero si se intenta insertar un entero, el programa lanzará una excepción `ArrayStoreException` durante la ejecución.

En resumen, respecto a la relación jerárquica:
* **Invariante** (Listas genéricas): `Gen<Sub>` no es compatible con `Gen<Super>`.
* **Covariante** (Arrays): `Array<Sub>` se considera subtipo de `Array<Super>`, permitiendo paso de referencias hacia arriba, pero con riesgo en tiempo de ejecución al escribir.
* **Contravariante**: `Gen<Super>` se comporta como un subtipo de `Gen<Sub>`, un concepto útil cuando se consumen datos (ej. un comparador genérico de objetos puede usarse para comparar cadenas).

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un *wildcard* (comodín), representado por el símbolo `?`, es una herramienta de los *generics* de Java para flexibilizar la rigidez de la invarianza. Se utiliza exclusivamente en la declaración de variables o parámetros para indicar que la colección operará sobre un tipo desconocido, pero sujeto a ciertos límites. Permite establecer relaciones de covarianza y contravarianza seguras en el momento de uso (*use-site variance*).

La sintaxis `List<? extends T>` establece una relación de **covarianza** (límite superior). Acepta una lista de `T` o de cualquier subclase de `T`. El compilador garantiza que cualquier elemento leído de esta lista será al menos de tipo `T`. Sin embargo, prohíbe estrictamente insertar nuevos elementos (excepto `null`), ya que desconoce cuál es la subclase concreta que contiene la lista. Se utiliza en escenarios donde la colección actúa puramente como **Productora** de datos (solo lectura).

La sintaxis `List<? super T>` establece una relación de **contravarianza** (límite inferior). Acepta una lista de `T` o de cualquier superclase de `T`. En este caso, el compilador permite insertar de forma segura objetos de tipo `T` o sus subclases. Sin embargo, al extraer elementos, la única garantía es que devolverá un `Object`. Se utiliza en escenarios donde la colección actúa puramente como **Consumidora** de datos (escritura). Esta regla mnemotécnica se conoce habitualmente como PECS (*Producer Extends, Consumer Super*).

```java
import java.util.List;
import java.util.ArrayList;

public class EjemploWildcards {
    
    // (i) Productor: Covarianza con ? extends Number
    // Lee números, no sabemos el tipo exacto, pero todos son Number. No podemos añadir nada.
    public static double sumarLista(List<? extends Number> lista) {
        double suma = 0;
        for (Number n : lista) { // Es seguro leer
            suma += n.doubleValue();
        }
        // lista.add(3.14); // ERROR DE COMPILACIÓN: no se puede escribir
        return suma;
    }

    // (ii) Consumidor: Contravarianza con ? super Integer
    // La lista destino puede ser de Integer, Number o Object. Es seguro inyectarle Integers.
    public static void rellenarConEnteros(List<? super Integer> lista) {
        lista.add(1); // Es seguro escribir
        lista.add(2);
        lista.add(3);
        // Integer i = lista.get(0); // ERROR DE COMPILACIÓN: al leer, devuelve Object
    }
}
```
