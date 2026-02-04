# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La **encapsulación** busca agrupar datos (atributos) y las operaciones que los manipulan (métodos) dentro de una misma unidad cohesiva llamada clase, creando así una frontera clara entre lo interno y lo externo de cada componente. Este principio permite estructurar el código de manera modular, donde cada clase representa una unidad autocontenida con responsabilidades bien definidas, similar a cómo en C se pueden agrupar funciones relacionadas en un mismo archivo `.c`, aunque sin las garantías formales que ofrece la POO.

La **ocultación de información** (también llamada *information hiding*) va un paso más allá de la encapsulación, estableciendo que los detalles internos de implementación de una clase deben mantenerse privados y ocultos al exterior, exponiendo solo aquellas operaciones necesarias a través de una interfaz pública bien definida. Esto significa que el código externo no puede ni debe acceder directamente a los datos internos de un objeto, sino que debe interactuar con él únicamente mediante los métodos públicos proporcionados. En C, aunque se pueden declarar funciones y variables como `static` para limitar su visibilidad al archivo actual, esto no ofrece el mismo nivel de control que los modificadores de acceso en POO.

Las ventajas principales de la ocultación de información incluyen: **mayor mantenibilidad** (los detalles internos pueden modificarse sin afectar al código que usa la clase), **protección de invariantes** (se garantiza que los datos permanezcan en estados válidos al controlar cómo se modifican), **reducción del acoplamiento** (el código externo no depende de los detalles de implementación, solo de la interfaz pública), y **facilidad de depuración** (al controlar todos los puntos de acceso a los datos, es más fácil localizar errores). Además, permite cambiar la representación interna de los datos sin afectar al resto del programa, siempre que la interfaz pública se mantenga consistente.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La **interfaz pública** de una clase es el conjunto de métodos y atributos marcados como públicos que definen cómo el código externo puede interactuar con los objetos de esa clase. Esta interfaz representa el "contrato" que la clase ofrece al resto del programa, especificando qué operaciones están disponibles y cómo invocarlas, sin revelar los detalles de cómo se implementan internamente. Es análogo a las declaraciones de funciones en un archivo `.h` en C, que muestran qué funciones están disponibles y sus firmas, sin mostrar su implementación que reside en el `.c`.

La interfaz pública se relaciona directamente con la ocultación de información porque actúa como la frontera entre lo que está visible y lo que permanece oculto. Todo aquello que no forma parte de la interfaz pública (atributos privados, métodos auxiliares internos) queda oculto al exterior, permitiendo que la implementación interna pueda modificarse libremente sin afectar al código que usa la clase. Esta separación entre interfaz e implementación es uno de los pilares fundamentales del diseño orientado a objetos, facilitando el mantenimiento y la evolución del software.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Diseñar cuidadosamente la interfaz pública es crucial porque una vez publicada y utilizada por código externo, resulta extremadamente difícil modificarla sin causar problemas. Cualquier cambio en la interfaz pública (eliminar métodos, modificar sus parámetros, cambiar tipos de retorno) romperá todo el código que dependa de ella, requiriendo actualizar potencialmente cientos o miles de líneas en múltiples archivos. Por el contrario, los detalles internos privados pueden modificarse libremente sin afectar a nadie, ya que están ocultos tras la interfaz pública.

Por esta razón, es fundamental diseñar interfaces públicas que sean **estables, completas y bien pensadas** desde el inicio, anticipando usos futuros pero sin exponer más de lo necesario. Una buena práctica es mantener la interfaz pública lo más pequeña posible (principio de mínima exposición), ya que cuanto menos se exponga, mayor libertad se tendrá para modificar la implementación interna en el futuro. Esto es análogo a definir cuidadosamente las funciones públicas en un archivo `.h` de C, sabiendo que cambiarlas después obligaría a recompilar y modificar todo código que las use.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son condiciones o propiedades que deben cumplirse siempre para todos los objetos de una clase, garantizando que los datos internos permanezcan en un estado válido y consistente en todo momento. Por ejemplo, en una clase `CuentaBancaria` una invariante podría ser que el saldo nunca sea negativo, o en una clase `Fecha` que el día esté entre 1 y 31, el mes entre 1 y 12, y que las combinaciones sean válidas (no permitir 31 de febrero). Estas restricciones definen qué estados son válidos y cuáles no, siendo responsabilidad de la clase mantener estas garantías.

