<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### En lenguajes sin mecanismos de genericidad avanzada, una forma clásica de permitir almacenar valores de distintos tipos consiste en usar un tipo “comodín”. En C se emplea void*, que representa un puntero a un tipo desconocido, mientras que en Java se puede utilizar *Object*, que es la superclase de todas las clases.

### En C, una estructura de datos basada en un array de *void** permite almacenar direcciones de memoria que apuntan a datos de cualquier tipo. El array no conoce el tipo real de los elementos, por lo que la responsabilidad de interpretar correctamente cada elemento recae completamente en quien lo usa.

typedef struct {
    void* datos[10];
    int size;
} Lista;

int x = 5;
double d = 3.14;
lista.datos[0] = &x;
lista.datos[1] = &d;

### En Java, el mismo efecto se consigue usando un array de *Object*. Al ser todas las clases subtipos de *Object*, se pueden almacenar instancias de cualquier clase. Sin embargo, al recuperar los elementos es necesario realizar conversiones explícitas (downcasting).

Object[] datos = new Object[10];
datos[0] = "Hola";
datos[1] = 42;


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### La programación genérica consiste en definir algoritmos y estructuras de datos de forma independiente del tipo concreto con el que van a trabajar. La idea principal es escribir código reutilizable que pueda operar sobre distintos tipos, manteniendo seguridad de tipos y evitando duplicación de código.

### esde el punto de vista conceptual, el uso de *void** en *C* o *Object* en Java se puede considerar una forma rudimentaria de genericidad. Se consigue cierta reutilización, ya que la misma estructura sirve para distintos tipos, pero se pierde información de tipos en tiempo de compilación.

### Por tanto, aunque estos ejemplos se suelen mencionar como antecedentes históricos, no constituyen programación genérica en sentido estricto. La programación genérica moderna incorpora mecanismos de chequeo de tipos en compilación, algo que no está presente en estas soluciones básicas.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### El principal problema es la pérdida de seguridad de tipos en tiempo de compilación. El compilador no puede verificar qué tipo real se está almacenando o recuperando, por lo que errores de tipo no se detectan hasta la ejecución, o incluso pasan desapercibidos.

### En C, el uso de *void** obliga a realizar conversiones explícitas de punteros. Si se realiza una conversión incorrecta, el comportamiento es indefinido, lo que puede provocar errores graves como accesos inválidos a memoria o datos corruptos.

### En Java, al usar *Object*, el problema se manifieta mediante *ClassCastException* en tiempo de ejecución. El compilador no puede garantizar que una conversión sea segura, obligando al programador a asumir ese riesgo manualmente.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Los parámetros de tipo son una forma de introducir tipos “variables” en clases, interfaces o métodos. En lugar de fijar un tipo concreto, se define un símbolo (por ejemplo T) que representará un tipo concreto cuando la clase o método se utilice.

### Este mecanismo permite que el compilador conozca y verifique los tipos concretos en cada uso, manteniendo la flexibilidad sin sacrificar seguridad. A diferencia de *Object*, no se pierde información de tipo a nivel del código fuente.

### En Java y C++, los parámetros de tipo son la base de la programación genérica moderna, ya que permiten expresar relaciones de tipo de forma clara y comprobable estáticamente.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### En Java, los generics permiten especificar el tipo de los elementos de una colección. Al instanciar una lista como *List<String>*, el compilador garantiza que solo se podrán introducir objetos de tipo *String*.

List<String> lista = new ArrayList<>();
lista.add("uno");
lista.add("dos");

for (String s : lista) {
    System.out.println(s.toUpperCase());
}

### En C++, los templates funcionan de forma similar, pero la verificación y generación de código se realiza en compilación. Un std::vector<std::string> solo admite cadenas, y no es necesario hacer conversiones al recorrerlo.

std::vector<std::string> v;
v.push_back("uno");
v.push_back("dos");

for (const std::string& s : v) {
    std::cout << s << std::endl;
}

### En ambos casos, cada elemento se maneja como String o std::string de forma segura, sin downcasting ni comprobaciones en tiempo de ejecución.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Cuando se usa una clase genérica, el compilador adapta su comportamiento según el lenguaje. En Java, el compilador verifica los tipos en compilación, pero elimina la información de los parámetros de tipo en el bytecode final.

### Este proceso se denomina *type erasure*. Tras la compilación, todos los tipos genéricos se sustituyen por su cota superior (normalmente *Object*), manteniendo únicamente las comprobaciones lógicas necesarias.

