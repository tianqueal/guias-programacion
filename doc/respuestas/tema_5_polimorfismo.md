<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es la capacidad que tienen los objetos de distintas clases de responder de manera diferente ante la misma invocación de un método. En lenguajes estructurados como C, esto requeriría estructuras complejas con punteros a funciones para simular comportamientos variables en tiempo de ejecución. En programación orientada a objetos, el polimorfismo permite tratar objetos de tipos derivados a través de una referencia de su tipo base, ejecutando automáticamente el código correspondiente a su tipo real.

Su principal utilidad es la creación de código genérico, flexible y fácilmente extensible. Al programar contra un tipo base o interfaz, el sistema puede incorporar nuevas subclases en el futuro sin necesidad de modificar las rutinas que las consumen. Esto elimina las largas cadenas de sentencias condicionales que evalúan el tipo de dato, delegando la decisión del comportamiento al propio objeto.

La sobreescritura (*overriding*) es el mecanismo fundamental que hace posible el polimorfismo de inclusión. Consiste en proporcionar una nueva implementación en una subclase para un método que ya ha sido definido en su superclase. Cuando se invoca este método sobre un objeto de la subclase (incluso si la referencia es del tipo padre), se ejecutará la versión sobreescrita y no la original.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica (*dynamic binding* o enlace tardío) es el proceso mediante el cual el entorno de ejecución determina qué implementación concreta de un método debe ejecutarse, basándose en el tipo real del objeto en memoria y no en el tipo de la variable o puntero que lo referencia. Esto contrasta con la ligadura estática habitual de lenguajes procedimentales, donde la dirección de la función a llamar se resuelve fijamente en tiempo de compilación.

La ligadura dinámica es el motor técnico subyacente que hace funcionar el polimorfismo. Sin ella, una referencia de la clase base siempre invocaría el método de la clase base, ignorando las sobreescrituras de las subclases. La forma en que se aplica esta característica depende completamente de las decisiones de diseño de cada lenguaje de programación.

En C++, la ligadura dinámica no es el comportamiento por defecto por razones de rendimiento; para activarla, es necesario marcar explícitamente los métodos con la palabra clave `virtual` en la clase base. En contraste, Java adopta el enfoque opuesto: todos los métodos de instancia (que no sean privados, estáticos o finales) utilizan ligadura dinámica por defecto, simplificando la escritura de código polimórfico. Por su parte, en lenguajes de tipado dinámico como Python, la ligadura es inherentemente dinámica (*duck typing*), resolviéndose siempre en tiempo de ejecución sin necesidad de declaraciones estrictas.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

Se define la clase base `Soldado` con un comportamiento predeterminado para el método `saludar()`. Las subclases `Zapador` y `Artillero` heredan de ella, pero `Zapador` sobreescribe el método para proporcionar su propio saludo, mientras que `Artillero` mantiene el comportamiento original heredado.

En el método principal, se crea un array de referencias de tipo `Soldado` que almacena instancias reales tanto de la clase base como de sus subclases. Al iterar sobre este array y llamar a `saludar()`, la máquina virtual de Java aplica la ligadura dinámica. Aunque todas las referencias son tratadas uniformemente como `Soldado`, el programa invoca el saludo genérico para los soldados y artilleros, y el saludo específico para los zapadores.

```java
class Soldado {
    public void saludar() {
        System.out.println("Soldado listo para el combate.");
    }
}

class Artillero extends Soldado {
    // Hereda saludar() sin cambios
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador preparado para detonar explosivos.");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] peloton = {
            new Soldado(),
            new Zapador(),
            new Artillero()
        };

        for (Soldado s : peloton) {
            s.saludar(); // Polimorfismo en acción
        }
    }
}
```

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, es perfectamente posible invocar la implementación del método de la clase base desde el método que lo sobreescribe en la subclase. Esto resulta muy útil cuando no se desea reemplazar por completo el comportamiento original, sino extenderlo, decorarlo o añadir validaciones previas o posteriores a la ejecución base.

Para lograr esto en Java, se utiliza la palabra clave `super` seguida del punto y el nombre del método (por ejemplo, `super.saludar()`). A diferencia de la llamada `super()` empleada en los constructores, que debe ser obligatoriamente la primera instrucción, la invocación a métodos de la superclase puede colocarse en cualquier lugar dentro de la lógica del método sobreescrito.