La ocultación de información es fundamental para preservar las invariantes de clase porque al mantener los atributos privados, se fuerza que cualquier modificación del estado interno pase obligatoriamente por métodos públicos controlados. Estos métodos pueden validar los datos antes de modificarlos, rechazando operaciones que violarían las invariantes. Si los atributos fueran públicos, cualquier código externo podría modificarlos directamente sin ningún control, haciendo imposible garantizar las invariantes. Por ejemplo, si el atributo `saldo` de `CuentaBancaria` fuera público, nada impediría que código externo lo estableciera a un valor negativo directamente mediante `cuenta.saldo = -100`.

En C, aunque se pueden usar `struct` para agrupar datos relacionados, no existe un mecanismo formal para proteger estos datos ni para garantizar invariantes, ya que cualquier código con acceso a la estructura puede modificar sus campos libremente. La POO resuelve este problema mediante la combinación de encapsulación con modificadores de acceso, asegurando que las invariantes se mantengan a lo largo de toda la vida del objeto.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

```java
public class Punto {
    private double x;
    private double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    public double getX() {
        return x;
    }
    
    public double getY() {
        return y;
    }
}
```

La **interfaz pública** de esta clase `Punto` está formada por el constructor `Punto(double x, double y)`, el método `calcularDistanciaAOrigen()`, y los métodos *getters* `getX()` y `getY()`. Estos son los únicos elementos accesibles desde código externo, definiendo cómo se puede crear un punto, consultar sus coordenadas y calcular su distancia al origen. Los atributos `x` e `y` permanecen privados y ocultos, no siendo accesibles directamente desde fuera de la clase.

El modificador **`private`** indica que un miembro (atributo o método) solo es accesible desde dentro de la propia clase, manteniéndolo oculto al exterior. Esto protege los datos internos de modificaciones no controladas, similar a declarar variables o funciones como `static` en C para limitar su alcance al archivo actual, aunque con mayor control. Por otro lado, el modificador **`public`** hace que un miembro sea accesible desde cualquier parte del código, formando parte de la interfaz pública de la clase. Al mantener los atributos privados y proporcionar métodos públicos para interactuar con ellos, se consigue la ocultación de información que permite cambiar la implementación interna sin afectar al código que usa la clase.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores `public` y `private` pueden aplicarse a **clases**, **atributos**, **métodos** y **constructores**. Para las clases de nivel superior (no anidadas), solo `public` es aplicable junto con el modificador por defecto (sin especificar ninguno); una clase declarada como `public` es accesible desde cualquier paquete, mientras que una sin modificador (por defecto) solo es accesible dentro de su propio paquete. Las clases anidadas (clases definidas dentro de otras clases) sí pueden ser `private`, limitando su visibilidad a la clase que las contiene.

Los **atributos** y **métodos** pueden ser `public` (accesibles desde cualquier parte), `private` (accesibles solo dentro de la misma clase), o usar otros modificadores de visibilidad como `protected` (accesibles en la misma clase, subclases y mismo paquete) o el modificador por defecto también conocido como *package-private* (accesibles solo dentro del mismo paquete). Los **constructores** también pueden llevar estos modificadores: un constructor `private` impide que código externo cree instancias directamente, siendo útil para patrones como Singleton o cuando se quiere controlar la creación mediante métodos factoría estáticos. Es importante notar que a diferencia de C++, en Java no existe el concepto de `friend` para otorgar acceso especial entre clases.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de `public` y `private`, existen otros niveles de visibilidad en la mayoría de lenguajes orientados a objetos. En Java, hay cuatro niveles de visibilidad: **`public`** (accesible desde cualquier lugar), **`private`** (accesible solo dentro de la misma clase), **`protected`** (accesible en la misma clase, subclases y clases del mismo paquete), y el **modificador por defecto o *package-private*** (sin especificar ningún modificador, accesible solo dentro del mismo paquete). El modificador `protected` es especialmente útil cuando se trabaja con herencia, permitiendo que las subclases accedan a miembros que no están completamente públicos pero que necesitan ser accesibles en la jerarquía de herencia.

