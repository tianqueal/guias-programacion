# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a una función en C es una variable que almacena la dirección de memoria de una función. Igual que un puntero normal puede apuntar a una variable, un puntero a función apunta al código ejecutable de una función concreta. Esto permite guardar funciones en variables, pasarlas como argumentos o elegir dinámicamente qué función ejecutar.

Para declarar un puntero a función se debe indicar la firma de la función: el tipo de retorno y los tipos de sus parámetros. Por ejemplo, si una función recibe un `char *` y devuelve un `char *`, el puntero debe declararse con esa misma forma.

```c
#include <stdio.h>
#include <ctype.h>

char* convertirAMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper((unsigned char) cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "hola mundo";

    char* (*aMayusculas)(char*) = convertirAMayusculas;

    printf("%s\n", aMayusculas(texto));

    return 0;
}
```

En este ejemplo, `aMayusculas` es una variable local que apunta a la función `convertirAMayusculas`. Al escribir `aMayusculas(texto)`, se invoca la función a través del puntero, obteniendo como resultado la cadena modificada en mayúsculas.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima, es decir, una función que se puede definir sin darle un nombre explícito. Normalmente se usa para escribir funciones pequeñas de forma compacta, especialmente cuando se quieren guardar en variables, pasar como parámetros o devolver como resultado de otra función.

A diferencia de C, donde se trabaja con punteros a funciones, en lenguajes como JavaScript o Java las lambdas se tratan como valores de más alto nivel. Esto permite escribir código más expresivo y cercano al paradigma funcional.

Ejemplo en JavaScript:

```javascript
const aMayusculas = cadena => cadena.toUpperCase();

console.log(aMayusculas("hola mundo"));
```

Ejemplo en Java:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
```

En Java, `Function<String, String>` representa una función que recibe un `String` y devuelve otro `String`. Para invocarla se usa el método `apply`, ya que la lambda se almacena dentro de un objeto que implementa una interfaz funcional.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es una forma de programar basada en el uso de funciones como elemento principal. En este paradigma se tiende a evitar modificar estados compartidos y se favorece transformar datos mediante funciones. Conceptos como funciones lambda, composición de funciones, inmutabilidad y funciones de orden superior son habituales en este estilo.

Java nació principalmente como un lenguaje orientado a objetos, pero desde Java 8 incorporó características funcionales, como expresiones lambda, referencias a métodos, interfaces funcionales y la API de streams. Por eso se dice que Java es un lenguaje multi-paradigma: permite programar usando orientación a objetos, pero también permite aplicar técnicas propias de la programación funcional.

Que las funciones sean “ciudadanos de primera clase” significa que pueden tratarse como cualquier otro valor del lenguaje. Es decir, pueden almacenarse en variables, pasarse como argumentos a otras funciones, devolverse como resultado y combinarse con otras funciones.

En C esto se consigue parcialmente con punteros a funciones, pero en lenguajes funcionales o multi-paradigma suele hacerse de forma más natural y segura. En Java, por ejemplo, una lambda puede almacenarse en una variable cuyo tipo sea una interfaz funcional.

## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis básica de una función lambda en Java utiliza el operador `->`. A la izquierda se escriben los parámetros de entrada y a la derecha se escribe el cuerpo de la función. La forma general es: parámetros, flecha y expresión o bloque de instrucciones.

Cuando la lambda tiene un solo parámetro, se pueden omitir los paréntesis. Si tiene cero parámetros o más de uno, los paréntesis son obligatorios. Si el cuerpo tiene una sola expresión, no es necesario escribir `return`; Java devuelve automáticamente el resultado de esa expresión.

```java
Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();
```

También se puede escribir una lambda con bloque de código. En ese caso, si se devuelve un valor, debe escribirse `return` explícitamente.

```java
Function<String, String> aMayusculas = cadena -> {
    String resultado = cadena.toUpperCase();
    return resultado;
};
```

El tipo de una lambda no existe de forma aislada: siempre debe asociarse a una interfaz funcional. Por eso, en el ejemplo anterior, la lambda se guarda en una variable de tipo `Function<String, String>`.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

Una función puede recibirse como parámetro igual que se recibe un número, una cadena o un objeto. Esto permite escribir métodos más generales, donde el comportamiento concreto se delega en la función recibida. A este tipo de funciones se les suele llamar funciones de orden superior.

En este caso, `transformar` no necesita saber exactamente qué transformación se va a realizar. Solo necesita recibir una cadena y una función capaz de transformar esa cadena en otra.

Ejemplo en JavaScript:

```javascript
function transformar(cadena, funcionTransformadora) {
    return funcionTransformadora(cadena);
}

