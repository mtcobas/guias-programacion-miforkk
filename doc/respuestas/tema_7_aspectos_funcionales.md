<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Un puntero a una función es una variable que almacena la dirección de memoria de una función. En C, las funciones no son objetos, pero sí pueden ser referenciadas mediante punteros, lo que permite tratarlas como datos: almacenarlas en variables, pasarlas como argumentos o invocarlas indirectamente.

### Este mecanismo se utiliza para desacoplar comportamiento, implementar callbacks o simular ciertos patrones que en otros lenguajes se expresan con objetos o lambdas. Sin embargo, el puntero solo conoce la firma de la función, no su contexto.

### En el siguiente ejemplo se define una función que convierte una cadena a mayúsculas, se almacena su dirección en un puntero llamado *aMayusculas* y se invoca a través del puntero.

#include <stdio.h>
#include <ctype.h>

char* convertirMayusculas(char* s) {
    for (int i = 0; s[i] != '\0'; i++) {
        s[i] = toupper(s[i]);
    }
    return s;
}

int main() {
    char texto[] = "hola";
    char* (*aMayusculas)(char*) = convertirMayusculas;
    printf("%s\n", aMayusculas(texto));
}



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Una función lambda es una función anónima, normalmente breve, que puede definirse “en línea” sin necesidad de declararla como una función tradicional con nombre. Su objetivo principal es expresar comportamiento de forma concisa y flexible.

### En JavaScript, las funciones son ciudadanos de primera clase desde su diseño inicial, por lo que las lambdas (arrow functions) encajan de forma natural. Se pueden asignar a variables, pasarse como argumentos y devolver como resultado.

### En Java, desde Java 8, se introducen las funciones lambda como una forma compacta de implementar interfaces funcionales. A continuación se muestran ejemplos equivalentes en ambos lenguajes.

let aMayusculas = s => s.toUpperCase();
console.log(aMayusculas("hola"));

Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(aMayusculas.apply("hola"));



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### El paradigma funcional es un estilo de programación que se basa en el uso de funciones como elemento central, evitando efectos colaterales y favoreciendo la inmutabilidad. El programa se construye aplicando y combinando funciones, más que modificando estados.

### Lenguajes como Java se consideran hoy multi‑paradigma porque, aunque su base sigue siendo la orientación a objetos, incorporan características funcionales como lambdas, interfaces funcionales y streams. Esto permite elegir el enfoque más adecuado según el problema.

### Decir que las funciones son “ciudadanos de primera clase” significa que pueden tratarse como cualquier otro valor: asignarse a variables, pasarse como argumentos y devolverse desde funciones, algo clave en programación funcional.


## 4. Explica la sintaxis básica de una función lambda en Java.

### La sintaxis de una función lambda en Java se compone de tres partes: una lista de parámetros, el operador -> y el cuerpo de la función. Los parámetros pueden ir entre paréntesis y su tipo puede omitirse si el compilador puede inferirlo.

### El cuerpo puede ser una única expresión o un bloque de código. Si es una única expresión, su valor se devuelve implícitamente, sin necesidad de *return*.

### Las lambdas siempre implementan una interfaz funcional, aunque esa interfaz no se mencione explícitamente en el código. El tipo de la lambda viene determinado por el contexto en el que se utiliza.

s -> s.toUpperCase()
(String s) -> { return s.toUpperCase(); }



## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Una característica esencial del enfoque funcional es poder recibir funciones como parámetros y ejecutarlas desde otro método. Esto permite separar el “qué se hace” del “cómo se aplica”.

### En Java, esto se consigue pasando una interfaz funcional como argumento. En JavaScript, simplemente se pasa la función. En ambos casos, el método *transformar* aplica una transformación a una cadena sin conocer su implementación concreta.

function transformar(texto, f) {
    return f(texto);
}

let aMayusculas = s => s.toUpperCase();
console.log(transformar("hola", aMayusculas));

static String transformar(String texto, Function<String,String> f) {
    return f.apply(texto);
}


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Una de las ventajas de las lambdas es que permiten definir el comportamiento justo en el lugar donde se necesita, sin crear variables o métodos adicionales.

### Esto hace el código más expresivo y evita introducir nombres artificiales para funciones que solo se usan una vez. El resultado es un estilo más declarativo.

### En este caso, la función que invierte una cadena se define directamente en la llamada a *transformar*.

transformar("hola", s -> new StringBuilder(s).reverse().toString());


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Una clausura ocurre cuando una función lambda captura variables del contexto en el que ha sido definida. Es decir, la lambda puede acceder a variables locales externas, incluso cuando se ejecuta en otro momento.

### En Java, estas variables deben ser *efectivamente finales*, lo que significa que no se modifican después de su inicialización. Esto evita inconsistencias y problemas de concurrencia.

### En el siguiente ejemplo, la lambda utiliza una variable *sufijo* definida fuera, formando una clausura.

String sufijo = "!!!";
Function<String,String> f = s -> s + sufijo;
System.out.println(f.apply("hola"));

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Un puntero a función en C solo apunta a código; no captura ningún estado ni contexto. Toda la información necesaria debe pasarse explícitamente mediante parámetros.