Otros lenguajes orientados a objetos tienen sus propias variantes de visibilidad. C++ ofrece `public`, `private` y `protected` con semántica ligeramente diferente a Java (no hay concepto de paquete), además del mecanismo de clases `friend` que permite otorgar acceso especial a funciones o clases específicas. Python adopta un enfoque más flexible basado en convenciones: los atributos que comienzan con un guion bajo `_atributo` son considerados privados por convención (aunque técnicamente accesibles), y los que comienzan con doble guion bajo `__atributo` activan *name mangling* dificultando su acceso desde fuera. C# es muy similar a Java con `public`, `private`, `protected` e `internal` (equivalente al package-private de Java), además de `protected internal` que combina ambos.

La elección del nivel de visibilidad adecuado es una decisión de diseño importante: generalmente se recomienda usar la visibilidad más restrictiva posible, ampliándola solo cuando sea necesario, siguiendo el principio de mínimo privilegio.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros privados están ocultos para **(a) otras clases**, NO para otras instancias de la misma clase. Esto significa que un objeto puede acceder a los atributos privados de otro objeto de su misma clase dentro de los métodos de esa clase. Esta característica puede resultar sorprendente al principio, pero es fundamental para implementar operaciones que involucren múltiples instancias de la misma clase.

```java
public class Punto {
    private double x;
    private double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;  // Acceso a otro.x (privado)
        double dy = this.y - otro.y;  // Acceso a otro.y (privado)
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En el método `calcularDistanciaAPunto`, el código accede directamente a `otro.x` y `otro.y`, aunque estos atributos son privados. Esto es posible porque el método pertenece a la clase `Punto`, y el control de acceso en Java (y en la mayoría de lenguajes POO) se aplica a nivel de **clase**, no de **instancia**. Si el control fuera por instancia, cada objeto solo podría acceder a sus propios atributos privados, haciendo imposible implementar métodos como este sin usar getters, lo cual sería innecesariamente restrictivo y verboso. Este enfoque facilita la implementación de operaciones entre objetos de la misma clase mientras mantiene la protección frente a clases externas.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos **getter** (o *accesores*) y **setter** (o *mutadores*) son métodos públicos que proporcionan acceso controlado a los atributos privados de una clase. Un *getter* es un método que devuelve el valor de un atributo privado, permitiendo que código externo consulte ese valor sin acceder directamente al atributo. Un *setter* es un método que permite modificar el valor de un atributo privado, ofreciendo un punto de control donde se pueden validar los nuevos valores antes de aplicarlos, garantizando así las invariantes de clase. Por convención en Java, estos métodos se nombran con los prefijos `get` y `set` seguidos del nombre del atributo con la primera letra en mayúscula, por ejemplo `getX()` y `setX(double nuevoX)`.

La ventaja de usar getters y setters en lugar de atributos públicos es que proporcionan un punto de indirección que permite añadir lógica adicional sin cambiar la interfaz pública. Por ejemplo, un setter puede validar que el valor esté en un rango válido, lanzar excepciones si es inválido, actualizar otros atributos relacionados, o registrar cambios. Además, permiten cambiar la representación interna sin afectar al código externo: se podría pasar de almacenar un valor directamente a calcularlo dinámicamente, manteniendo el mismo getter. En C, el acceso a campos de un `struct` es siempre directo, sin posibilidad de interponer lógica de validación, mientras que los getters/setters de POO proporcionan esta flexibilidad esencial para mantener la integridad de los datos y las invariantes de clase.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, cuando se habla de "seguridad" en el contexto de la ocultación de información en POO, **no se refiere a seguridad informática** frente a ataques o hackers, sino a la **seguridad en el diseño y corrección del programa**. Se trata de proteger la integridad lógica del programa, garantizando que los objetos mantengan estados válidos y que las operaciones se realicen de manera correcta y predecible. Es análogo a usar `const` en C para proteger datos de modificaciones accidentales, no para prevenir accesos maliciosos.

La "seguridad" que proporciona la encapsulación se refiere a prevenir errores de programación accidentales, asegurar que las invariantes de clase se respeten, y evitar efectos secundarios inesperados. Por ejemplo, si un atributo `saldo` en una clase `CuentaBancaria` es privado con un setter que valida que no sea negativo, se está protegiendo contra errores lógicos donde el programador podría establecer accidentalmente un valor inválido. Esta protección no impide que un atacante con acceso al sistema o mediante reflexión pueda manipular los datos, sino que protege contra usos incorrectos durante el desarrollo normal del software, facilitando la detección temprana de errores y mejorando la robustez del código.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Los **miembros de instancia** son atributos o métodos que pertenecen a cada objeto individual, de forma que cada instancia tiene su propia copia de los atributos y los métodos operan sobre el estado específico de ese objeto. Por ejemplo, en la clase `Punto`, cada instancia tiene sus propios valores de `x` e `y` independientes. Los **miembros de clase** (también llamados *miembros estáticos*), marcados con `static` en Java, pertenecen a la clase en sí misma y no a las instancias individuales, siendo compartidos por todos los objetos de esa clase. Existe una única copia de cada atributo estático, independientemente de cuántas instancias se creen, similar a las variables globales en C pero con alcance limitado a la clase.

Los miembros de clase sí pueden ocultarse usando modificadores de visibilidad como `private` o `public`. Un atributo estático privado será accesible solo desde métodos de la misma clase (ya sean estáticos o de instancia), mientras que un atributo estático público será accesible desde cualquier lugar usando el nombre de la clase (por ejemplo, `Punto.contadorPuntos`). Los métodos estáticos también pueden ser privados, resultando útiles como funciones auxiliares internas que no deben exponerse en la interfaz pública. Por ejemplo, la clase `Math` en Java tiene métodos estáticos públicos como `Math.sqrt()`, pero podría tener métodos privados auxiliares para cálculos internos que no se exponen al exterior, manteniendo así la encapsulación a nivel de clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido en varios patrones de diseño importantes. Un constructor privado impide que código externo cree instancias directamente usando `new`, pero la clase puede crear instancias desde sus propios métodos estáticos públicos, conocidos como **métodos factoría**. Este patrón permite controlar completamente cómo y cuándo se crean los objetos, validar parámetros, reutilizar instancias existentes, o devolver subtipos diferentes según las necesidades.

Los casos de uso más comunes incluyen el **patrón Singleton** (garantizar que solo exista una única instancia de la clase), **clases con métodos factoría** que proporcionan formas alternativas de construcción con nombres más descriptivos que múltiples constructores sobrecargados, y **clases utilitarias** que solo contienen métodos estáticos y no deberían instanciarse nunca (como la clase `Math` de Java). Por ejemplo, una clase `Configuracion` podría tener un constructor privado y un método `getInstance()` para implementar un Singleton, asegurando que todas las partes del programa usen la misma configuración compartida.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Los miembros de clase en Java se indican mediante la palabra clave **`static`** antes del tipo del atributo o del tipo de retorno del método. Estos miembros pertenecen a la clase en sí misma y no a instancias individuales, siendo compartidos por todos los objetos de esa clase. Se puede acceder a ellos usando el nombre de la clase (por ejemplo, `Punto.maxX`) o, menos recomendable, a través de una instancia.

```java
public class Punto {
    private double x;
    private double y;
    
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        
        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    public static double getMaxX() {
        return maxX;
    }
    
