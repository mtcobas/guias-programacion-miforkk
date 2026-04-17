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

### La herencia es un mecanismo que permite que una clase derive de otra para reutilizar atributos y métodos ya definidos. La frase “A es‑un B” se emplea para expresar que un subtipo puede considerarse un caso más específico de su supertipo, tal como un artillero puede considerarse un tipo concreto de soldado. Gracias a esta relación, la subclase hereda la estructura y el comportamiento definidos en la clase base, evitando duplicar código y permitiendo construir jerarquías lógicas de especialización.

### Esta relación implica *compatibilidad de tipos*, lo que significa que cualquier objeto de una subclase puede almacenarse donde se espere un objeto de la superclase. Por ejemplo, un *Artillero* es compatible con *Soldado*, lo que permite crear estructuras heterogéneas que operan sobre soldados sin importar su subtipo concreto. Esta compatibilidad facilita la extensibilidad del programa y el uso de polimorfismo.

### Además, la herencia implica *herencia de estado y comportamiento*, es decir, que los atributos y métodos de la superclase pasan a formar parte real de cada instancia de la subclase. Así, si la clase *Soldado* define un nombre y un método *saludar()*, ambos están disponibles en *Artillero* y *Zapador*. Las subclases pueden añadir funcionalidades propias como el número de minas o cohetes, completando el comportamiento sin romper la estructura heredada.

### Ejemplo:

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() { return cohetes; }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() { return minas; }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = {
            new Artillero("Pedro", 5),
            new Zapador("Luis", 3),
            new Artillero("Ana", 2)
        };

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Al crear un objeto de una subclase, siempre se ejecutan primero los constructores de la superclase y luego el de la subclase. Este orden garantiza que la parte heredada del objeto queda correctamente inicializada antes de que se inicialicen los atributos específicos del subtipo. Este comportamiento es automático, incluso aunque no se escriba explícitamente, siempre que el constructor de la superclase tenga un constructor por defecto visible.

### La palabra *super* permite invocar explícitamente al constructor de la clase base. Su uso es obligatorio cuando la superclase no tiene un constructor sin parámetros accesible, ya que Java necesita saber cómo inicializar los atributos heredados. Por ejemplo, si *Soldado* exige un nombre en su constructor, cualquier subclase estará obligada a llamar a *super(nombre)*.

### Si el constructor sin parámetros de la superclase no es visible o no existe, entonces debe llamarse explícitamente al constructor adecuado mediante *super(...)*. De lo contrario, el código no compilará porque Java no puede inicializar la parte heredada del objeto.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### En memoria, los atributos privados de la superclase forman parte real del objeto, incluso cuando la instancia pertenece a una subclase. Esto significa que un *Artillero* contiene en su estructura interna el atributo privado *nombre* definido en *Soldado*. La herencia no elimina ni copia superficialmente esos datos; simplemente amplía la estructura de la clase base.

### Sin embargo, que el atributo esté presente en memoria no implica que la subclase pueda acceder directamente a él. El modificador *private* restringe su acceso únicamente a la clase donde se declaró, por lo que ni *Artillero* ni *Zapador* pueden referirse directamente a *nombre*. Para interactuar con esos datos deben usar métodos públicos o protegidos definidos en la superclase.

### En el ejemplo dado, aunque *Artillero* “posee” el nombre heredado, no podría usarlo directamente en su código; dependiendo del diseño, se necesitaría un getter o un atributo *protected* en la clase base.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### El hecho de que los objetos de las subclases sean compatibles con el tipo de la superclase permite extender el sistema sin modificar el código existente que opera sobre la superclase. Si el código actual solo necesita llamar a un método común, como *saludar()*, entonces se pueden añadir nuevos tipos de soldados sin tocar el bucle que los recorre. Esto habilita construcciones más flexibles y reduce el acoplamiento entre componentes.

### Si se crea un nuevo subtipo como *Francotirador*, bastará con que también herede de *Soldado*. El código que gestiona un array de soldados seguirá funcionando sin cambios, porque todos comparten la interfaz común heredada. Esta propiedad ilustra una de las grandes ventajas de la orientación a objetos: el software puede crecer sin que el núcleo funcional se vea modificado.

### Ejemplo de nueva subclase:

class Francotirador extends Soldado {
    public Francotirador(String nombre) {
        super(nombre);
    }
}

### El mismo bucle de saludo seguirá válido sin añadir ninguna línea adicional.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### En Java, una referencia de un supertipo puede apuntar a objetos reales de cualquier subtipo. Esto permite almacenar en una misma colección instancias de diferentes clases derivadas. Esta conversión implícita se denomina *upcasting*, y no requiere ninguna sintaxis especial. Sin embargo, una referencia del supertipo solo puede usar los métodos visibles definidos en dicho supertipo, independientemente del objeto concreto al que apunte.

### Si se desea acceder a métodos propios del subtipo, es necesario realizar un *downcasting*, que consiste en convertir una referencia del supertipo en una del subtipo real. Esta operación solo es segura si se comprueba previamente con *instanceof*, que permite verificar si el objeto real pertenece a un subtipo concreto. De no hacerlo, puede producirse una excepción *ClassCastException*.

### Ejemplo:

for (Soldado s : ejercito) {
    s.saludar();
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getCohetes());
    }
}


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### El modificador *protected* permite que un atributo o método de la superclase sea accesible desde las subclases, incluso aunque siga siendo inaccesible para el exterior. Este nivel de protección se sitúa entre *private* y *public*, y facilita la reutilización sin vulnerar completamente la encapsulación. Su uso sirve para ofrecer a las subclases un punto controlado de acceso a la información heredada.

