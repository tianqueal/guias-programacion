# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C, donde no existe el mecanismo de excepciones, el control de errores debe hacerse de forma manual mediante convenciones explícitas entre la función y su llamador. Esto significa que el programador debe decidir de antemano cómo comunicar que algo ha ido mal, lo que añade una responsabilidad extra al código llamador de comprobar si ha ocurrido un error.

**Opción 1: valor de retorno especial.** La función devuelve un valor especial (como `-1.0`) que por convención indica error. El llamador debe comparar el resultado con ese valor centinela para detectar el fallo.

```c
#include <math.h>
#include <stdio.h>

double raiz(double x) {
    if (x < 0) {
        return -1.0; /* valor especial que indica error */
    }
    return sqrt(x);
}

int main() {
    double resultado = raiz(-4.0);
    if (resultado == -1.0) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
    return 0;
}
```

**Opción 2: parámetro de salida para el código de error.** Se añade un puntero a entero como parámetro adicional. La función escribe en él un código de error (0 = éxito, 1 = error), y el valor de retorno queda reservado exclusivamente para el resultado útil.

```c
#include <math.h>
#include <stdio.h>

double raiz(double x, int *error) {
    if (x < 0) {
        *error = 1;
        return 0.0;
    }
    *error = 0;
    return sqrt(x);
}

int main() {
    int error;
    double resultado = raiz(-4.0, &error);
    if (error) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
    return 0;
}
```

Ambas opciones tienen el inconveniente de que dependen de que el llamador recuerde comprobar el error. Si el programador olvida hacer esa comprobación, el error pasa desapercibido silenciosamente, lo que puede dar lugar a comportamientos incorrectos difíciles de depurar.


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un mecanismo de los lenguajes de programación modernos que permite señalizar situaciones de error o condiciones anómalas de forma separada del flujo normal de ejecución del programa. En lugar de devolver valores especiales o pasar punteros de error, el propio lenguaje proporciona una vía alternativa para comunicar que algo inesperado ha ocurrido, interrumpiendo el flujo normal de ejecución.

Cuando un programador **implementa** una función, usa excepciones para indicar que no puede completar su tarea en las condiciones actuales (por ejemplo, recibir un argumento inválido o encontrarse con un recurso no disponible). Cuando un programador **llama** a una función, usa las excepciones para reaccionar ante esas situaciones de error en un lugar concreto del código, sin necesidad de comprobar valores de retorno en cada llamada. Esto permite separar claramente el código del "camino feliz" del código de tratamiento de errores, haciendo el código más legible y menos propenso a errores de olvido.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, la función `raiz` pasa a ser un método de una clase. Cuando el argumento es negativo, en lugar de devolver un valor especial o modificar un parámetro, se lanza una excepción usando la palabra clave `throw`. El método llamador puede entonces capturar esa excepción con un bloque `try-catch`.

```java
public class Calculadora {

    public double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "No se puede calcular la raiz de un numero negativo: " + x
            );
        }
        return Math.sqrt(x);
    }
}
```

```java
public class Main {

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            double resultado = calc.raiz(-4.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado: " + e.getMessage());
        }
    }
}
```

La ventaja respecto al ejemplo en C es inmediata: si el programador no escribe el `try-catch`, la excepción se propagará automáticamente hacia arriba hasta que alguien la trate o el programa termine. No es posible ignorar la excepción sin consecuencias, a diferencia de olvidarse de comprobar un valor de retorno especial.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

**Lanzar** una excepción (`throw`) significa que una función interrumpe su ejecución normal y señaliza un error creando un objeto excepción y entregándolo al mecanismo de gestión de errores del lenguaje. En el ejemplo de `raiz`, la línea `throw new IllegalArgumentException(...)` hace que el método abandone inmediatamente su ejecución sin devolver ningún valor.