    public static double getMaxY() {
        return maxY;
    }
}
```

En este ejemplo, `maxX` y `maxY` son atributos estáticos privados que mantienen los valores máximos de coordenadas vistos hasta el momento, compartidos por todas las instancias. Cada vez que se crea un `Punto`, el constructor actualiza estos valores si las coordenadas del nuevo punto son mayores. Los métodos estáticos `getMaxX()` y `getMaxY()` permiten consultar estos valores desde fuera de la clase mediante `Punto.getMaxX()` y `Punto.getMaxY()`. Es importante notar que los miembros estáticos también pueden ocultarse usando `private`, de forma que `maxX` y `maxY` no son accesibles directamente desde el exterior, solo a través de los getters públicos.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

```java
public static Punto crearConCoordendasRedondeadas(double x, double y) {
    double xRedondeada = Math.round(x);
    double yRedondeada = Math.round(y);
    return new Punto(xRedondeada, yRedondeada);
}
```

Sí, se ha usado `static` porque un **método factoría** debe ser estático. La razón es que este método debe poder invocarse sin tener primero una instancia de la clase, ya que su propósito es precisamente crear nuevas instancias. Se invoca usando el nombre de la clase: `Punto p = Punto.crearConCoordendasRedondeadas(3.7, 4.2);`, lo cual resulta más descriptivo que simplemente usar el constructor con redondeo manual.

Los métodos factoría estáticos ofrecen ventajas sobre los constructores: pueden tener nombres descriptivos que clarifican su propósito (mientras que múltiples constructores sobrecargados pueden ser confusos), pueden devolver instancias previamente creadas en lugar de crear siempre nuevas (útil para caché), y pueden devolver subtipos diferentes según la lógica. En este caso, el nombre `crearConCoordendasRedondeadas` explica claramente qué hace el método, siendo más legible que un constructor `Punto(double x, double y, boolean redondear)` con un flag booleano.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

```java
public class Punto {
    private double[] coordenadas;
    
