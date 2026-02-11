<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### La encapsulación y la ocultación de información buscan separar el *uso* de un objeto de su *implementación interna*. Esto permite que el código que emplea un objeto no dependa de sus detalles internos, sino únicamente de su interfaz pública. De este modo, se consigue que los cambios dentro de la clase no afecten al resto del programa mientras se mantenga la misma interfaz.

### Entre las ventajas de la ocultación de información destacan la reducción de errores, ya que se evita que el estado interno sea manipulado indebidamente, y la facilidad de mantenimiento. También permite mejorar o modificar la implementación interna sin afectar a quienes utilicen la clase, lo cual favorece la evolución segura del software.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### La interfaz pública de un objeto o clase es el conjunto de métodos y atributos accesibles desde fuera de la misma. Representa aquello que los usuarios de la clase pueden ver y utilizar, sin necesidad de conocer cómo está implementado internamente. En Java, esta interfaz se define principalmente mediante los elementos marcados como *public*.

### La ocultación de información funciona de forma complementaria, ya que los detalles internos se marcan como *private* o se mantienen inaccesibles. De esta forma, la interfaz pública actúa como un "contrato" entre la clase y el exterior, indicando qué operaciones se pueden realizar sin exponer la estructura interna.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Es importante diseñar con cuidado la interfaz pública porque cualquier cambio posterior puede afectar a todo el código que la utilice. La interfaz pública se convierte en un punto estable sobre el cual dependen otros módulos del programa. Si se modifica de forma inadecuada, se pueden introducir incompatibilidades o errores en distintas partes del software.

### No siempre es fácil cambiar la interfaz una vez creada, especialmente si el proyecto es grande o si la clase es usada por muchos módulos. Por eso, suele intentarse que la interfaz sea simple, estable y bien pensada desde el principio, mientras que la implementación interna puede evolucionar con menos restricciones.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Las invariantes de clase son condiciones que deben cumplirse siempre que un objeto esté en un estado válido. Son reglas internas que garantizan que los datos representen información coherente, como por ejemplo que un punto mantenga siempre valores numéricos válidos o que un intervalo tenga sus límites ordenados.

### La ocultación de información ayuda a mantener estas invariantes porque evita que el estado interno sea modificado arbitrariamente desde el exterior. Al controlar el acceso mediante métodos específicos, se puede comprobar que las modificaciones respetan las condiciones necesarias para mantener el objeto en un estado consistente.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

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

    public double getX() { return x; }
    public double getY() { return y; }
}

### La interfaz pública está formada por el constructor, los métodos getter y el método *calcularDistanciaAOrigen*. Estos elementos son los que el usuario de la clase puede emplear sin conocer cómo se guardan internamente las coordenadas. Son el conjunto visible y estable de operaciones que permiten interactuar con un objeto *Punto*.

### *public* indica que un miembro es accesible desde fuera de la clase, mientras que *private* restringe su uso únicamente a métodos internos. Gracias a esto, los atributos permanecen protegidos ante modificaciones externas no controladas.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Los modificadores *public* y *private* pueden aplicarse tanto a clases (solo en algunos casos) como a métodos, atributos y constructores. Esto permite controlar qué elementos se exponen a los demás componentes del programa y cuáles quedan restringidos para uso interno.

### En Java, solo las clases de nivel superior pueden ser *public* o tener visibilidad por defecto, mientras que las clases internas sí pueden ser *private*. Los métodos y atributos pueden ser declarados con cualquiera de los cuatro niveles de visibilidad disponibles.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Además de pública y privada, muchos lenguajes orientados a objetos ofrecen visibilidades adicionales. En Java existen cuatro: pública (*public*), privada (*private*), protegida (*protected*) y la visibilidad por defecto o "package-private", que aplica cuando no se indica ningún modificador. Esta última permite que los miembros sean accesibles únicamente desde clases del mismo paquete.

### Otros lenguajes, como C++, incluyen matices diferentes, aunque suelen mantener el mismo concepto de público, protegido y privado. Cada lenguaje amplía o ajusta estos niveles para adaptarse a su propio modelo modular.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Los miembros de instancia privados están ocultos frente a otras clases, pero no frente a otras instancias de la misma clase. Las instancias de la misma clase comparten acceso interno, ya que se consideran parte de la misma unidad conceptual. Por tanto, un objeto sí puede acceder a los atributos privados de otro objeto de su misma clase.

public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;  // acceso permitido
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
``

### Este comportamiento facilita implementar métodos que operan sobre varios objetos de la misma clase sin comprometer la encapsulación frente al exterior.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Los métodos getter y setter son operaciones que permiten obtener y modificar el valor de un atributo privado. El getter devuelve el valor almacenado, mientras que el setter lo actualiza siguiendo reglas determinadas. Su uso impide exponer directamente los atributos, permitiendo mantener invariantes y aplicar validaciones.

### No todos los atributos necesitan setters, y no todos los setters deben permitir cualquier valor. Estos métodos forman parte del control de acceso que la encapsulación proporciona, evitando modificaciones indebidas.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Cuando se dice que la ocultación mejora la "seguridad", no se refiere a evitar ataques informáticos. El objetivo es proteger la integridad interna del objeto, evitando que sea usado en formas incorrectas o inconsistentes. La seguridad aquí se entiende como robustez y fiabilidad de la estructura interna de la clase.