**Capturar** o **controlar** una excepción significa que un bloque `try-catch` en el código llamador intercepta esa excepción antes de que siga propagándose. En `main`, el bloque `catch (IllegalArgumentException e)` recibe el objeto excepción y ejecuta el código de tratamiento de error, recuperando el control del flujo del programa en ese punto.

Cuando ningún bloque `catch` en el método actual captura la excepción, esta **se propaga**: el método se abandona abruptamente (sin llegar a su `return` normal) y la excepción asciende al método que lo llamó, que a su vez puede capturarla o dejarla seguir propagándose hacia arriba en la **pila de llamadas** (*call stack*). Por ejemplo, si `main` llamara a un método intermedio `calcular` y este llamara a `raiz`, y ninguno capturara la excepción, primero se abandonaría `raiz`, luego `calcular` y finalmente `main`, terminando el programa con un mensaje de error. Las funciones que no capturan la excepción **no se reanudan** en ningún momento; quedan interrumpidas definitivamente en el punto donde la excepción las alcanzó, como si sus líneas posteriores nunca hubieran existido.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La propagación natural de excepciones elimina la necesidad de comprobar manualmente el error en cada nivel de la pila de llamadas. En C, si `main` llama a `A`, `A` llama a `B` y `B` llama a `raiz`, hay que comprobar el error en `raiz`, devolvérselo a `B`, que debe comprobar y devolvérselo a `A`, que a su vez debe comprobar y devolvérselo a `main`. Cada nivel intermedio debe gestionar el error aunque no sepa qué hacer con él, lo que contamina el código con comprobaciones repetitivas.

Con excepciones, si solo `main` sabe cómo tratar el error de argumento inválido, las funciones intermedias `A` y `B` no necesitan escribir ningún código especial: la excepción "viaja" automáticamente a través de ellas hasta llegar a `main`. Esto produce un código más limpio donde cada función se centra en su responsabilidad, sin mezclar lógica de negocio con gestión de errores.

Además, la propagación natural garantiza que el error nunca pueda ser ignorado silenciosamente: si ningún `catch` lo captura, el programa termina visiblemente con información sobre la excepción. En C, en cambio, olvidarse de comprobar un valor de retorno de error provoca que el programa continúe con datos incorrectos, generando fallos difíciles de rastrear.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Sí, en orientación a objetos las excepciones son objetos. En Java, todas las excepciones son instancias de clases que heredan de `Throwable` (concretamente de `Exception` o `RuntimeException`). Esto significa que al lanzar una excepción se crea un objeto como cualquier otro, con atributos y métodos propios.

El hecho de que sean objetos aporta las mismas ventajas de **encapsulación** que se aplican a cualquier clase: la excepción puede llevar consigo toda la información relevante sobre el error (mensaje, contexto, valores que causaron el fallo) encapsulada en sus atributos, con métodos para acceder a ella de forma controlada. El código que captura la excepción puede consultar esa información a través de métodos como `getMessage()` o `getCause()`, sin necesidad de usar variables globales ni estructuras ad-hoc como en C.

Sí se pueden crear **excepciones personalizadas** simplemente creando una clase que extienda `Exception` o `RuntimeException`. Por ejemplo, una clase `RaizDeNegativoException` puede incluir atributos específicos como el valor del argumento inválido, aportando información más precisa que una excepción genérica. Esta posibilidad de personalización es fundamental para diseñar APIs expresivas donde los errores sean parte del contrato de la clase.

```java
public class RaizDeNegativoException extends RuntimeException {
    private final double valorNegativo;

    public RaizDeNegativoException(double valorNegativo) {
        super("No se puede calcular la raiz de: " + valorNegativo);
        this.valorNegativo = valorNegativo;
    }

    public double getValorNegativo() {
        return valorNegativo;
    }
}
```


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Todo objeto excepción en Java lleva encapsulada, como mínimo, una **traza de la pila de llamadas** (*stack trace*) capturada automáticamente en el momento en que se crea la excepción. Esta traza indica exactamente qué método lanzó la excepción y toda la cadena de llamadas que llevó hasta ese punto, con los nombres de los ficheros y los números de línea correspondientes. En C no existe nada equivalente: si una función devuelve un código de error, no hay información sobre dónde ni por qué ocurrió, lo que hace la depuración considerablemente más difícil.

