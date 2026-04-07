<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia es un mecanismo fundamental que permite crear nuevas clases a partir de otras ya existentes, estableciendo una relación semántica del tipo "A es-un B" (por ejemplo, "un Artillero es un Soldado"). Esta relación implica que la nueva clase (subclase o clase derivada) hereda las características de la clase original (superclase o clase base), promoviendo la reutilización de código y la creación de jerarquías lógicas.

La primera implicación es la **herencia de estado y comportamiento**. La subclase recibe todos los atributos (estado) y métodos (comportamiento) de la superclase. En C, esto sería análogo a incluir un *struct* dentro de otro de forma transparente, heredando además las funciones asociadas a él. La segunda implicación es la **compatibilidad de tipos**. Al establecerse que la subclase "es un" tipo de la superclase, cualquier objeto de la subclase puede ser tratado y referenciado como si fuera un objeto de la superclase.

```java
class Soldado {
    private String nombre;
    public Soldado(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Hola, soy " + nombre); }
}

class Artillero extends Soldado {
    private int cohetes;
    public Artillero(String nombre, int cohetes) { super(nombre); this.cohetes = cohetes; }
    public int getCohetes() { return cohetes; }
}

class Zapador extends Soldado {
    private int minas;
    public Zapador(String nombre, int minas) { super(nombre); this.minas = minas; }
    public int getMinas() { return minas; }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] peloton = {
            new Artillero("Juan", 5),
            new Zapador("Pedro", 10),
            new Soldado("Luis")
        };
        // Compatibilidad de tipos: todos pueden tratarse como Soldado
        for (Soldado s : peloton) {
            s.saludar();
        }
    }
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de una subclase (por ejemplo, un `Artillero`), se ejecutan los constructores de todas las clases de su jerarquía, desde la clase base más alta hasta la subclase concreta, en orden descendente. Primero se inicializa la parte correspondiente al `Soldado` y luego la parte específica del `Artillero`. Esto garantiza que el objeto esté completamente inicializado en todos sus niveles lógicos.

La palabra clave `super` dentro de un constructor se utiliza para invocar explícitamente a un constructor de la clase base. Es el mecanismo equivalente a llamar a una función de inicialización del *struct* padre en C, pero integrado en el lenguaje. Esta llamada debe ser siempre la primera instrucción dentro del constructor de la subclase.

Si la clase base no tiene visible un constructor sin parámetros (ya sea porque se ha definido uno con parámetros y el compilador ya no genera el de por defecto, o porque es privado), es estrictamente obligatorio llamar a `super(...)` pasándole los argumentos correspondientes. De no hacerlo, el compilador producirá un error, ya que no sabría cómo inicializar el estado heredado de la superclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los atributos privados de la superclase forman parte íntegra de la instancia de la subclase en memoria. Al instanciar un `Artillero`, se reserva memoria (en el *heap*) tanto para sus atributos específicos (el número de cohetes) como para los atributos heredados de su superclase (el nombre del `Soldado`). Físicamente, el dato existe dentro del nuevo objeto.

Sin embargo, que el dato exista en memoria no implica que sea accesible directamente desde el código de la subclase. Debido a las reglas de encapsulación, los atributos marcados como `private` en `Soldado` están ocultos para cualquier otra clase, incluyendo a `Artillero` y `Zapador`. Para interactuar con ese "nombre" heredado, las subclases deben utilizar obligatoriamente los métodos públicos o protegidos proporcionados por la superclase (como getters o setters), manteniendo así la integridad de la información.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad a nivel de tipos permite escribir código abstracto que opere sobre la clase base, ignorando las implementaciones específicas de las subclases. En términos de extensibilidad, esto significa que se pueden añadir nuevos tipos al sistema sin necesidad de modificar el código existente que los consume (principio de abierto/cerrado). El código que maneja la superclase automáticamente será capaz de manejar las nuevas subclases.

Añadiendo un nuevo `Francotirador`, se observa que el bucle que recorre el array de soldados para pedirles que saluden permanece inalterado. La lógica de control se abstrae del tipo concreto, logrando una arquitectura mucho más mantenible que las clásicas sentencias `switch` basadas en enumerados o *tags* en C.

```java
class Francotirador extends Soldado {
    public Francotirador(String nombre) {
        super(nombre);
    }
}

