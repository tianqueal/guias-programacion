# Tema 4.1. Composición

## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En C, la composición se modela de forma natural con `struct`: una estructura puede contener otras como campos. En este caso, una `Linea` "tiene" dos `Punto` (`inicio` y `fin`), y cada `Punto` encapsula sus dos coordenadas (`x`, `y`). Aunque C no tiene clases, el patrón conceptual de composición ya está presente.

Para calcular la longitud de una línea, basta reutilizar una función de distancia entre dos puntos y aplicarla a los extremos de la línea. Este enfoque evita duplicar lógica: la operación de nivel superior (`longitudLinea`) se construye sobre otra de nivel inferior (`distancia`), reflejando una composición limpia también a nivel de funciones.

```c
#include <math.h>
#include <stdio.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto inicio;
    Punto fin;
} Linea;

double distancia(Punto a, Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

double longitudLinea(Linea l) {
    return distancia(l.inicio, l.fin);
}

int main(void) {
    Linea l = {{0.0, 0.0}, {3.0, 4.0}};
    printf("Longitud: %.2f\n", longitudLinea(l)); /* 5.00 */
    return 0;
}
```


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

En Java, la misma idea se expresa mediante clases: `Linea` se compone de dos objetos `Punto`. Para garantizar inmutabilidad, los atributos se declaran `private final`, se inicializan en el constructor y no se ofrecen *setters*. Así, ni el punto ni la línea pueden cambiar de estado tras crearse.

Esta estrategia supera al enfoque típico de C en robustez: al no poder modificarse el estado interno desde fuera, se preservan mejor las invariantes y se evita que otras partes del programa dejen objetos en estados inconsistentes. La interfaz pública queda reducida a operaciones de consulta (`getX`, `getY`, `distancia`, `longitud`).

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

```java
public final class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        if (inicio == null || fin == null) {
            throw new IllegalArgumentException("Los puntos no pueden ser null");
        }
        this.inicio = inicio;
        this.fin = fin;
    }

    public Punto getInicio() { return inicio; }
    public Punto getFin() { return fin; }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad indica cuántas instancias de una clase pueden asociarse con una instancia de otra clase en una relación de composición o asociación. Es, en esencia, una restricción de cardinalidad del modelo: "exactamente una", "cero o más", "una o varias", etc.

En el ejemplo dado, de `Linea` a `Punto` la multiplicidad es **2** (o `2..2`), porque una línea tiene exactamente dos puntos: inicio y fin. No tiene sentido una `Linea` con menos ni con más en ese modelo.

De `Punto` a `Linea`, la multiplicidad es **0..***, porque un mismo punto puede no pertenecer a ninguna línea, o pertenecer a muchas (por ejemplo, vértice compartido entre segmentos). Esta segunda multiplicidad no obliga a almacenar una colección de líneas dentro de `Punto`; solo expresa lo que permite el modelo conceptual.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

En composición **fuerte**, el objeto contenido forma parte del objeto contenedor y su ciclo de vida queda ligado a él. Si el contenedor deja de existir, los contenidos también dejan de tener sentido dentro del modelo. Es una relación de "parte-todo" con dependencia fuerte de existencia.

En composición **débil**, el contenedor solo mantiene una referencia a objetos que pueden existir independientemente. El contenedor "usa" o "agrupa" esos objetos, pero no es su dueño exclusivo. Por ello, los objetos relacionados pueden sobrevivir aunque el contenedor desaparezca.

Habitualmente, en UML y en diseño OO, se llama **composición** (en sentido estricto) a la fuerte, y **agregación/asociación** a la débil. La diferencia práctica más importante está en el ciclo de vida y en la propiedad conceptual de los objetos contenidos.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

En esos casos se habla, en general, de **dependencia**. Una clase depende de otra cuando la necesita para implementar algún comportamiento puntual, pero sin mantener necesariamente una relación estructural estable como atributo del objeto.

La composición implica que la relación forme parte del estado del objeto (normalmente como campo), no solo del cuerpo de un método. Por ejemplo, crear temporalmente un `Punto` dentro de un método de `Linea` no convierte automáticamente esa relación en composición; puede ser simplemente colaboración momentánea.

Por tanto, parámetros, variables locales y objetos creados al vuelo suelen modelar dependencia. Se reserva "composición" para relaciones parte-todo persistentes en la estructura de las clases.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

La versión fuerte puede modelarse haciendo que `Linea` cree internamente sus puntos a partir de valores primitivos. Así, desde fuera no se comparten instancias de `Punto`: los puntos nacen dentro de la línea y conceptualmente pertenecen a ella.

La versión débil se modela recibiendo referencias a `Punto` ya existentes en el constructor. En ese caso, `Linea` no es propietaria del ciclo de vida de los puntos; simplemente mantiene referencias a objetos creados en otro lugar del sistema.

```java
/* Composición fuerte: Linea crea sus propios puntos */
public final class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```

```java
/* Composición débil (agregación): Linea referencia puntos externos */
public final class LineaDebil {
    private final Punto inicio;
    private final Punto fin;