### En Java, basta con declarar el atributo o método con la palabra clave *protected*. Por ejemplo, si se desea que *Zapador* pueda consultar el nombre del soldado dentro de su método de poner minas, se podría modificar la clase base para que dicho atributo pase de *private* a *protected*. Esto permitiría usarlo dentro de las subclases sin exponerlo completamente al exterior

### Ejemplo:

class Soldado {
    protected String nombre;
    ...
}


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### En muchos lenguajes orientados a objetos existe una clase base universal de la cual heredan todas las demás clases. En otros lenguajes, como C++, este comportamiento no es obligatorio, por lo que se pueden crear jerarquías sin una raíz común. Cada lenguaje adopta una filosofía distinta para organizar la jerarquía de tipos.

### En Java, *todas las clases heredan implícitamente de la clase Object*, incluso aunque no se indique en el código. Esto permite que todos los objetos compartan métodos comunes como *toString()*, *equals()* o *hashCode()*. Este diseño unifica el funcionamiento de las estructuras y facilita la interoperabilidad entre componentes.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### La herencia múltiple consiste en permitir que una clase herede simultáneamente de varias clases. Este mecanismo ofrece una gran flexibilidad, pero también introduce complejidad adicional en los lenguajes que lo soportan, como problemas de ambigüedad o el clásico “problema del diamante”.

### En Java *no existe herencia múltiple de clases*, aunque sí se permite heredar de múltiples interfaces. Esto ofrece parte de la flexibilidad sin los problemas completos de la herencia múltiple, ya que las interfaces solo definen métodos y no aportan estado, evitando conflictos de atributos duplicados.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### En Java, las excepciones son objetos, lo que permite crear tipos personalizados. Una excepción no controlada se obtiene heredando de *RuntimeException*. Si se desea incluir en la excepción un objeto *Usuario* para conocer cuál generó el problema, basta con añadirlo como atributo interno. También es posible sobrecargar constructores para permitir que la excepción incluya una causa subyacente.

### Ejemplo:

class Usuario {}
    
class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super(cause);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Utilizar la herencia únicamente para reutilizar código puede generar un diseño incorrecto, ya que la herencia implica una relación conceptual fuerte (“A es‑un B”), no solo una conveniencia técnica. Si la relación conceptual no es verdadera, la jerarquía se vuelve incoherente y difícil de mantener, y puede forzar a las subclases a heredar comportamientos no deseados.

### Además, la herencia acopla fuertemente las clases, lo que significa que cambios en la clase base pueden afectar a todas las subclases, incluso cuando no están relacionadas lógicamente. Esto provoca fragilidad en el diseño y dificulta la evolución del software. Por eso, si el objetivo es simplemente compartir código, generalmente es preferible recurrir a la composición.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Se recomienda favorecer la composición porque permite construir objetos complejos combinando otros objetos, en lugar de depender de la estructura rígida de una jerarquía. La composición ofrece mayor flexibilidad, ya que los componentes pueden intercambiarse sin afectar a la estructura general, ofreciendo así una mayor modularidad y menos acoplamiento.

### La composición también respeta mejor la encapsulación, porque los detalles internos de cada componente quedan aislados dentro de su propia clase. Esto hace que los cambios en un componente no afecten directamente a los demás, reduciendo el riesgo de efectos secundarios y facilitando el mantenimiento del código.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### La herencia puede romper la encapsulación porque las subclases dependen de detalles internos de la superclase. Si la superclase cambia su estructura interna o la implementación de un método heredado, estas modificaciones pueden afectar inesperadamente a todas sus subclases. Así, las subclases quedan acopladas a decisiones internas que idealmente deberían permanecer ocultas.

### En cambio, con composición, un objeto se limita a usar otro objeto como parte de su estructura, sin depender de su implementación interna. Esto permite encapsular detalles de forma más estricta y reduce el riesgo de introducir fallos cuando se modifica la clase usada como componente.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Con herencia, se puede definir una superclase *Persona* que contenga los campos comunes *dni* y *nombre*, y hacer que *Estudiante* y *Trabajador* hereden de ella. De esta forma, ambos tipos tendrán automáticamente esos atributos y métodos relacionados sin necesidad de duplicarlos. Este enfoque refleja una relación conceptual clara si se considera que tanto estudiantes como trabajadores son tipos de personas.

### Con composición, en lugar de heredar se agrupa la información común en una clase independiente llamada *DatosPersonales*. Tanto *Estudiante* como *Trabajador* recibirán una instancia de esta clase en el constructor. Este enfoque desacopla la estructura común, permite reutilizarla sin imponer jerarquías y aporta mayor flexibilidad si en el futuro se desea usar los mismos datos en otro contexto.

### Ejemplo:

// Opción 1: herencia
class Persona {
    String dni;
    String nombre;
}
class Estudiante extends Persona {}
class Trabajador extends Persona {}

// Opción 2: composición
class DatosPersonales {
    String dni;
    String nombre;
}

class EstudianteC {
    private DatosPersonales datos;
    public EstudianteC(DatosPersonales datos) { this.datos = datos; }
}

class TrabajadorC {
    private DatosPersonales datos;
    public TrabajadorC(DatosPersonales datos) { this.datos = datos; }
}
