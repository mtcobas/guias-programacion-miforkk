<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### En C, la composición se logra anidando unas estructuras dentro de otras. Un punto se puede representar con dos coordenadas, y una línea se puede modelar como dos puntos que actúan como sus extremos. Esto permite expresar directamente la relación “una línea *tiene dos* puntos”, sin necesidad de técnicas orientadas a objetos.

### A continuación se muestra un ejemplo que incluye una función para calcular la distancia entre dos puntos, usando la raíz cuadrada, y otra función que obtiene la longitud de una línea reutilizando la función previa. De este modo, se ilustra cómo pequeñas estructuras se combinan para formar otras mayores.

#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;
    Punto b;
} Linea;

double distancia(Punto p1, Punto p2) {
    double dx = p1.x - p2.x;
    double dy = p1.y - p2.y;
    return sqrt(dx*dx + dy*dy);
}

double longitud(Linea l) {
    return distancia(l.a, l.b);
}

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### En Java, la composición se modela mediante clases que contienen otras clases como atributos. Punto se puede diseñar como una clase inmutable cuyas coordenadas se inicializan en el constructor y no se modifican posteriormente. Además, se puede añadir un método para calcular la distancia a otro punto, encapsulando el comportamiento en el objeto.

### La clase *Linea*, por su parte, contiene dos puntos finales, también inmutables. Gracias a la ocultación de información, la propia clase controla que una línea, una vez creada, no permita cambiar qué puntos la forman. Esto refuerza la idea de diseño seguro que supera a C, donde cualquier código puede modificar directamente los campos de un *struct*.


final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}

final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.distancia(b);
    }
}

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### La multiplicidad indica cuántas instancias de un tipo están asociadas a cuántas de otro dentro de la composición. Es un concepto clave en diseño orientado a objetos, porque determina restricciones estructurales que deben cumplirse en la relación entre clases. Se expresa normalmente mediante intervalos como 1, 0..1, 0..* o 1..*.

### En el ejemplo, una *Linea contiene exactamente dos* puntos: por tanto, la multiplicidad desde Linea hacia Punto es 2. En dirección contraria, un mismo *Punto* podría pertenecer a ninguna, una o varias *líneas* distintas, así que la multiplicidad desde *Punto* hacia *Linea* sería 0..*. Esta asimetría es habitual en modelos de composición.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### La composición fuerte implica que los objetos contenidos viven únicamente dentro del contenedor, de manera que su ciclo de vida depende totalmente de él. Si el contenedor deja de existir, también lo harán los objetos que contiene. Este tipo de relación se denomina habitualmente *composición* en sentido estricto.

### Por el contrario, la composición débil o *agregación* permite que las partes tengan un ciclo de vida independiente del todo. Los objetos agregados pueden existir por separado, ser compartidos o ser usados por varias instancias del contenedor. En esta modalidad, destruir el contenedor no implica necesariamente destruir los objetos incluidos.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Cuando una clase emplea otra únicamente para recibirla o devolverla como parámetro en métodos, o para crearla como instancia local, no se considera composición. En estos casos, se habla de *dependencia*. La relación es temporal y limitada al uso concreto que se hace del objeto, sin que forme parte estructural permanente del contenedor.

### Este tipo de relación es la más débil en el diseño orientado a objetos, y describe que una clase necesita a otra para llevar a cabo una operación; pero no expresa una unión conceptual de "una tiene-a otra". Es frecuente que aparezca en funciones utilitarias o en métodos que procesan datos sin guardarlos como atributos.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### En la composición fuerte, la línea crea internamente los puntos y no permite recibir puntos externos. De este modo, los puntos no existen fuera del contexto de la línea, y desaparecen junto a ella. Esto puede implementarse almacenando coordenadas en el constructor y generando internamente los objetos *Punto*.

### En la composición débil, por el contrario, la línea recibe los puntos desde fuera, de forma que estos pueden ser compartidos o reutilizados por otros objetos. En este caso, el ciclo de vida de los puntos no depende del de la línea. A continuación se muestran ambos estilos:

// Composición fuerte
final class LineaFuerte {
    private final Punto a;
    private final Punto b;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.a = new Punto(x1, y1);
        this.b = new Punto(x2, y2);
    }

    public double longitud() {
        return a.distancia(b);
    }
}

// Composición débil
final class LineaDebil {
    private final Punto a;
    private final Punto b;

    public LineaDebil(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.distancia(b);
    }
}

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### En Java, la destrucción explícita de objetos no existe, por lo que el contenedor no es responsable de liberar la memoria de las partes. La eliminación se delega totalmente al recolector de basura, que analiza qué objetos ya no son accesibles desde el programa y los libera automáticamente. Por esa razón, una clase como Linea no destruye manualmente los puntos que contiene.