    public LineaDebil(Punto inicio, Punto fin) {
        if (inicio == null || fin == null) {
            throw new IllegalArgumentException("Los puntos no pueden ser null");
        }
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java no existe destrucción manual explícita como `free` o `delete`. Cuando un objeto contenedor deja de ser alcanzable (por ejemplo, una `Linea` que ya no está referenciada), también dejan de ser alcanzables sus objetos internos, salvo que existan otras referencias externas a ellos.

Una vez los objetos se vuelven inaccesibles, el recolector de basura (GC) puede liberar su memoria en un momento no determinista. Por eso no se observa código de destrucción explícita en `Linea`: la gestión de memoria es automática y la JVM decide cuándo ejecutar la recolección.

En composición fuerte bien diseñada, al no exponer referencias mutables a los componentes internos, es natural que su vida útil quede alineada con la del contenedor. No se "destruyen" en una línea concreta de código, sino cuando pasan a ser basura para el GC.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

Este caso es una agregación (composición débil): `Departamento` referencia objetos `Profesor` que pueden existir independientemente. La invariante clave es doble: debe existir siempre un director y ese director debe pertenecer a la colección de profesores del departamento.

Para respetar encapsulación, no se expone el array interno; se ofrece una interfaz pública basada en operaciones (`getNumeroProfesores`, `getProfesor`, `agregarProfesor`, `eliminarProfesor`, `setDirector`). También se validan posiciones, capacidad máxima y pertenencia del director, lanzando excepciones cuando se incumple el contrato.

```java
public final class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede estar vacio");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public final class Departamento {
    private static final int MAX_PROFESORES = 50;
    private final Profesor[] profesores = new Profesor[MAX_PROFESORES];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir director inicial");
        }
        profesores[0] = directorInicial;
        numProfesores = 1;
        director = directorInicial;
    }

    public int getNumeroProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        validarPosicion(pos);
        return profesores[pos];
    }

    public Profesor getDirector() {
        return director;
    }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        if (!contiene(nuevoDirector)) {
            throw new IllegalStateException("El director debe pertenecer al departamento");
        }
        director = nuevoDirector;
    }

    public void agregarProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser null");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("Capacidad maxima alcanzada");
        }
        profesores[numProfesores] = profesor;
        numProfesores++;
    }

    public void eliminarProfesor(int pos) {
        validarPosicion(pos);
        Profesor aEliminar = profesores[pos];
        if (aEliminar == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    private void validarPosicion(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posicion invalida: " + pos);
        }
    }

    private boolean contiene(Profesor profesor) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == profesor) { /* identidad de objeto */
                return true;
            }
        }
        return false;
    }
}
```


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

Con `List<Profesor>` se simplifica mucho la implementación porque la gestión de capacidad y desplazamientos al eliminar deja de ser manual. Se elimina código de bajo nivel (array fijo, bucles de corrimiento, contador manual de huecos), quedando el foco en las invariantes de dominio.

El riesgo de devolver directamente la lista interna es romper encapsulación: código externo podría añadir/eliminar profesores sin pasar por las validaciones, o incluso dejar al director fuera de la lista. Para evitarlo, se debe devolver una vista inmodificable o una copia defensiva.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public final class DepartamentoConList {
    private static final int MAX_PROFESORES = 50;
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoConList(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir director inicial");
        }
        profesores.add(directorInicial);
        director = directorInicial;
    }

    public int getNumeroProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public Profesor getDirector() {
        return director;
    }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El director debe pertenecer al departamento");
        }
        director = nuevoDirector;
    }

    public void agregarProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("El profesor no puede ser null");
        }
        if (profesores.size() >= MAX_PROFESORES) {
            throw new IllegalStateException("Capacidad maxima alcanzada");
        }
        profesores.add(profesor);
    }

    public void eliminarProfesor(int pos) {
        Profesor eliminado = profesores.get(pos);
        if (eliminado == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        profesores.remove(pos);
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
```


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

Una composición recursiva aparece cuando una clase contiene referencias a objetos del mismo tipo. En `Persona`, el atributo `madre` vuelve a ser de tipo `Persona`, lo que permite encadenar generaciones de forma natural. Para mantener inmutabilidad, los campos se declaran `final` y no se incluyen *setters*.

Este patrón es común en estructuras jerárquicas y árboles. Igual que una excepción puede contener su causa (otra excepción), una persona puede contener su madre (otra persona), y así sucesivamente hasta un caso base (`null` o valor especial).

```java
public final class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("Nombre invalido");
        }
        this.nombre = nombre;
        this.madre = madre; /* puede ser null para cortar recursión */
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }
}
```

```java
public class MainFamilia {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Bea", abuela);
        Persona nieto = new Persona("Carlos", madre);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre: " + nieto.getMadre().getNombre());
        System.out.println("Abuela: " + nieto.getMadre().getMadre().getNombre());
    }
}
```

Otros ejemplos clásicos de composición recursiva son: directorios que contienen subdirectorios, nodos de árboles binarios (`izquierdo` y `derecho`), menús con submenús y categorías de productos con subcategorías.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Una relación bidireccional es aquella en la que ambos extremos mantienen referencia entre sí. En vez de que solo `Departamento` conozca a sus `Profesor`, cada `Profesor` también conoce su `Departamento`. Esto facilita navegar en ambos sentidos, pero obliga a mantener coherencia entre dos estructuras de datos enlazadas.

En el ejemplo, habría que añadir en `Profesor` un campo `departamento` (idealmente privado) y controlar las operaciones de alta/baja/cambio para que actualicen simultáneamente ambos lados. Si se añade un profesor al departamento, también se debe fijar su `departamento`; si se elimina, debe limpiarse esa referencia.

La recomendación práctica es centralizar esas operaciones en un único punto (por ejemplo, métodos de `Departamento`) para evitar inconsistencias. Además, conviene impedir asignaciones directas desde fuera y validar invariantes (director perteneciente al departamento) tras cada operación, de forma que nunca exista un estado intermedio inválido.