    public Punto(double x, double y) {
        coordenadas = new double[2];
        coordenadas[0] = x;
        coordenadas[1] = y;
    }
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + 
                        coordenadas[1] * coordenadas[1]);
    }
    
    public double getX() {
        return coordenadas[0];
    }
    
    public double getY() {
        return coordenadas[1];
    }
    
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coordenadas[0] - otro.coordenadas[0];
        double dy = this.coordenadas[1] - otro.coordenadas[1];
        return Math.sqrt(dx * dx + dy * dy);
    }
    
    public static Punto crearConCoordendasRedondeadas(double x, double y) {
        double xRedondeada = Math.round(x);
        double yRedondeada = Math.round(y);
        return new Punto(xRedondeada, yRedondeada);
    }
}
```

Este ejemplo ilustra perfectamente el valor de la ocultación de información: se ha cambiado completamente la representación interna de la clase (de dos atributos `x` e `y` a un array `coordenadas`), pero **la interfaz pública permanece idéntica**. Todo el código externo que utilizaba la clase `Punto` seguirá funcionando sin modificaciones, ya que los métodos públicos (`getX()`, `getY()`, `calcularDistanciaAOrigen()`, etc.) mantienen la misma firma y comportamiento.

Esta flexibilidad para cambiar la implementación sin afectar al código cliente es una de las mayores ventajas de la encapsulación. En C, si un `struct Punto` tuviera campos `x` e `y` accesibles directamente y se decidiera cambiarlos por un array, habría que modificar todo el código que accediera a esos campos. Con la encapsulación adecuada en POO, estos cambios internos quedan completamente aislados del resto del programa, facilitando el mantenimiento y la evolución del software.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Aunque pueda parecer que un atributo con getter y setter públicos es equivalente a un atributo público, **no es así**. Los métodos getter y setter proporcionan un punto de control e indirección que ofrece flexibilidad fundamental. Un setter puede incluir lógica de validación para asegurar que el valor asignado respeta las invariantes de clase (por ejemplo, verificar que un `edad` no sea negativa), puede actualizar otros atributos relacionados cuando cambia uno, o registrar cambios para auditoría. Un getter podría calcular el valor dinámicamente en lugar de devolverlo almacenado, o devolver una copia en lugar de la referencia directa para evitar modificaciones externas. Con un atributo público, no existe esta posibilidad de interponer lógica ni de cambiar la implementación futura.

La **convención más habitual y recomendada** es declarar todos los atributos como **privados** y proporcionar getters y setters públicos solo cuando sea necesario. Esta práctica sigue el principio de mínima exposición y máxima encapsulación. Mantener los atributos privados permite cambiar la representación interna en el futuro sin romper código externo, añadir validaciones posteriormente, o hacer que un valor pase de ser almacenado a calculado de forma transparente. En C, los campos de un `struct` son típicamente públicos por defecto, lo que limita esta flexibilidad.

Esto está directamente relacionado con las **invariantes de clase**: si los atributos fueran públicos, cualquier código externo podría modificarlos directamente sin pasar por validaciones, haciendo imposible garantizar las invariantes. Por ejemplo, en una clase `Rectangulo` con atributos `ancho` y `alto`, un setter puede validar que ambos sean positivos antes de asignarlos, manteniendo la invariante de que no existen rectángulos con dimensiones negativas o cero. Si estos atributos fueran públicos, nada impediría código externo establecer `rectangulo.ancho = -5`, violando la invariante.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es **inmutable** cuando sus instancias no pueden cambiar su estado después de ser creadas, es decir, no existen métodos que modifiquen los valores de sus atributos una vez inicializados en el constructor. Por ejemplo, la clase `String` en Java es inmutable: una vez creada una cadena, no se puede cambiar; cualquier operación que parezca modificarla en realidad crea un nuevo objeto `String`. Para lograr la inmutabilidad, todos los atributos deben ser `private` y `final`, no debe haber setters, y cualquier método que parezca modificar el estado debe devolver un nuevo objeto en lugar de modificar el existente.

Un **método modificador** (o *mutador*) es cualquier método que cambia el estado interno del objeto, alterando el valor de uno o más atributos. No todos los métodos modificadores son setters: mientras que un setter simplemente asigna un nuevo valor a un atributo (`setX(double x)`), otros métodos modificadores realizan operaciones más complejas. Por ejemplo, en una clase `CuentaBancaria`, métodos como `depositar(double cantidad)` o `retirar(double cantidad)` son modificadores que alteran el `saldo`, pero no son setters simples porque incluyen lógica adicional de validación y quizás actualización de otras variables.

Las clases inmutables tienen importantes ventajas: son **inherentemente thread-safe** (seguras en entornos multihilo sin necesidad de sincronización porque no pueden cambiar), más fáciles de razonar y depurar (su estado no cambia inesperadamente), pueden compartirse libremente sin riesgo de efectos secundarios, y funcionan correctamente como claves en estructuras como `HashMap` o `HashSet`. Por estas razones, se recomienda diseñar clases inmutables siempre que sea posible, especialmente para objetos que representan valores conceptuales como fechas, puntos, coordenadas o dinero. El principal inconveniente es que cada modificación requiere crear un nuevo objeto, lo cual puede ser ineficiente si se realizan muchas operaciones consecutivas.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, **no es recomendable incluir setters automáticamente como convención**. Los setters deben añadirse solo cuando existe una necesidad real de modificar ese atributo después de la construcción del objeto. Incluir setters innecesariamente hace que la clase sea mutable cuando podría ser inmutable, perdiendo las ventajas de seguridad, simplicidad y thread-safety que ofrece la inmutabilidad. Además, cada setter es parte de la interfaz pública que deberá mantenerse indefinidamente una vez publicada, limitando la flexibilidad futura de cambiar la implementación interna.

La buena práctica es diseñar clases lo más inmutables posible, proporcionando todos los valores necesarios en el constructor y omitiendo setters. Si posteriormente se necesita "modificar" el objeto, se puede devolver una nueva instancia con los valores cambiados en lugar de mutar el existente. Los getters son menos problemáticos y pueden proporcionarse más liberalmente ya que solo permiten consultar, no modificar, aunque tampoco deben generarse automáticamente sin pensar si realmente son necesarios en la interfaz pública. El principio general es exponer lo mínimo necesario, añadiendo métodos a la interfaz pública solo cuando exista un caso de uso claro que lo justifique.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase `String` en Java es **inmutable**: una vez creada, su contenido no puede modificarse. Todas las operaciones que parecen modificar una cadena (como `toUpperCase()`, `substring()`, `replace()`, etc.) en realidad devuelven un **nuevo objeto** `String` con el resultado, dejando el original intacto. Esta inmutabilidad proporciona ventajas de seguridad, thread-safety y permite que las cadenas se compartan eficientemente en memoria, pero tiene implicaciones de rendimiento en ciertas operaciones.

Al concatenar dos cadenas usando el operador `+` o el método `concat()`, se crea un nuevo objeto `String` que contiene el resultado de la concatenación. Si se realizan múltiples concatenaciones en un bucle (por ejemplo, concatenar miles de fragmentos), cada operación crea un nuevo objeto y copia todo el contenido anterior más el nuevo fragmento, resultando en una complejidad temporal de O(n²) y generando muchos objetos temporales que luego debe recolectar el garbage collector. Esto es similar al problema de usar `strcat()` repetidamente en C, donde cada concatenación debe recorrer toda la cadena existente.

Para construir cadenas largas mediante concatenaciones sucesivas, se debe usar la clase **`StringBuilder`** (o `StringBuffer` si se necesita sincronización para multihilo), que es una versión **mutable** de cadenas diseñada específicamente para este propósito. `StringBuilder` mantiene un buffer interno que puede crecer dinámicamente, permitiendo añadir fragmentos eficientemente con el método `append()` en complejidad O(1) amortizado. Una vez completada la construcción, se puede obtener el `String` final inmutable llamando a `toString()`. Por ejemplo: `StringBuilder sb = new StringBuilder(); for(...) { sb.append(fragmento); } String resultado = sb.toString();`


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En POO, la comparación de objetos puede realizarse de dos formas fundamentales: por **identidad** (si son exactamente el mismo objeto en memoria) o por **contenido** (si tienen los mismos valores en sus atributos). El operador `==` en Java compara identidad, es decir, verifica si dos referencias apuntan al mismo objeto en memoria, similar a comparar punteros en C. Para comparar por contenido, se debe usar el método **`equals()`**, que es el mecanismo estándar en POO para definir qué significa que dos objetos sean "iguales" semánticamente.

El método `equals()` está definido en la clase `Object` (clase base de todas las clases en Java) y por defecto implementa comparación por identidad (equivalente a `==`), lo cual raramente es el comportamiento deseado. Por esta razón, las clases deben **sobrescribir** el método `equals()` para implementar comparación por contenido según su semántica específica. Por ejemplo, dos objetos `Punto` deberían considerarse iguales si tienen las mismas coordenadas `x` e `y`, independientemente de ser objetos distintos en memoria. Al sobrescribir `equals()`, también debe sobrescribirse el método `hashCode()` para mantener el contrato de que objetos iguales según `equals()` deben tener el mismo hash code, esencial para el correcto funcionamiento de colecciones como `HashMap` o `HashSet`.

Para comparar dos cadenas en Java, **nunca se debe usar `==`** (que compara referencias, no contenido), sino el método **`equals()`**: `str1.equals(str2)` o `str1.equalsIgnoreCase(str2)` para comparación ignorando mayúsculas/minúsculas. Esto es diferente a C donde `strcmp(str1, str2) == 0` compara el contenido de las cadenas. En Java, debido a la optimización de *string pooling* (cadenas literales idénticas pueden compartir la misma instancia), a veces `==` puede dar `true` por casualidad, pero nunca debe confiarse en esto ya que no es garantizado y puede llevar a errores sutiles. Siempre debe usarse `equals()` para comparar contenido de objetos, incluyendo cadenas.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases **wrapper** (o *envoltorias*) son clases que encapsulan tipos primitivos para poder tratarlos como objetos. En Java existen ocho tipos primitivos (`int`, `double`, `boolean`, `char`, `byte`, `short`, `long`, `float`) que no son objetos por razones de eficiencia, y sus correspondientes clases wrapper (`Integer`, `Double`, `Boolean`, `Character`, `Byte`, `Short`, `Long`, `Float`) que los envuelven. Por ejemplo, `Integer` es la clase wrapper que envuelve el tipo primitivo `int`, proporcionando métodos útiles como `parseInt()`, `toString()`, o `compareTo()`.

El proceso de convertir un primitivo a su wrapper se llama **boxing** (encajonamiento), y el proceso inverso se llama **unboxing** (desencajonamiento). En versiones antiguas de Java esto debía hacerse manualmente (`Integer i = new Integer(5);`), pero desde Java 5 existe el **autoboxing/autounboxing**, un mecanismo automático del compilador que convierte transparentemente entre primitivos y wrappers cuando es necesario. Por ejemplo, `Integer x = 5;` automáticamente envuelve el `int` en un `Integer`, y `int y = x;` automáticamente extrae el valor primitivo. Sin embargo, hay que tener cuidado con el rendimiento en bucles intensivos, ya que el boxing/unboxing repetido crea objetos y tiene overhead.

Las ventajas de los wrappers incluyen: poder usar primitivos en **colecciones genéricas** que solo aceptan objetos (como `ArrayList<Integer>`), aprovechar métodos de utilidad para conversiones y operaciones, permitir valores `null` (mientras que primitivos siempre tienen un valor), y tratarlos uniformemente con otros objetos. No todos los lenguajes POO tienen esta dualidad: lenguajes como **Python, Ruby y Smalltalk** tratan todo como objetos (incluso los números), sin tipos primitivos separados, eliminando la necesidad de wrappers. C# tiene un sistema de tipos unificado donde los tipos primitivos (value types) automáticamente se comportan como objetos cuando es necesario mediante *boxing implícito*, aunque internamente mantiene la distinción por eficiencia.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un **tipo de dato enumerado** (o *enum*) es un tipo especial que representa un conjunto fijo y limitado de valores constantes, donde cada valor es una instancia con nombre del tipo. Por ejemplo, un enumerado `DiaSemana` podría tener exactamente siete valores posibles: `LUNES`, `MARTES`, `MIERCOLES`, etc. Los enumerados reemplazan el patrón de C de definir constantes enteras mediante `#define` o `const int`, ofreciendo seguridad de tipos ya que no se pueden asignar valores arbitrarios: una variable de tipo `DiaSemana` solo puede contener uno de los siete valores definidos o `null`.