Además del *stack trace*, el objeto excepción encapsula un **mensaje descriptivo** (accesible mediante `getMessage()`) que puede explicar en lenguaje natural el motivo del error, y puede incluir una **causa** (accesible mediante `getCause()`), que es otra excepción que originó la actual. En excepciones personalizadas es posible añadir cualquier otro dato relevante como atributos de la clase (como el valor que causó el error), todo ello accesible de forma limpia y encapsulada a través de métodos.

Esta información integrada en el propio objeto excepción, y disponible automáticamente sin que el programador tenga que hacer nada especial, es una ventaja enorme respecto a C. Cuando se imprime una excepción o se llama a `printStackTrace()`, se obtiene inmediatamente un diagnóstico completo del error, lo que agiliza enormemente la localización y corrección de fallos.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí, en Java un bloque `try` puede tener **múltiples bloques `catch`**, cada uno especializados en capturar un tipo distinto de excepción. Esto permite tratar de forma diferenciada los distintos errores que puede producir el código dentro del `try`, aplicando una estrategia de recuperación específica para cada caso.

```java
try {
    Calculadora calc = new Calculadora();
    double resultado = calc.raiz(Double.parseDouble(args[0]));
    System.out.println("Resultado: " + resultado);
} catch (IllegalArgumentException e) {
    System.out.println("Argumento invalido: " + e.getMessage());
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Falta el argumento en la linea de comandos");
} catch (NumberFormatException e) {
    System.out.println("El argumento no es un numero valido: " + e.getMessage());
}
```

**Solo se ejecuta uno de los bloques `catch`**, el primero cuyo tipo de excepción sea compatible (es decir, sea del mismo tipo o un supertipo) con la excepción lanzada. Una vez ejecutado ese `catch`, el resto se omite y el flujo continúa después del último `catch`. Por ello, si se tienen varios `catch` con tipos relacionados por herencia, deben ordenarse de más específico a más general: si el más general se pusiera primero, capturaría todas las excepciones y los `catch` más específicos nunca se ejecutarían.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

El bloque **`finally`** en Java garantiza que su contenido se ejecuta **siempre**, tanto si el bloque `try` termina normalmente, como si lanza una excepción capturada por un `catch`, como si la excepción se propaga sin ser capturada. Es el lugar adecuado para liberar recursos que deben cerrarse independientemente del resultado (ficheros, conexiones a bases de datos, etc.).

**Con `catch` y `finally`:** se captura la excepción para tratarla, pero el cierre del recurso queda garantizado en `finally` incluso si el tratamiento del error vuelve a lanzar otra excepción.

```java
BufferedReader lector = null;
try {
    lector = new BufferedReader(new FileReader("datos.txt"));
    String linea = lector.readLine();
    System.out.println("Primera linea: " + linea);
} catch (IOException e) {
    System.out.println("Error leyendo el fichero: " + e.getMessage());
} finally {
    if (lector != null) {
        try {
            lector.close();
        } catch (IOException e) {
            System.out.println("Error cerrando el fichero: " + e.getMessage());
        }
    }
}
```

**Sin `catch`, solo con `finally`:** la excepción se propaga hacia arriba, pero antes se ejecuta el cierre del recurso. Esto es útil cuando el método no sabe cómo tratar el error pero sí debe limpiar recursos.