```java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Invoca el comportamiento de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobreescribir un método, la firma (nombre del método y lista exacta de parámetros) debe ser idéntica a la de la clase base. El tipo de retorno debe ser el mismo o un subtipo del original (lo que se conoce como tipos de retorno covariantes). Además, el nivel de acceso no puede volverse más restrictivo; por ejemplo, si el método en la clase base es `protected`, la versión sobreescrita en la subclase puede ser `protected` o `public`, pero nunca `private`.

La diferencia principal radica en su propósito y resolución. La sobreescritura (*overriding*) redefine un método heredado manteniendo su firma exacta para habilitar el polimorfismo dinámico en tiempo de ejecución. Por el contrario, la sobrecarga (*overloading*) consiste en definir múltiples métodos con el mismo nombre dentro de una clase, pero con distinta lista de parámetros. La sobrecarga se resuelve de forma estática durante la compilación.

La anotación `@Override` se coloca justo encima de la declaración del método en la subclase. Sirve como una instrucción explícita al compilador para que verifique que realmente existe un método en la superclase con esa firma. Es una práctica altamente recomendada, ya que previene errores sutiles como fallos tipográficos en el nombre del método o discrepancias en los parámetros, que de otro modo generarían una sobrecarga accidental e inadvertida en lugar de la sobreescritura deseada.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí, el polimorfismo se emplea desde los primeros programas escritos en Java, a menudo sin que el desarrollador sea plenamente consciente de la mecánica subyacente. Esto se debe a la arquitectura unificada del lenguaje, donde todas las clases heredan implícita o explícitamente de la clase raíz universal `java.lang.Object`.

Métodos fundamentales como `toString()` o `equals()` están definidos en esta superclase común `Object`. Cuando funciones estándar de la biblioteca, como `System.out.println()`, reciben un objeto cualquiera, operan sobre él empleando una referencia genérica de tipo `Object` y llaman a su método `toString()`. Al sobreescribir estos métodos en las clases propias, se entra directamente en el terreno del polimorfismo, forzando a la máquina virtual a invocar dinámicamente las versiones específicas de la subclase.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Un método abstracto es aquel que se declara para definir un contrato de comportamiento, pero no proporciona ninguna implementación concreta (carece de cuerpo o bloque de código). Es conceptualmente idéntico a una función virtual pura en C++ (identificada con `= 0`). Una clase abstracta es una clase incompleta que sirve como plantilla genérica; si una clase contiene al menos un método abstracto, debe ser declarada obligatoriamente como clase abstracta.

Debido a su naturaleza incompleta, no es posible crear instancias directas de una clase abstracta (no se puede usar el operador `new` sobre ella). Su único propósito es servir como clase base para que otras subclases hereden su estado común y proporcionen las implementaciones específicas de los métodos abstractos. La palabra clave `abstract` debe colocarse tanto en la firma del método abstracto como en la declaración principal de la clase.

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado listo.");
    }
    
    // Método abstracto: fuerza a las subclases a implementarlo
    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Colocando explosivos en el objetivo.");
    }
}
```

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave `final` actúa como una restricción estricta en la cadena de herencia. Cuando se aplica a la declaración de un método, prohíbe explícitamente que cualquier subclase pueda sobreescribirlo. Si se aplica a nivel de la declaración de una clase entera, impide por completo que dicha clase pueda ser heredada, bloqueando la creación de cualquier subclase a partir de ella.

El uso de `final` limita intencionadamente las capacidades polimórficas del código. Se emplea en situaciones de seguridad o diseño arquitectónico cerrado, cuando es imperativo garantizar que un comportamiento fundamental no sea alterado, extendido o corrompido por implementaciones derivadas de terceros.

Un ejemplo clásico y vital en la API estándar de Java es la clase `java.lang.String`. Esta clase está declarada como `final` para asegurar su inmutabilidad absoluta y garantizar que cualquier referencia a un objeto de tipo texto se comporte exactamente como dicta la especificación, previniendo vulnerabilidades de seguridad que podrían surgir si el sistema aceptara una subclase modificada de "String malicioso".

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las interfaces en Java son estructuras puramente abstractas que definen un contrato formal de comportamiento, especificando exhaustivamente "qué" debe hacer una clase, pero sin dictar en absoluto "cómo" debe hacerlo. Tradicionalmente, contienen únicamente firmas de métodos vacías (sin implementación) y constantes públicas. Proporcionan una alternativa limpia y estructurada frente a las complejidades técnicas de la herencia múltiple presentes en lenguajes como C++.