En Java, un enumerado **sí es una clase**, específicamente una clase especial que extiende automáticamente de `java.lang.Enum`. Cada valor del enumerado es una instancia estática y final de esa clase, creada automáticamente una única vez. Esto significa que los enumerados en Java pueden tener constructores, atributos y métodos como cualquier clase, permitiendo asociar datos y comportamiento a cada valor. Por ejemplo, cada día de la semana podría tener un atributo que indique si es laborable o no, y métodos para consultarlo. Esta capacidad va mucho más allá de los simples enumerados de C, que son solo alias de enteros sin comportamiento asociado.

En términos de encapsulación, los enumerados en Java ofrecen ventajas significativas: pueden tener **atributos privados** con valores específicos para cada constante enumerada, **constructores privados** (los enumerados no permiten constructores públicos porque las instancias están predefinidas), y **métodos públicos** que operan sobre esos atributos privados. Esto permite ocultar detalles de implementación mientras se proporciona una interfaz pública rica. Por ejemplo, un enum `Mes` podría tener un atributo privado `numeroDias` inicializado en el constructor privado con valores diferentes para cada mes, y un método público `getDias()` para consultarlo, manteniendo la encapsulación y garantizando que los valores no puedan modificarse desde el exterior. Además, garantizan que no se pueden crear nuevas instancias fuera de las predefinidas, proporcionando un control total sobre el conjunto de valores posibles.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