### En C++, ocurre lo contrario. Cada instanciación de un template con un tipo distinto genera código específico. Este proceso se denomina instanciación de plantillas y conserva los tipos concretos en el binario final.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Una clase genérica permite definir múltiples parámetros de tipo. En este caso, *Par<A,B>* permite almacenar dos valores cuyos tipos pueden ser distintos, manteniendo la seguridad de tipos.

public class Par<A, B> {
    private final A primero;
    private final B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() { return primero; }
    public B getSegundo() { return segundo; }
}

### Un uso típico consiste en devoluciones múltiples desde una función, como media y desviación típica de un array.

public static Par<Double, Double> estadisticas(double[] datos) {
    // cálculo omitido
    return new Par<>(media, desviacion);
}

### De este modo se evita crear clases auxiliares específicas y se mantiene total claridad sobre los tipos devueltos.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### En Java, los parámetros de tipo pueden declararse a nivel de método, lo que permite escribir funciones independientes de una clase genérica concreta. Un método <T> T *seleccionaUno*(T a, T b) garantiza que ambos parámetros tengan el mismo tipo.

public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}

### Si se usaran dos *Object*, el compilador no podría forzar que ambos fueran del mismo tipo ni evitar conversiones explícitas en el resultado. Con generics, se elimina el downcasting y se fortalece el contrato del método.

### En consecuencia, el uso de parámetros de tipo mejora la legibilidad, la seguridad y la robustez del código.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Es posible restringir los parámetros de tipo mediante *bounds*. En Java, *T extends Number* permite indicar que un tipo genérico debe ser, como mínimo, un subtipo de *Number*.

### Una solución sin generics consiste en declarar las coordenadas como *Number*, lo que permite cualquier subtipo, pero pierde información precisa del tipo concreto.

class Punto {
    private Number x, y;
}

### Una solución genérica refuerza el chequeo de tipos:

class Punto<T extends Number> {
    private T x, y;
}

### Tras el *type erasure*, el tipo final generado por el compilador en ambos casos es *Number*.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Ambas soluciones permiten reutilizar la clase *Punto* con distintos tipos numéricos. Sin embargo, solo la solución sin generics permite mezclar tipos, por ejemplo un *Integer* en x y un *Double* en y.

### En la versión genérica *Punto<T>*, ambas coordenadas deben ser del mismo tipo T. Esto refuerza la coherencia interna del objeto y permite al compilador detectar usos incorrectos.

### Además, *getX* devuelve *Number* en la versión no genérica, mientras que en la versión genérica devuelve T, lo que permite trabajar directamente con el tipo concreto sin conversiones.


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

### Se puede parametrizar la interfaz para indicar que la distancia solo se calcula entre puntos del mismo tipo concreto, usando un patrón recursivo de generics.

public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

### Las implementaciones fijan su propio tipo:

public class Punto2D implements Punto<Punto2D> {
    @Override
    public double distanciaA(Punto2D p) { ... }
}

### De este modo, el compilador garantiza que *Punto2D* solo calcula distancias a otros *Punto2D*, eliminando *instanceof* y *downcasting*.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Aunque *String* es subtipo de *Object*, *List<String>* no es subtipo de *List<Object>*. Los genéricos en Java son invariantes para preservar la seguridad de tipos.

### Sin embargo, los arrays sí son covariantes, por lo que *String[]* es subtipo de *Object[]*. Esto permite compilar código incorrecto que falla en tiempo de ejecución con *ArrayStoreException*.

### Un tipo es covariante si conserva la relación de subtipado, contravariante si la invierte, e invariante si no existe relación entre las versiones genéricas.



## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Un wildcard (?) representa un tipo desconocido, permitiendo expresar variancia de forma controlada. ? *extends* T habilita covarianza, mientras que ? *super* T habilita contravarianza.

### *List<?* extends Number> se usa cuando solo se necesita leer elementos como *Number*, por ejemplo para sumar valores.

double suma(List<? extends Number> l) {
    double s = 0;
    for (Number n : l) s += n.doubleValue();
    return s;
}

### *List<?* super *Integer>* se usa cuando se necesitan insertar enteros en la lista.

void añadeEnteros(List<? super Integer> l) {
    l.add(1);
    l.add(2);
}

### Este mecanismo permite combinar flexibilidad con seguridad de tipos en contextos específicos.