### Una función lambda, en cambio, puede capturar variables del entorno y mantener ese estado asociado a la función. Esto permite crear funciones más expresivas y autónomas.

### Además, las lambdas en Java están integradas en el sistema de tipos mediante interfaces funcionales, mientras que en C los punteros a función no ofrecen ningún mecanismo de seguridad adicional más allá de la firma.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Devolver funciones es otra característica clave del estilo funcional. En este caso, se define una función que crea descuentos, devolviendo otra función que aplica ese descuento.

### La lambda devuelta “recuerda” el porcentaje con el que fue creada, lo que constituye una clausura. Cada descuento mantiene su propio estado independiente.

static Function<Double,Double> crearDescuento(double porcentaje) {
    return precio -> precio * (1 - porcentaje);
}

Function<Double,Double> d10 = crearDescuento(0.10);
Function<Double,Double> d20 = crearDescuento(0.20);


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Una interfaz funcional es una interfaz que declara *exactamente un método abstracto*. Es el tipo sobre el que se apoyan las funciones lambda en Java.

### Puede contener métodos *default* o *static*, pero solo un método abstracto. Esto permite que el compilador asocie una lambda a esa interfaz de forma inequívoca.

### Ejemplos comunes son *Runnable*, *Comparator* o *Function<T,R>*, todas pensadas para representar comportamiento.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Crear una interfaz funcional propia permite expresar de forma clara el propósito semántico de una lambda, más allá de usar interfaces genéricas.

### En este caso, *Transformador* describe explícitamente una operación que transforma una cadena en otra, mejorando la legibilidad del código.

@FunctionalInterface
interface Transformador {
    String transformar(String s);
}


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Generalizar la interfaz permite reutilizarla en muchos contextos distintos. Mediante parámetros de tipo, se consigue un transformador de cualquier tipo T a cualquier tipo R.

### Esto refuerza el chequeo de tipos y evita conversiones innecesarias. Un ejemplo claro es transformar un *Double* en un *Integer*.

interface Transformador<T,R> {
    R transformar(T valor);
}

Transformador<Double,Integer> redondear =
    d -> d.intValue();


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Java proporciona un amplio conjunto de interfaces funcionales en *java.util.function* para evitar redefinir patrones comunes.

### Entre las más usadas están *Function<T,R>*, *Predicate<T>*, *Consumer<T>*, *Supplier<T>* y las variantes para tipos primitivos como *IntFunction* o *DoubleConsumer*.

### Estas interfaces cubren la mayoría de necesidades habituales y favorecen la interoperabilidad con streams y APIs funcionales.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### El método *forEach* permite recorrer una colección aplicando una acción a cada elemento, sin necesidad de controlar índices o iteradores.

### Este estilo enfatiza qué se quiere hacer con cada elemento, no cómo se recorre la colección, lo que mejora la claridad.

List<Integer> lista = List.of(-2, 3, 5);
lista.forEach(i -> {
    if (i > 0) {
        System

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### PECS significa Producer Extends, Consumer Super. Es una regla para decidir cuándo usar extends o super en genéricos con wildcards.

### *forEach* recibe un *Consumer<? super T>* porque el consumidor *consume* valores de tipo T. Permitir supertipos aumenta la flexibilidad sin comprometer la seguridad.

### Aplicado a *transformar*, usar *Function<? super T, ? extends R>* permitiría aceptar funciones más generales como entrada y más específicas como salida, reforzando la reutilización.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Una referencia a método permite tratar un método existente como si fuera una función, sin necesidad de definir una lambda explícita.

### En JavaScript, los métodos son funciones asociadas a objetos. En Java, las referencias a método ofrecen una sintaxis más compacta y legible.

class Persona {
    constructor(nombre) { this.nombre = nombre; }
    saludar() { console.log("Hola, soy " + this.nombre); }
}

let p = new Persona("Ana");
let saludo = p.saludar.bind(p);
saludo();

Persona p = new Persona("Ana");
Runnable saludo = p::saludar;
saludo.run();


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Java permite varios tipos de referencias a método. Cada una sustituye a una lambda compatible en un contexto funcional.

### Se puede referenciar un método estático *(Clase::metodoEstatico)*, un constructor *(Clase::new)*, un método de una instancia concreta *(obj::metodo)* o un método de instancia genérico *(Clase::metodo)*.

Integer::parseInt
Persona::new
p::saludar
String::length


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### El enfoque funcional permite definir criterios de ordenación de forma concisa y expresiva, evitando clases anónimas largas.

### La versión manual usa una lambda que compara edad y luego nombre. La versión con *Comparator* aprovecha métodos auxiliares para componer comparadores.

Collections.sort(lista, (p1, p2) -> {
    int c = Integer.compare(p1.getEdad(), p2.getEdad());
    return c != 0 ? c : p1.getNombre().compareTo(p2.getNombre());
});


Collections.sort(lista,
    Comparator.comparing(Persona::getEdad)
              .thenComparing(Persona::getNombre));