### Esta conducta se alinea con la composición fuerte: cuando ya no existan referencias accesibles a la línea, tampoco habrá un camino hacia los puntos que contiene, lo que permitirá que el recolector elimine todos esos objetos. Por tanto, la dependencia de ciclo de vida se respeta sin necesidad de gestionar memoria manualmente.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### En este caso, un departamento contiene una colección de profesores, pero estos pueden existir también fuera, de modo que la relación es de agregación. Además, el departamento debe tener siempre un director, que debe pertenecer a la lista de profesores. Para cumplir la invariante, al cambiar el director o eliminar profesores es necesario comprobar que el director sigue siendo válido.

### En la implementación con arrays, se emplea un almacenamiento interno con capacidad fija, pero sin exponer el array externamente. El acceso se hace mediante métodos controlados, manteniendo la encapsulación. Se permite añadir profesores mientras no se supere el límite, y eliminar uno desplazando los elementos restantes. El director se puede cambiar siempre que se elija uno perteneciente al departamento.

final class Profesor {
    private final String nombre;
    public Profesor(String nombre) { this.nombre = nombre; }
    public String getNombre() { return nombre; }
}

final class Departamento {
    private final Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        profesores[0] = directorInicial;
        numProfesores = 1;
        director = directorInicial;
    }

    public void addProfesor(Profesor p) {
        if (numProfesores >= 50) throw new IllegalStateException("Límite alcanzado");
        profesores[numProfesores++] = p;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException();
        if (profesores[pos] == director)
            throw new IllegalStateException("No se puede eliminar al director");
        for (int i = pos; i < numProfesores - 1; i++)
            profesores[i] = profesores[i + 1];
        profesores[--numProfesores] = null;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException();
        return profesores[pos];
    }

    public int getNumProfesores() { return numProfesores; }

    public void setDirector(Profesor p) {
        boolean existe = false;
        for (int i = 0; i < numProfesores; i++)
            if (profesores[i] == p)
                existe = true;
        if (!existe)
            throw new IllegalArgumentException("El director debe estar en el departamento");
        director = p;
    }
}


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Usar *List<Profesor>* simplifica sustancialmente la implementación. La gestión de tamaño, inserciones y eliminaciones se vuelve más sencilla y no requiere desplazar manualmente elementos. También desaparecen los límites fijos, lo que hace el código más flexible y legible.

### Sin embargo, exponer directamente la lista interna mediante un método que la devuelva tal cual sería peligroso, porque permitiría modificar la estructura desde fuera del departamento, rompiendo encapsulación e invariantes. Para evitarlo, se debe devolver una copia inmutable o una vista no modificable mediante *Collections.unmodifiableList.*

import java.util.*;

final class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        profesores.add(directorInicial);
        director = directorInicial;
    }

    public void addProfesor(Profesor p) {
        profesores.add(p);
    }

    public void removeProfesor(int pos) {
        Profesor p = profesores.get(pos);
        if (p == director) throw new IllegalStateException("No se puede eliminar al director");
        profesores.remove(pos);
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public int getNumProfesores() { return profesores.size(); }

    public void setDirector(Profesor p) {
        if (!profesores.contains(p))
            throw new IllegalArgumentException("Debe ser del departamento");
        director = p;
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Una composición recursiva se produce cuando una clase contiene instancias de sí misma, generando una estructura en forma de árbol. En este caso, una clase *Persona* puede tener una referencia inmutable a su madre, que es otra instancia independiente de *Persona*. Este diseño permite construir estructuras genealógicas sin ciclos indeseados.

### La recursividad aparece de manera natural cuando se modelan jerarquías biológicas, organizativas o de carpetas. Deben evitarse referencias circulares para que la estructura siga siendo un árbol. A continuación se presenta un ejemplo con un pequeño árbol familiar desde la abuela hasta el nieto.

final class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }
}

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Abuela", null);
        Persona madre = new Persona("Madre", abuela);
        Persona hijo  = new Persona("Hijo", madre);
    }
}

### Otros ejemplos clásicos son los nodos de árboles binarios, los elementos de usn árbol de directorios o las expresiones aritméticas compuestas recursivamente por operadores y operandos.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Una composición bidireccional implica que ambos objetos conocen la relación: el contenedor mantiene referencia a las partes, y cada parte mantiene referencia al contenedor. Este diseño aumenta la complejidad porque debe evitar inconsistencias, ya que al modificar un lado debe actualizarse el otro.

### Para implementarlo en *Profesor y Departamento*, cada *Profesor* debería incluir un atributo con su *Departamento*. Al añadir o eliminar un profesor desde el departamento, sería obligatorio actualizar también la referencia en el profesor. De esta forma, la asociación queda sincronizada en ambos sentidos, lo que aumenta la expresividad pero exige mayor cuidado para mantener las invariantes.
