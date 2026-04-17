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

### El *polimorfismo* es un principio de la programación orientada a objetos que permite tratar objetos de distintas clases de forma uniforme, siempre que compartan una misma superclase o interfaz. Esto significa que una referencia de tipo base puede apuntar a objetos de diferentes subclases, y que una misma llamada a un método puede producir comportamientos distintos según el objeto concreto al que se aplique. Su objetivo principal es aumentar la flexibilidad, reutilización y extensibilidad del software.

### Gracias al polimorfismo, el código puede escribirse en términos más generales, reduciendo dependencias con clases concretas. De esta forma, es posible añadir nuevos tipos de objetos sin modificar el código que ya los utiliza, siempre que respeten la interfaz esperada. Este enfoque es especialmente útil cuando se trabaja con jerarquías de herencia.

### La *sobreescritura* de métodos consiste en redefinir en una subclase un método que ya estaba definido en su clase base, manteniendo la misma firma (nombre y parámetros). Al sobrescribir un método, la subclase proporciona una implementación específica que sustituye al comportamiento heredado cuando el método se invoca sobre un objeto de esa subclase.

### La sobreescritura es el mecanismo clave que hace posible el polimorfismo dinámico: permite que, al llamar a un método mediante una referencia de tipo base, se ejecute la versión adecuada según el tipo real del objeto en tiempo de ejecución.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### La *ligadura dinámica* *(o enlace tardío)* es el mecanismo mediante el cual el lenguaje decide qué implementación de un método debe ejecutarse en *tiempo de ejecución*, en función del tipo real del objeto y no del tipo de la referencia. Esto contrasta con la ligadura estática, donde la decisión se toma en tiempo de compilación.

### Este concepto está íntimamente relacionado con el polimorfismo, ya que es lo que permite que una llamada a un método sobrescrito ejecute la versión adecuada según la subclase concreta del objeto. Sin ligadura dinámica, la sobreescritura no tendría efecto práctico al usar referencias del tipo base.

### En *C++*, la ligadura dinámica no es el comportamiento por defecto. Para que un método participe en el polimorfismo debe declararse como *virtual* en la clase base. Si esto no se hace, las llamadas se resuelven de forma estática. En *Java*, sin embargo, todos los métodos de instancia (no *static*, no *final*, no *private*) usan ligadura dinámica automáticamente, sin necesidad de indicarlo explícitamente.

### En *Python*, todo el despacho de métodos es dinámico por naturaleza. No existe declaración de tipos estáticos en el mismo sentido que en Java o C++, y cualquier método puede ser redefinido en una subclase sin palabras clave especiales. Por tanto, el polimorfismo y la ligadura dinámica están siempre presentes de forma implícita.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### En este ejemplo se define una clase base *Soldado* con un método *saludar*. A partir de ella se crean dos subclases: *Zapador* y *Artillero*. La clase *Zapador* sobrescribe completamente el método *saludar*, mientras que *Artillero* hereda el comportamiento original.

### El objetivo es mostrar cómo, al usar referencias de tipo *Soldado*, el método que se ejecuta depende del tipo real del objeto. Esto ilustra claramente el polimorfismo: una misma llamada produce resultados distintos.

class Soldado {
    public void saludar() {
        System.out.println("Soldado saludando.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador listo para construir o destruir.");
    }
}

class Artillero extends Soldado {
    // No sobreescribe saludar
}



public class EjemploPolimorfismo {
    public static void main(String[] args) {
        Soldado[] ejercito = {
            new Soldado(),
            new Zapador(),
            new Artillero()
        };

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}


### Al recorrer el array y llamar a *saludar*, Java selecciona en tiempo de ejecución la versión correcta del método, demostrando el uso de ligadura dinámica y polimorfismo.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Al sobreescribir un método en Java, es posible invocar explícitamente la implementación de la clase base y trabajar a partir de su resultado o comportamiento. Esto resulta útil cuando se desea ampliar o modificar ligeramente el comportamiento heredado en lugar de reemplazarlo por completo.

### Para ello se utiliza la palabra clave *super*, que permite acceder a los miembros (métodos o atributos) de la superclase. En el contexto de un método sobrescrito, *super.metodo()* ejecuta la versión definida en la clase base.

### En el ejemplo siguiente, *Zapador* reutiliza el saludo del *Soldado* base y añade un mensaje adicional propio:

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}

### La palabra clave utilizada para invocar el método de la clase base es *super*. Su uso permite construir comportamientos más especializados sin duplicar código ni romper la lógica común definida en la jerarquía.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Al sobreescribir un método en Java, deben cumplirse ciertas restricciones. *La firma del método* debe coincidir: mismo nombre y mismos tipos de parámetros. El tipo de retorno debe ser el mismo o un subtipo (covarianza). Además, no se puede reducir la visibilidad del método (por ejemplo, no se puede pasar de *public a protected*).

### La *sobreescritura (overriding)* ocurre entre una clase base y una subclase, y su elección se resuelve en tiempo de ejecución mediante ligadura dinámica. En cambio, la *sobrecarga (overloading)* consiste en definir varios métodos con el mismo nombre pero distinta lista de parámetros en una misma clase, y se resuelve en tiempo de compilación.

### La anotación *@Override* indica explícitamente que un método está destinado a sobrescribir otro de la superclase. No es obligatoria, pero su uso es altamente recomendable porque permite al compilador detectar errores, como firmas mal definidas o cambios accidentales en la clase base.