### Por tanto, no es un mecanismo de defensa contra malware, sino una forma de asegurar que el código funcione correctamente bajo las reglas definidas para cada tipo de objeto.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Un miembro de instancia pertenece a cada objeto concreto, por lo que cada instancia dispone de su propia copia. Un miembro de clase, declarado con *static*, es compartido por todas las instancias y existe incluso aunque no se haya creado ningún objeto.

### Los miembros de clase también pueden ocultarse mediante *private*, lo cual limita su acceso a la propia clase. Esto permite mantener valores globales controlados sin exponerlos al exterior.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Tiene sentido que un constructor sea privado en situaciones donde se quiere controlar estrictamente cómo se crean los objetos. Un caso común es el uso de patrones como *factoría* o *singleton*, donde la clase ofrece métodos estáticos para construir objetos en lugar de permitir la creación directa desde fuera.

### Esto permite validar parámetros, gestionar recursos o impedir la creación de objetos adicionales según convenga al diseño.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

    public class Punto {
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}
``

### Los miembros *static* permiten información compartida entre todas las instancias. En este caso, almacenan los valores máximos de coordenadas creados hasta el momento.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}

### Se usa *static* porque el método crea instancias sin necesitar un objeto previo. Este tipo de método permite controlar el proceso de creación de manera flexible.



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

public class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}

### La interfaz pública se mantiene igual, lo que demuestra que la ocultación permite cambiar completamente la implementación interna sin afectar al exterior.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Aunque un atributo tenga getter y setter, no debe exponerse directamente como público porque se perdería control sobre él. Los atributos suelen declararse privados para mantener invariantes y evitar estados inconsistentes. Esto permite que los setters validen los valores antes de aceptarlos.

### La convención habitual es que los atributos sean privados. Esto está estrechamente relacionado con la idea de mantener invariantes confiables y proteger el estado interno.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Una clase inmutable es aquella cuyo estado no puede cambiar una vez creado el objeto. Esto implica que no existen setters ni métodos que alteren los valores internos. Un método modificador es aquel que cambia el estado del objeto, pero no siempre es un setter; puede modificar varios atributos a la vez.

### Las clases inmutables ofrecen ventajas como simplicidad conceptual, facilidad para razonar sobre el estado, y seguridad en entornos concurrentes. Java utiliza esta idea en clases como *String*.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### No se recomienda proporcionar setters por defecto. Solo deben existir cuando sean necesarios para el diseño del objeto y respeten sus invariantes. Incluir setters indiscriminadamente debilita la encapsulación y puede permitir estados inconsistentes.

### En muchos casos, la ausencia de setters favorece clases más seguras y más fáciles de mantener.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### La clase *String* en Java es inmutable. Al concatenar dos cadenas, no se modifica ninguna de las originales, sino que se crea un nuevo objeto con el contenido combinado. Esto causa múltiples creaciones de objetos si se realizan concatenaciones repetidas en bucles.

### Para operaciones con muchas concatenaciones seguidas se recomienda utilizar *StringBuilder*, que sí es mutable y permite construir cadenas eficientemente.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### La comparación entre objetos debe basarse en su contenido cuando se busca equivalencia lógica. Comparar por identidad solo indica si dos referencias apuntan al mismo objeto. En Java, el método *equals* sirve para comparar el contenido de los objetos, pero por defecto hereda la implementación de *Object*, que compara identidades.

### Las cadenas deben compararse con *equals*, nunca con ==. Usar == solo comprueba si son la misma referencia, no si tienen el mismo texto.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Los wrappers son clases que representan tipos primitivos como objetos, permitiendo tratarlos en contextos donde se requieren referencias. En Java existen *Integer*, *Double*, *Boolean*, entre otros. Java realiza la conversión entre primitivo y wrapper de forma automática mediante **autoboxing** y **unboxing**.

### No todos los lenguajes distinguen entre tipos primitivos y objetos. Algunos, como Python, representan números como objetos desde el principio, por lo que no necesitan wrappers.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Un tipo enumerado define un conjunto finito y fijo de valores posibles. En Java, un enum es realmente una clase especial, con atributos, métodos y constructores privados. Esto permite encapsular comportamiento y asociarlo a cada valor del enumerado.

### Los enumerados mejoran la encapsulación al restringir las opciones posibles y garantizar que solo existan valores válidos dentro del rango definido.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(30, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private int dias;
    private int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() { return dias; }
    public int getOrdinal() { return ordinal; }
}

### Cada constante es una instancia de la clase *Mes*. El constructor es privado, como exige Java.


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

public boolean esDePrimavera(boolean norte) {
    return norte ? (this == MARZO || this == ABRIL || this == MAYO)
                 : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
}

public boolean esDeVerano(boolean norte) {
    return norte ? (this == JUNIO || this == JULIO || this == AGOSTO)
                 : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
}

public boolean esDeOtono(boolean norte) {
    return norte ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE)
                 : (this == MARZO || this == ABRIL || this == MAYO);
}

public boolean esDeInvierno(boolean norte) {
    return norte ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
                 : (this == JUNIO || this == JULIO || this == AGOSTO);
}