const aMayusculas = cadena => cadena.toUpperCase();

console.log(transformar("hola mundo", aMayusculas));
```

Ejemplo en Java:

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String cadena, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(cadena);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(transformar("hola mundo", aMayusculas));
    }
}
```

En Java se usa `apply` porque `Function<T, R>` define ese método para aplicar la función. En JavaScript, en cambio, la función se invoca directamente con paréntesis.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Una función lambda puede definirse directamente en el punto donde se pasa como argumento. Esto evita crear una variable intermedia cuando la función solo se va a usar una vez. El código queda más breve y expresa claramente que esa transformación pertenece a esa llamada concreta.

En este ejemplo, se pasa a `transformar` una lambda que invierte la cadena recibida. La función se define justo dentro de la llamada.

Ejemplo en JavaScript:

```javascript
function transformar(cadena, funcionTransformadora) {
    return funcionTransformadora(cadena);
}

console.log(
    transformar("hola mundo", cadena => cadena.split("").reverse().join(""))
);
```

Ejemplo en Java:

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String cadena, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(cadena);
    }

    public static void main(String[] args) {
        System.out.println(
            transformar("hola mundo", cadena -> new StringBuilder(cadena).reverse().toString())
        );
    }
}
```

La ventaja de esta forma es que la transformación queda localizada en el lugar donde se utiliza. No obstante, si la lambda es larga o se reutiliza varias veces, suele ser más claro guardarla en una variable con un nombre significativo.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un cierre, o *closure*, aparece cuando una función lambda recuerda y puede usar variables del contexto donde fue definida. Es decir, la lambda no solo contiene su propio código, sino que también puede acceder a ciertos valores externos que estaban disponibles en el momento de su creación.

En Java, las variables locales usadas dentro de una lambda deben ser finales o efectivamente finales. Esto significa que no hace falta declararlas con `final`, pero no pueden cambiar de valor después de haber sido inicializadas. Esta restricción evita problemas derivados de modificar variables locales capturadas.

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String cadena, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(cadena);
    }

    public static void main(String[] args) {
        String sufijo = "!!!";

        Function<String, String> concatenarSufijo = cadena -> cadena + sufijo;

        System.out.println(transformar("hola mundo", concatenarSufijo));
    }
}
```

En este ejemplo, la lambda `cadena -> cadena + sufijo` accede a la variable local `sufijo`, aunque dicha variable no pertenece a la propia lambda. Esa captura de contexto es lo que se conoce como cierre.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

Un puntero a función en C almacena únicamente la dirección de una función existente. Permite invocar esa función de forma indirecta, pero no representa por sí mismo una función anónima con contexto asociado. Normalmente apunta a una función definida previamente con una firma concreta.

Una función lambda, en cambio, puede definirse directamente en el lugar donde se necesita. Además, en lenguajes como Java o JavaScript, puede capturar variables del entorno donde fue creada, formando un cierre. Esta capacidad no existe directamente en los punteros a funciones de C.

Otra diferencia importante es el nivel de abstracción. En C se trabaja cerca de la memoria y de las direcciones, por lo que el programador debe manejar con cuidado los tipos y los punteros. En Java, una lambda se asocia a una interfaz funcional y se comprueba estáticamente que encaja con el tipo esperado.

Por tanto, aunque ambos mecanismos permiten ejecutar comportamiento indirectamente, las lambdas son más expresivas. No solo apuntan a código, sino que pueden representar comportamiento definido en el momento, integrado con el sistema de tipos y capaz de conservar contexto.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