```java
BufferedReader lector = new BufferedReader(new FileReader("datos.txt"));
try {
    String linea = lector.readLine();
    System.out.println("Primera linea: " + linea);
} finally {
    lector.close(); /* se ejecuta siempre, aunque se propague la excepción */
}
```


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, como se ha visto en el ejemplo anterior, el bloque `finally` **puede ir sin `catch`**. La combinación válida en Java es `try-finally` (sin `catch`), `try-catch` (sin `finally`) o `try-catch-finally` (con ambos). Cuando se usa `try-finally` sin `catch`, las excepciones no se capturan sino que se propagan, pero el `finally` se ejecuta igualmente antes de que la propagación continúe.

El bloque `finally` **se ejecuta siempre**, tanto si el `try` termina normalmente sin excepción, como si lanza una excepción capturada por un `catch`, como si lanza una excepción que se propaga. Es la garantía más fuerte que ofrece Java para la ejecución de código de limpieza.

Incluso si **hay un `return` dentro del `try`** (o dentro de un `catch`), el bloque `finally` se ejecuta antes de que el método retorne efectivamente. Java retiene el valor de retorno, ejecuta el `finally`, y solo entonces devuelve el control al llamador. El único caso donde `finally` puede no ejecutarse es si la JVM se apaga forzosamente (por ejemplo, llamando a `System.exit()`) o si el hilo se mata abruptamente.

```java
public static int ejemplo() {
    try {
        System.out.println("Dentro del try");
        return 1; /* el finally se ejecuta antes de retornar */
    } finally {
        System.out.println("Dentro del finally"); /* esto SI se imprime */
    }
}
/* Salida: "Dentro del try", "Dentro del finally". Retorna 1. */
```


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Las excepciones **controladas** (*checked exceptions*) son aquellas que el compilador de Java obliga a gestionar explícitamente: si un método puede lanzar una excepción controlada, el llamador debe o bien capturarla con `try-catch` o bien declarar que la puede propagar con `throws`. Estas excepciones representan condiciones de error que el programador puede anticipar y de las que razonablemente se puede recuperar. Heredan de `Exception` pero **no** de `RuntimeException`.

Las excepciones **no controladas** (*unchecked exceptions*) son aquellas que el compilador no obliga a tratar. Representan errores de programación (bugs) o condiciones tan graves que generalmente no tiene sentido intentar recuperarse de ellas. En Java, son todas las que heredan de `RuntimeException` (o de `Error`). El papel de `RuntimeException` es precisamente marcar esa frontera: todo lo que descienda de ella es no controlada.

Ejemplos de excepciones **controladas** (propias o del JDK): `IOException`, `FileNotFoundException`, `SQLException`, y una personalizada `SaldoInsuficienteException`.
Ejemplos de excepciones **no controladas**: `NullPointerException`, `IllegalArgumentException`, `ArrayIndexOutOfBoundsException`, y una personalizada `EstadoInvalidoException`.

**Situaciones donde se prefiere una excepción controlada:**
- Abrir un fichero que puede no existir (`FileNotFoundException`).
- Conectar a una base de datos con problemas de red (`SQLException`).
- Leer un fichero con formato incorrecto (excepción personalizada de parseo).
- Realizar una transferencia bancaria con fondos insuficientes (`SaldoInsuficienteException`).

**Situaciones donde se prefiere una excepción no controlada:**
- Pasar un argumento `null` a un método que no lo acepta (`NullPointerException`, `IllegalArgumentException`).
- Acceder a un índice fuera de rango en un array (`ArrayIndexOutOfBoundsException`).
- Llamar a un método en un estado de objeto inválido para esa operación (`IllegalStateException`).
- Dividir por cero por un error de lógica del programador (`ArithmeticException`).


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La cláusula **`throws`** en la firma de un método declara que ese método puede propagar hacia el llamador una o varias excepciones controladas sin capturarlas internamente. Es una forma de informar al compilador (y al programador que usa el método) de que esa excepción puede salir del método, de modo que el llamador sea quien decida cómo tratarla.