// En el código principal, el mismo bucle anterior funciona sin cambios:
// Soldado[] peloton = { new Artillero("J", 5), new Francotirador("Wolf") };
// for (Soldado s : peloton) { s.saludar(); }
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, es completamente válido (y muy habitual) tener una referencia de un supertipo apuntando a un objeto real de un subtipo. A esta operación implícita y segura se la conoce como **"upcasting"** (por ejemplo, guardar un `Artillero` en una variable de tipo `Soldado`). Sin embargo, utilizando la referencia del supertipo, el compilador solo permite invocar los métodos definidos en dicho supertipo, ignorando los métodos específicos del objeto real subyacente.

Para acceder a los métodos específicos de la subclase es necesario realizar un **"downcasting"**, que consiste en forzar temporalmente la referencia al tipo derivado mediante un *cast* explícito, similar al moldeo de punteros en C (ej. `(Artillero) soldado`). Como esto puede fallar en tiempo de ejecución si el objeto no es del tipo esperado, se utiliza el operador `instanceof` para comprobar de forma segura si un objeto pertenece a una clase determinada antes de realizar el moldeo.

```java
for (Soldado s : peloton) {
    s.saludar();
    // Comprobación de tipo seguro
    if (s instanceof Artillero) {
        // Downcasting explícito
        Artillero artillero = (Artillero) s;
        System.out.println("Tengo " + artillero.getCohetes() + " cohetes.");
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El nivel de acceso protegido representa un punto intermedio entre el encapsulamiento total (privado) y la exposición pública. En orientación a objetos, un miembro protegido puede ser accedido desde la propia clase, desde otras clases del mismo paquete y, lo más importante para este contexto, desde cualquier **subclase**, sin importar en qué paquete se encuentre. En C no existe un equivalente directo, ya que la visibilidad a nivel de estructura suele ser todo o nada.

En Java se implementa mediante la palabra clave `protected`. Si se cambia la visibilidad del atributo `nombre` en la clase `Soldado` de `private` a `protected`, los atributos seguirán ocultos para el resto del programa, pero las clases hijas como `Zapador` podrán leer y modificar esa variable directamente, como si la hubieran declarado ellas mismas.

```java
class Soldado {
    protected String nombre; // Ahora es accesible por las subclases
    public Soldado(String nombre) { this.nombre = nombre; }
}