También es posible devolver funciones desde otros métodos. En este caso, el método `crearDescuento` recibe un porcentaje y devuelve una función que aplica ese porcentaje a cualquier cantidad. La función devuelta queda especializada con el descuento indicado.

Esto es un ejemplo claro de cierre, porque la lambda devuelta recuerda el valor de `porcentaje` aunque el método `crearDescuento` ya haya terminado su ejecución. Cada llamada a `crearDescuento` crea una función con su propio porcentaje capturado.

```java
import java.util.function.Function;

public class Main {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad - (cantidad * porcentaje / 100);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento25 = crearDescuento(25);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento25.apply(precio)); // 75.0
    }
}
```

En el ejemplo, `descuento10` recuerda que `porcentaje` vale `10`, mientras que `descuento25` recuerda que vale `25`. Aunque ambas funciones tienen el mismo código, cada una conserva un contexto distinto.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional en Java es una interfaz que tiene exactamente un método abstracto. Ese único método abstracto define la forma de la función: qué parámetros recibe y qué tipo devuelve. Por eso una lambda puede asignarse a una variable de ese tipo.

Por ejemplo, `Function<T, R>` es una interfaz funcional porque define un método abstracto principal llamado `apply`, que recibe un valor de tipo `T` y devuelve un valor de tipo `R`. Una lambda compatible con esa firma puede guardarse en una variable `Function<T, R>`.

Una interfaz funcional puede tener métodos `default`, métodos `static` y métodos heredados de `Object`, sin dejar de ser funcional. Lo importante es que tenga un único método abstracto propio.

Para indicar explícitamente que una interfaz está pensada como interfaz funcional, se puede usar la anotación `@FunctionalInterface`. Esta anotación no es obligatoria, pero ayuda a detectar errores en compilación si se añade accidentalmente más de un método abstracto.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Se puede crear una interfaz funcional propia cuando se quiere dar un nombre específico a un tipo de operación. En este caso, `Transformador` representa cualquier función que reciba una cadena de texto y devuelva otra cadena de texto.

La interfaz solo necesita un método abstracto. El nombre del método puede elegirse libremente, aunque conviene que sea expresivo. En este ejemplo se usa `transformar`.

```java
@FunctionalInterface
interface Transformador {
    String transformar(String cadena);
}
```

Uso de la interfaz funcional:

```java
public class Main {
    public static void main(String[] args) {
        Transformador aMayusculas = cadena -> cadena.toUpperCase();

        System.out.println(aMayusculas.transformar("hola mundo"));
    }
}
```

La lambda `cadena -> cadena.toUpperCase()` encaja con la interfaz porque recibe un `String` y devuelve un `String`. Por tanto, Java puede convertir esa lambda en una implementación de `Transformador`.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

La interfaz `Transformador` puede hacerse más reutilizable usando genericidad. En lugar de limitarse a transformar un `String` en otro `String`, se puede definir una versión genérica que transforme un valor de tipo `T` en un valor de tipo `R`.

De esta forma, la misma interfaz sirve para representar muchas operaciones diferentes: de `String` a `String`, de `Double` a `Integer`, de `Persona` a `String`, etc. El tipo concreto se indica al declarar la variable.

```java
@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T valor);
}
```

Ejemplo de uso con un transformador que redondea un `Double` y devuelve un `Integer`:

```java
public class Main {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = valor -> (int) Math.round(valor);

        System.out.println(redondear.transformar(4.7)); // 5
        System.out.println(redondear.transformar(4.3)); // 4
    }
}
```

En este caso, `T` es `Double` y `R` es `Integer`. La lambda recibe un número decimal y devuelve un entero redondeado.

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

Java incluye varias interfaces funcionales predefinidas en el paquete `java.util.function`. Estas interfaces cubren muchos casos comunes, por lo que normalmente no es necesario crear una interfaz propia si ya existe una equivalente.

Las más utilizadas son `Function`, `Consumer`, `Supplier` y `Predicate`. Cada una representa un tipo distinto de función según reciba parámetros, devuelva resultados o evalúe condiciones.