```java
public void leerFichero(String ruta) throws IOException {
    BufferedReader lector = new BufferedReader(new FileReader(ruta));
    /* ... */
}
```

Es una **alternativa a capturar** la excepción controlada porque el compilador exige que las excepciones controladas sean gestionadas: o bien se capturan con `try-catch` dentro del método, o bien se declaran en `throws` para propagarlas. Si un método no sabe cómo tratar el error o no es su responsabilidad hacerlo, lo más limpio es declararlo en `throws` y dejar que el llamador, que tiene más contexto, decida cómo actuar. Si en cambio se captura la excepción donde no se puede hacer nada útil, se corre el riesgo de silenciarla o de escribir código de tratamiento de errores irrelevante.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

El método `leerPrimeraLinea` declara con `throws IOException` que no captura el error de fichero no encontrado, dejando esa responsabilidad al llamador. Sin embargo, usa `finally` para garantizar que el recurso se cierra aunque la excepción se propague.

```java
public String leerPrimeraLinea(String ruta) throws IOException {
    BufferedReader lector = null;
    try {
        lector = new BufferedReader(new FileReader(ruta));
        return lector.readLine();
    } finally {
        if (lector != null) {
            lector.close(); /* se ejecuta siempre, incluso si se propaga la excepción */
        }
    }
}
```

El llamador es quien debe decidir si captura la excepción o la propaga a su vez:

```java
public static void main(String[] args) {
    try {
        String linea = leerPrimeraLinea("datos.txt");
        System.out.println("Linea: " + linea);
    } catch (IOException e) {
        System.out.println("No se pudo leer el fichero: " + e.getMessage());
    }
}
```

Este diseño respeta el principio de separación de responsabilidades: el método `leerPrimeraLinea` hace lo que su nombre indica (leer) y gestiona sus recursos, pero no decide qué hacer cuando el fichero no existe, ya que esa decisión depende del contexto de uso de cada llamador.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí, **es sintácticamente válido** incluir excepciones no controladas (subclases de `RuntimeException`) en la cláusula `throws`, aunque el compilador no lo exige ni lo impone. No obliga al llamador a capturarlas; el código compila igualmente sin ningún `try-catch` en el llamador.

El método llamador **no está obligado** a poner `try-catch` para excepciones no controladas declaradas en `throws`, y generalmente tampoco debería hacerlo, ya que estas excepciones representan errores de programación de los que normalmente no se puede recuperar de forma significativa.

Sin embargo, declarar excepciones no controladas en `throws` **puede tener sentido como documentación**: comunica explícitamente al programador que usa el método que en ciertas condiciones se lanzará esa excepción, algo que de otro modo solo se sabría leyendo el código fuente o la documentación. Por ejemplo, si un método lanza `IllegalArgumentException` cuando recibe un argumento nulo, declararlo en `throws` sirve de advertencia en la firma del método. En general, esta práctica es más útil en APIs públicas como forma de contrato explícito, pero no es un patrón universal ni obligatorio.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda usar **excepciones controladas** cuando el error representa una condición externa, previsible y de la que el llamador **puede razonablemente recuperarse**: recursos no disponibles (ficheros, redes, bases de datos), entradas de usuario con formato incorrecto, o situaciones en las que el código llamador tiene sentido que tome decisiones diferentes según el fallo. El hecho de que el compilador fuerce a tratar estas excepciones actúa como documentación del contrato del método y previene que el error pase inadvertido.

Se recomienda usar **excepciones no controladas** cuando el error indica un **bug o uso incorrecto del API** por parte del programador: argumentos inválidos, estados de objeto inconsistentes, violaciones de precondiciones que el llamador debería haber evitado. En estos casos, obligar a cada llamador a rodear el código con `try-catch` sería innecesariamente verboso e incómodo, ya que la solución es corregir el código, no capturar la excepción.