class Zapador extends Soldado {
    private int minas;
    public Zapador(String nombre, int minas) { 
        super(nombre); 
        this.minas = minas; 
    }
    public void ponerMina() {
        // Acceso directo al atributo protegido de la superclase
        System.out.println(this.nombre + " ha puesto una mina.");
    }
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

No todos los lenguajes orientados a objetos tienen una jerarquía unificada con una única clase base para todos los objetos. Por ejemplo, en C++ es posible crear múltiples jerarquías independientes que no comparten ningún ancestro común, lo que proporciona mucha libertad pero dificulta la creación de estructuras de datos genéricas o utilidades universales.

En el caso de Java, sí existe una jerarquía unificada. Todas las clases, ya sean las nativas del lenguaje o las creadas por el programador, heredan implícita o explícitamente de la clase `java.lang.Object`. Esto garantiza que cualquier instancia en Java disponga de un conjunto mínimo de métodos predefinidos (como `toString()` o `equals()`) y permite utilizar el tipo `Object` como una referencia comodín, de manera similar a cómo se emplea `void*` en C para referenciar cualquier dato genérico.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple es una característica de algunos lenguajes orientados a objetos (como C++ o Python) que permite que una subclase herede de dos o más superclases simultáneamente. Esto facilita agrupar comportamientos heterogéneos en un solo objeto, pero introduce complejidades importantes, como el "problema del diamante": ambigüedades sobre qué implementación utilizar si dos superclases tienen un método o atributo con el mismo nombre.

En Java **no existe** la herencia múltiple de clases. Los diseñadores del lenguaje decidieron sacrificar esta característica para simplificar la semántica y evitar las complicaciones asociadas. Una clase en Java solo puede usar la palabra `extends` con una única superclase. Sin embargo, Java permite solventar la mayoría de los casos de uso de la herencia múltiple mediante el uso de **interfaces**, que permiten heredar múltiples contratos o comportamientos sin heredar estado ni causar ambigüedades.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

Al ser las excepciones objetos normales en Java, se benefician de la herencia para crear tipos específicos. Para crear una excepción *no controlada* (que no obliga a ser capturada con `try-catch`), la nueva clase debe heredar de `RuntimeException`. A esta clase personalizada se le puede añadir estado propio, favoreciendo la composición al incluir objetos relevantes que aporten contexto al error.

Además, al sobrescribir o añadir constructores y usar `super`, se puede invocar a los constructores de la clase padre de las excepciones para aprovechar sus mecanismos internos de seguimiento de pila (*stack trace*) y encadenamiento de causas.

```java
public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario; // Composición: incluye contexto extra

    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Sobrecarga para permitir encadenamiento de excepciones (causa subyacente)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa); // Llama al constructor base que gestiona la causa
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

El uso de la herencia establece una relación estática y muy rígida entre dos clases ("A es un B"). Si se emplea la herencia únicamente para aprovechar un par de métodos útiles de otra clase, sin que exista una verdadera relación jerárquica conceptual, se termina contaminando el diseño. Por ejemplo, si una clase `Coche` hereda de `BaseDeDatos` solo para usar un método de guardado, semánticamente se está afirmando que "un coche es una base de datos", lo cual es incorrecto.

Esta práctica genera jerarquías frágiles y acoplamientos innecesarios. Las subclases heredan un montón de atributos y métodos que no necesitan ni tienen sentido en su contexto (contaminación del interfaz público). En C/C++ esto equivale a incluir un *struct* masivo dentro de otro solo para acceder a un entero, desperdiciando memoria y entorpeciendo el mantenimiento del código a largo plazo.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Se recomienda favorecer la composición porque establece una relación del tipo "tiene-un" (ej. "el Coche tiene-un Motor"), que resulta mucho más flexible que la relación "es-un" de la herencia. Con la composición, el comportamiento se reutiliza delegando el trabajo en componentes internos, en lugar de acoplar estáticamente el código a una superclase. 

Esta flexibilidad permite ensamblar y modificar el comportamiento de un objeto en tiempo de ejecución (cambiando las instancias que lo componen), algo imposible con la herencia que se fija en tiempo de compilación. Además, la composición reduce el acoplamiento y evita la explosión combinatoria de clases que suele producirse cuando se intenta modelar sistemas complejos basándose únicamente en largas cadenas de herencia.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

Se afirma que la herencia rompe la encapsulación porque la subclase pasa a depender íntimamente de los detalles de implementación internos de su superclase, cruzando las fronteras tradicionales del encapsulamiento de caja negra. En una relación normal entre dos clases distintas, interactúan únicamente mediante contratos públicos bien definidos.

Cuando se usa herencia, si la clase base modifica un método interno, cambia la semántica de una variable `protected`, o añade una nueva llamada cruzada entre sus propios métodos, la subclase puede dejar de funcionar o comportarse de forma inesperada. Esta fuga de detalles de implementación hace que los cambios en las clases superiores se propaguen en cascada, provocando que la jerarquía sea frágil y difícil de mantener.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

La principal diferencia entre ambos enfoques reside en la escalabilidad y la claridad semántica. Mediante herencia, se extrae el estado común a una clase superior de la cual dependen las otras de manera estática y rígida. Mediante composición, la clase encapsula a otra entidad que actúa como componente intercambiable, favoreciendo la delegación de responsabilidades de forma dinámica.

```java
// Alternativa 1: Herencia ("es un")
class Persona {
    protected String dni;
    protected String nombre;
    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) { super(dni, nombre); }
}

// Alternativa 2: Composición ("tiene un")
class DatosPersonales {
    private String dni;
    private String nombre;
    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Trabajador {
    private DatosPersonales datos; // Composición
    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```