Aunque guardan similitud con las clases abstractas en el hecho de que no pueden ser instanciadas y contienen métodos abstractos, difieren de forma drástica en su naturaleza. Una clase abstracta representa una relación de pertenencia ("es un") y puede mantener estado interno (variables) y lógica común. Una interfaz representa capacidades accesorias ("es capaz de" o "se comporta como") y carece por completo de estado asociado a la instancia.

Una característica arquitectónica clave en Java es que, si bien una clase está restringida a heredar de una única superclase (herencia simple), tiene total libertad para implementar múltiples interfaces simultáneamente. Esto permite diseñar sistemas donde un mismo objeto puede asumir múltiples roles polimórficos de forma segura, satisfaciendo requerimientos transversales sin recurrir a intrincadas jerarquías de herencia.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

La clase `Punto` se declara como abstracta y define el contrato general a través del método `calcularDistanciaA`. Sus clases derivadas, `Punto2D` y `Punto3D`, proporcionan las implementaciones matemáticas concretas. Para garantizar la coherencia de las operaciones, utilizan el operador `instanceof` para validar la dimensionalidad del punto objetivo antes de realizar un *downcasting* seguro que permite extraer las coordenadas específicas.

Posteriormente, la clase `Linea` actúa como consumidora de esta jerarquía abstracta. Al almacenar internamente referencias genéricas de tipo `Punto`, la línea desconoce si opera en dos o tres dimensiones. Al solicitar su longitud, invoca el cálculo sobre los puntos; será la ligadura dinámica quien redirija la ejecución a la implementación bidimensional o tridimensional correspondiente, evidenciando el poder de la abstracción polimórfica.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto p);
}

class Punto2D extends Punto {
    double x, y;
    public Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (p instanceof Punto2D) {
            Punto2D p2d = (Punto2D) p; // Downcasting seguro
            return Math.sqrt(Math.pow(this.x - p2d.x, 2) + Math.pow(this.y - p2d.y, 2));
        }
        throw new IllegalArgumentException("El punto destino debe ser 2D.");
    }
}

class Punto3D extends Punto {
    double x, y, z;
    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (p instanceof Punto3D) {
            Punto3D p3d = (Punto3D) p; // Downcasting seguro
            return Math.sqrt(Math.pow(this.x - p3d.x, 2) + Math.pow(this.y - p3d.y, 2) + Math.pow(this.z - p3d.z, 2));
        }
        throw new IllegalArgumentException("El punto destino debe ser 3D.");
    }
}

class Linea {
    private Punto p1;
    private Punto p2;

    public Linea(Punto p1, Punto p2) { this.p1 = p1; this.p2 = p2; }

    public double getLongitud() {
        return p1.calcularDistanciaA(p2); // Llamada polimórfica ciega a las dimensiones
    }
}
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces en Java es el mecanismo que permite que una interfaz derive directamente de otra preexistente, expandiendo el contrato original mediante la incorporación de nuevas firmas de métodos. Esta extensión se define utilizando la misma palabra reservada `extends` que rige en las clases. Cualquier clase concreta que decida implementar una interfaz derivada queda obligada contractualmente a desarrollar tanto los métodos definidos en la interfaz base como los nuevos métodos de la interfaz especializada.

A diferencia del rígido sistema de herencia simple de las clases convencionales, las interfaces en Java sí permiten la herencia múltiple real. Una única interfaz puede extender múltiples interfaces simultáneamente (separando sus nombres por comas). Esto no compromete la integridad del lenguaje porque las interfaces carecen de estado y de implementación inherente, eliminando de raíz cualquier posible ambigüedad de resolución o conflicto de variables.

```java
interface Fichero {
    String leerContenido();
}

// Herencia de interfaces: expande las capacidades del contrato base
interface FicheroEscribible extends Fichero {
    void escribirContenido(String datos);
    void eliminarFichero();
}

// La clase concreta asume la totalidad de las responsabilidades del contrato
class FicheroTexto implements FicheroEscribible {
    @Override
    public String leerContenido() { return "Datos de ejemplo..."; }
    
    @Override
    public void escribirContenido(String datos) { /* Lógica de escritura al disco */ }
    
    @Override
    public void eliminarFichero() { /* Lógica para eliminar el archivo */ }
}
```