La distinción entre controladas y no controladas **no existe en todos los lenguajes**: Python, C#, Kotlin, Swift o C++ solo tienen un tipo de excepciones (equivalentes a las no controladas). En estos lenguajes, la excepción puede propagarse sin que el compilador obligue a capturarla. La opción más habitual en los lenguajes que solo tienen un tipo de excepción es precisamente la **no controlada** (*unchecked*), ya que la experiencia con Java ha mostrado que el abuso de excepciones controladas puede generar un código más verboso y con `catch` vacíos que las silencian, resultando peor que no tenerlas.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, **tiene sentido lanzar excepciones dentro de un `catch`**. El caso más habitual es capturar una excepción de bajo nivel y lanzar otra de más alto nivel, más apropiada para el contexto del llamador. Por ejemplo, un repositorio de datos puede capturar una `SQLException` (detalle de implementación) y lanzar una excepción propia de la capa de negocio, ocultando que internamente se usa SQL.

```java
public Usuario buscarUsuario(int id) {
    try {
        return baseDatos.consultar("SELECT * FROM usuarios WHERE id = " + id);
    } catch (SQLException e) {
        throw new RepositorioException("Error al buscar el usuario con id " + id, e);
    }
}
```

También **se puede relanzar la misma excepción capturada** usando `throw e` o simplemente `throw` dentro del `catch`. Esto tiene sentido cuando se quiere ejecutar alguna acción antes de que la excepción siga propagándose (registrar el error en un log, limpiar algún estado, notificar algo) pero sin tener intención de recuperarse de ella.

```java
public void procesarFichero(String ruta) throws IOException {
    try {
        leerFichero(ruta);
    } catch (IOException e) {
        logger.error("Error procesando fichero: " + ruta, e); /* registrar antes de propagar */
        throw e; /* relanzar la misma excepción */
    }
}
```

En ambos casos el control después del `catch` no continúa: al lanzar dentro del `catch`, el flujo se interrumpe de nuevo y la nueva excepción (o la relanzada) sigue propagándose hacia arriba por la pila de llamadas.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Que una excepción sea la **"causa"** (*cause*) de otra significa que la excepción original que desencadenó el error se encapsula dentro de la nueva excepción, preservando toda la información diagnóstica del fallo original. En Java, la mayoría de constructores de excepciones aceptan un parámetro `Throwable cause` que almacena esa excepción original, accesible luego mediante `getCause()`. Esta técnica se conoce como **encadenamiento de excepciones** (*exception chaining*).

El encadenamiento es fundamental para mantener un buen nivel de abstracción sin perder información. Si una capa de acceso a datos captura una `SQLException` y lanza una `RepositorioException`, pero no preserva la causa, el desarrollador que depure el problema en producción no sabrá qué falló exactamente en la base de datos. Al encadenarla, la `RepositorioException` da contexto de alto nivel y la `SQLException` preserva el diagnóstico técnico preciso.

```java
public class RepositorioException extends RuntimeException {

    public RepositorioException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class UsuarioRepositorio {

    public Usuario buscarPorId(int id) {
        try {
            /* simulamos un fallo de base de datos */
            throw new SQLException("Conexion perdida con el servidor");
        } catch (SQLException e) {
            throw new RepositorioException(
                "No se pudo recuperar el usuario con id " + id, e
            );
        }
    }
}

public class Main {

    public static void main(String[] args) {
        UsuarioRepositorio repo = new UsuarioRepositorio();
        try {
            repo.buscarPorId(42);
        } catch (RepositorioException e) {
            e.printStackTrace();
        }
    }
}
```

Cuando una excepción con causa se imprime por pantalla (mediante `printStackTrace()` o al propagarse sin capturar), **sí se ve la causa**: Java imprime la traza de la excepción principal y debajo, precedida de `Caused by:`, imprime la traza completa de la excepción original. Esto permite ver toda la cadena de causas hasta el fallo raíz, lo que resulta enormemente útil para diagnosticar problemas en producción.