```java
public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);
    
    private final int ordinal;
    private final int numeroDias;
    
    private Mes(int ordinal, int numeroDias) {
        this.ordinal = ordinal;
        this.numeroDias = numeroDias;
    }
    
    public int getDias() {
        return numeroDias;
    }
    
    public int getOrdinal() {
        return ordinal;
    }
    
    public boolean esDePrimavera(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return ordinal >= 3 && ordinal <= 5;  // marzo, abril, mayo
        } else {
            return ordinal >= 9 && ordinal <= 11;  // septiembre, octubre, noviembre
        }
    }
    
    public boolean esDeVerano(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return ordinal >= 6 && ordinal <= 8;  // junio, julio, agosto
        } else {
            return ordinal == 12 || ordinal <= 2;  // diciembre, enero, febrero
        }
    }
    
    public boolean esDeOtonio(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return ordinal >= 9 && ordinal <= 11;  // septiembre, octubre, noviembre
        } else {
            return ordinal >= 3 && ordinal <= 5;  // marzo, abril, mayo
        }
    }
    
    public boolean esDeInvierno(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return ordinal == 12 || ordinal <= 2;  // diciembre, enero, febrero
        } else {
            return ordinal >= 6 && ordinal <= 8;  // junio, julio, agosto
        }
    }
}
```

Este enumerado demuestra el poder de la encapsulación en Java: cada mes tiene atributos privados (`ordinal` y `numeroDias`) que solo pueden establecerse mediante el constructor privado, haciéndolos inmutables y protegidos. Los métodos públicos (`getDias()`, `getOrdinal()`, y los de estaciones) proporcionan la interfaz para consultar información sobre cada mes, ocultando los detalles de cómo se determina la estación. Las instancias del enum son constantes predefinidas (`ENERO`, `FEBRERO`, etc.) que no pueden modificarse ni crearse nuevas desde el exterior.

El uso sería: `Mes mes = Mes.MARZO; int dias = mes.getDias(); // 31; boolean esPrimavera = mes.esDePrimavera(true); // true en hemisferio norte`. Este diseño es muy superior a usar constantes enteras como en C (`#define ENERO 1`), ya que proporciona seguridad de tipos, comportamiento asociado, y garantiza que solo existen los doce valores válidos definidos.