```java
import java.util.function.Function;
import java.util.function.Consumer;
import java.util.function.Supplier;
import java.util.function.Predicate;
import java.util.function.UnaryOperator;
import java.util.function.BinaryOperator;
import java.util.function.BiFunction;
import java.util.function.BiConsumer;
import java.util.function.BiPredicate;
```

Ejemplos de uso:

```java
Function<String, Integer> longitud = texto -> texto.length();

Consumer<String> mostrar = texto -> System.out.println(texto);

Supplier<Double> aleatorio = () -> Math.random();

Predicate<Integer> esPositivo = numero -> numero > 0;

UnaryOperator<String> aMayusculas = texto -> texto.toUpperCase();

BinaryOperator<Integer> sumar = (a, b) -> a + b;

BiFunction<String, String, String> unir = (a, b) -> a + b;

BiConsumer<String, Integer> repetir = (texto, veces) -> {
    for (int i = 0; i < veces; i++) {
        System.out.println(texto);
    }
};

BiPredicate<String, Integer> longitudMayorQue = (texto, limite) -> texto.length() > limite;
```

Además, existen versiones especializadas para tipos primitivos, como `IntFunction`, `IntPredicate`, `IntConsumer`, `DoubleFunction`, `DoublePredicate`, `LongSupplier`, etc. Estas evitan conversiones innecesarias entre tipos primitivos y sus clases envoltorio.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método `forEach` permite recorrer los elementos de una colección usando una función lambda. Es una alternativa más funcional al bucle `for`, porque se indica qué acción realizar con cada elemento, sin escribir explícitamente la estructura de iteración.

En una lista de enteros, se puede pasar a `forEach` una lambda que reciba cada número y compruebe si es positivo. Si lo es, se muestra un mensaje por pantalla.

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = List.of(-3, 0, 5, 8, -1, 10);

        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println(numero + " es positivo");
            }
        });
    }
}
```

En este caso, `forEach` recibe una función de tipo `Consumer`, porque consume cada elemento de la lista pero no devuelve ningún resultado. Su objetivo es producir una acción, como imprimir por pantalla.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

La firma de `forEach` usa `Consumer<? super T>` porque un consumidor de elementos de tipo `T` también puede ser un consumidor de algún supertipo de `T`. Por ejemplo, si se tiene una lista de `Integer`, también sería válido usar un `Consumer<Number>` o un `Consumer<Object>`, porque un `Integer` puede tratarse como `Number` u `Object`.

PECS significa *Producer Extends, Consumer Super*. Esta regla indica que, si una estructura produce valores, conviene usar `? extends T`; si consume valores, conviene usar `? super T`. En `forEach`, el `Consumer` consume elementos de la lista, por eso se usa `? super T`.

```java
import java.util.List;
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = List.of(1, 2, 3);

        Consumer<Number> consumidor = n -> System.out.println(n);

        numeros.forEach(consumidor);
    }
}
```

Para mejorar el método `transformar`, se puede aplicar la misma idea. La función transformadora consume un valor de entrada, por lo que para el parámetro de entrada se puede usar `? super T`. A la vez, produce un resultado, por lo que para el tipo de salida se puede usar `? extends R`.

```java
import java.util.function.Function;

public class Main {
    public static <T, R> R transformar(T valor, Function<? super T, ? extends R> funcion) {
        return funcion.apply(valor);
    }

    public static void main(String[] args) {
        String resultado = transformar("hola", texto -> texto.toUpperCase());

        System.out.println(resultado);
    }
}
```

Así, el método queda más flexible. La función puede aceptar `T` o algún supertipo de `T`, y puede devolver `R` o algún subtipo de `R`.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Una referencia a método permite guardar un método en una variable para invocarlo después. Es una forma de tratar métodos como valores, de manera parecida a como se hace con funciones lambda.

En JavaScript debe tenerse cuidado con el valor de `this`, porque al extraer un método de un objeto se puede perder el contexto. Por eso suele usarse `bind` para fijar la instancia concreta.

Ejemplo en JavaScript:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        return `Hola, soy ${this.nombre}`;
    }
}

const persona = new Persona("Ana");

const saludar = persona.saludar.bind(persona);

console.log(saludar());
```

Ejemplo en Java:

```java
import java.util.function.Supplier;

class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola, soy " + nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");

        Supplier<String> saludar = persona::saludar;

        System.out.println(saludar.get());
    }
}
```

En Java, `persona::saludar` es una referencia al método `saludar` de una instancia concreta. Como no recibe parámetros y devuelve un `String`, encaja con la interfaz funcional `Supplier<String>`.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java existen varios tipos de referencias a método. Todas usan el operador `::`, pero cambian según se refieran a un método estático, un constructor, un método de una instancia concreta o un método de instancia aplicable a cualquier objeto de una clase.

Estas referencias son una forma abreviada de escribir lambdas cuando la lambda solo llama directamente a un método existente. Hacen el código más legible cuando el método referenciado ya expresa claramente la operación.

```java
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.function.BiFunction;

class Persona {
    private String nombre;

    public Persona() {
        this.nombre = "Sin nombre";
    }

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola, soy " + nombre;
    }

    public String saludarA(String otraPersona) {
        return "Hola " + otraPersona + ", soy " + nombre;
    }

    public static Persona crear(String nombre) {
        return new Persona(nombre);
    }
}

public class Main {
    public static void main(String[] args) {
        // Referencia a método estático
        Function<String, Persona> crearPersona = Persona::crear;

        // Referencia a constructor
        Function<String, Persona> constructorPersona = Persona::new;

        // Referencia a método de instancia de una instancia concreta
        Persona ana = new Persona("Ana");
        Supplier<String> saludoAna = ana::saludar;

        // Referencia a método de instancia sobre cualquier instancia
        Function<Persona, String> saludarPersona = Persona::saludar;

        // Referencia a método de instancia con parámetro sobre cualquier instancia
        BiFunction<Persona, String, String> saludarAOtro = Persona::saludarA;

        System.out.println(crearPersona.apply("Luis").saludar());
        System.out.println(constructorPersona.apply("Marta").saludar());
        System.out.println(saludoAna.get());
        System.out.println(saludarPersona.apply(new Persona("Carlos")));
        System.out.println(saludarAOtro.apply(new Persona("Elena"), "Pedro"));
    }
}
```

La referencia `Persona::saludar` no apunta a una instancia concreta, sino que espera recibir una instancia de `Persona` sobre la que invocar el método. Por eso encaja con `Function<Persona, String>`.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

Para ordenar una lista de objetos en Java se puede usar `Collections.sort` junto con un comparador. El comparador define qué persona debe ir antes que otra. En este caso, primero se compara la edad y, si ambas personas tienen la misma edad, se compara el nombre alfabéticamente.

La primera versión puede hacerse manualmente escribiendo toda la lógica de comparación dentro de una lambda. La segunda puede aprovechar los métodos de ayuda de `Comparator`, como `comparingInt` y `thenComparing`.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Ana", 30));
        personas.add(new Persona("Luis", 25));
        personas.add(new Persona("Carlos", 30));
        personas.add(new Persona("Beatriz", 25));

        Collections.sort(personas, (p1, p2) -> {
            if (p1.getEdad() < p2.getEdad()) {
                return -1;
            } else if (p1.getEdad() > p2.getEdad()) {
                return 1;
            } else {
                return p1.getNombre().compareTo(p2.getNombre());
            }
        });

        System.out.println(personas);
    }
}
```

La versión anterior es clara porque muestra explícitamente cómo funciona la comparación. Sin embargo, Java ofrece una forma más declarativa usando `Comparator`.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

class Persona {
    private String nombre;
    private int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }

    public String getNombre() {
        return nombre;
    }

    public int getEdad() {
        return edad;
    }

    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Ana", 30));
        personas.add(new Persona("Luis", 25));
        personas.add(new Persona("Carlos", 30));
        personas.add(new Persona("Beatriz", 25));

        Collections.sort(
            personas,
            Comparator
                .comparingInt(Persona::getEdad)
                .thenComparing(Persona::getNombre)
        );

        System.out.println(personas);
    }
}
```

La segunda versión suele considerarse más expresiva y fácil de mantener. Se lee casi como una frase: ordenar comparando primero por edad y después por nombre.