### Usar *@Override* mejora la legibilidad del código y reduce errores sutiles, especialmente en jerarquías grandes o cuando se mantienen proyectos a largo plazo.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### En la práctica, el polimorfismo se utiliza en Java desde las primeras etapas de aprendizaje, incluso aunque no siempre se sea consciente de ello. Al sobrescribir métodos como *toString* o *equals*, se está aplicando directamente el concepto de polimorfismo.

### Por ejemplo, cuando un objeto se imprime con *System.out.println(objeto)*, Java llama internamente al método *toString*. Si dicho método ha sido sobrescrito en la clase del objeto, se ejecuta la versión específica, aunque la referencia sea de tipo *Object*.

### Esto implica que una referencia de tipo *Object* puede comportarse de manera diferente según el objeto concreto al que apunte, lo cual es una manifestación clara del polimorfismo. Lo mismo ocurre con equals, que permite definir criterios de igualdad específicos para cada clase.

### Por tanto, sí: desde muy temprano en Java se emplea polimorfismo, incluso antes de estudiar formalmente el concepto, lo que demuestra que está profundamente integrado en el diseño del lenguaje.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Una *clase abstracta* es una clase que no puede ser instanciada directamente y que está pensada para ser heredada. Sirve como plantilla común para un conjunto de subclases relacionadas, definiendo comportamiento común y dejando partes sin implementar.

### Un *método abstracto* es un método que no tiene implementación en la clase abstracta y que debe ser obligatoriamente implementado por las subclases concretas. Su existencia fuerza a que cada tipo concreto proporcione su propio comportamiento.

### No se pueden crear instancias de una clase abstracta directamente. La palabra clave *abstract* debe colocarse tanto en la definición de la clase como en la del método abstracto:

abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado saludando.");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El artillero dispara el cañón.");
    }
}

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### La palabra clave *final* en Java tiene distintos efectos según dónde se aplique. Cuando se usa en un *método*, indica que este no puede ser sobrescrito por las subclases. Cuando se aplica a una clase, impide que dicha clase sea heredada.

### Desde el punto de vista del polimorfismo, *final* introduce una limitación clara. Un método *final* no puede participar en el polimorfismo dinámico mediante sobreescritura, ya que su comportamiento queda fijado en la clase donde se define.

### Esto puede usarse por razones de diseño, seguridad o eficiencia, cuando se desea garantizar un comportamiento inmutable. Un ejemplo clásico de clase *final* en la API de Java es *String*, que no puede ser heredada.

### La decisión de marcar clases o métodos como *final* debe tomarse conscientemente, ya que reduce la extensibilidad pero aumenta el control sobre la jerarquía de clases.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Una *interfaz* en Java define un conjunto de métodos (y constantes) que una clase se compromete a implementar. Representa un contrato que especifica qué debe hacerse, pero no cómo. Desde Java 8, las interfaces pueden incluir métodos *default*, pero su propósito principal sigue siendo definir comportamientos comunes.

### Las interfaces se parecen a las clases abstractas, pero no son iguales. Una clase abstracta puede contener estado (atributos) y constructores, mientras que una interfaz no. Además, una clase solo puede heredar de una clase abstracta, pero puede *implementar múltiples interfaces*.

### Esta capacidad permite una forma de herencia múltiple de comportamiento abstracto, evitando los problemas clásicos de la *herencia múltiple* de clases. Las interfaces son fundamentales para el polimorfismo, ya que permiten programar contra abstracciones.

### En resumen, una clase en Java puede implementar varias interfaces, lo que incrementa enormemente la flexibilidad del diseño orientado a objetos.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Se plantea una clase abstracta Punto con un método abstracto *calcularDistanciaA*, que obliga a las subclases a definir cómo se calcula la distancia. Las clases *Punto2D* y *Punto3D* implementan este método según sus dimensiones.

### Para garantizar que la distancia se calcule solo entre puntos del mismo tipo, se usa *instanceof* junto con *downcasting*. Esto permite verificar compatibilidad en tiempo de ejecución.

abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}
``
 
class Punto2D extends Punto {
    double x, y;

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro;
            return Math.hypot(x - p.x, y - p.y);
        }
        throw new IllegalArgumentException("Punto incompatible");
    }
}


class Punto3D extends Punto {
    double x, y, z;

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro;
            return Math.sqrt(
                Math.pow(x - p.x, 2) +
                Math.pow(y - p.y, 2) +
                Math.pow(z - p.z, 2)
            );
        }
        throw new IllegalArgumentException("Punto incompatible");
    }
}


class Linea {
    private Punto a, b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}

### La clase *Linea* no necesita conocer si trabaja con puntos 2D o 3D; confía en el polimorfismo para obtener la longitud correcta.

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### *La herencia de interfaces* en Java consiste en que una interfaz puede extender otra interfaz, heredando sus métodos. A diferencia de las clases, Java permite *herencia múltiple de interfaces*, es decir, una interfaz puede extender varias interfaces a la vez.

### Esto permite construir jerarquías de contratos cada vez más ricos, sin problemas de ambigüedad en la implementación, ya que las interfaces no contienen estado. Las clases que implementan estas interfaces deben proporcionar las implementaciones concretas.

### Ejemplo de interfaces *Fichero* y *FicheroEscribible*:

interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

### Cualquier clase que implemente *FicheroEscribible* estará obligada a implementar todos los métodos definidos en ambas interfaces, lo que refuerza el uso del polimorfismo basado en contratos y no en implementaciones concretas.
