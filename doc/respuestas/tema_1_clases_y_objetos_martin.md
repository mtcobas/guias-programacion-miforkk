<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### La programación orientada a objetos se basa en cuatro pilares fundamentales. El primero es la *encapsulación*, que consiste en agrupar datos y funciones relacionadas dentro de una misma unidad llamada objeto, ocultando además los detalles internos para ofrecer solo una interfaz de uso. 

### El segundo pilar es la *abstracción*, que permite representar conceptos del mundo real mediante modelos simplificados en forma de clases, centrando la atención únicamente en los aspectos relevantes. El tercero es la *herencia*, que posibilita que una clase derive de otra y herede sus atributos y métodos, favoreciendo la reutilización del código. Por último está el *polimorfismo*, que permite que distintos objetos respondan de manera diferente a un mismo mensaje o llamada, dependiendo de su tipo concreto.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Entre los lenguajes populares que soportan la programación orientada a objetos se encuentran *Java*, ampliamente utilizado en desarrollo empresarial y Android. También está *C++*, que combina programación estructurada con orientación a objetos, permitiendo un control profundo de memoria.

### Otro lenguaje muy extendido es *Python*, que adopta un modelo de objetos flexible y sencillo. Finalmente, *C#* ofrece un enfoque moderno y es habitual en aplicaciones de Windows, videojuegos con Unity y servicios en la nube.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### La *programación estructurada* es un paradigma basado en dividir un programa en bloques de control simples: secuencias, condicionales y bucles. Su objetivo es facilitar la claridad y evitar el uso excesivo de saltos incontrolados como *goto*, promoviendo un flujo de ejecución más legible y mantenible.

### *La programación modular*, por su parte, lleva esta idea un paso más allá, organizando el código en funciones o módulos independientes. Cada módulo se encarga de una tarea específica y puede desarrollarse, probarse y reutilizarse de manera aislada. Esta aproximación ya anticipaba la idea de organizar el código en estructuras lógicas, aunque todavía sin asociar datos y operaciones como lo hace la POO.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Un objeto se define principalmente por sus *atributos*, que representan su estado interno o las características que posee. Estos atributos son variables almacenadas dentro del objeto.

### Además, un objeto cuenta con *métodos*, que representan el comportamiento o las operaciones que puede realizar. Finalmente, un objeto tiene una *identidad*, que permite distinguirlo de otros objetos, aunque tengan el mismo valor en sus atributos.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Una *clase* es una plantilla o plano que define qué atributos y métodos tendrán los objetos creados a partir de ella. No es lo mismo que un objeto: la clase describe, mientras que el objeto existe en memoria como una unidad concreta que sigue esa descripción.

### Una *instancia* es, por tanto, un objeto creado a partir de una clase específica, con sus propios valores internos. No todos los lenguajes orientados a objetos requieren el concepto de clase; lenguajes como JavaScript permiten trabajar con objetos sin necesidad de clases formales, usando prototipos.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### El lugar exacto donde se almacena un objeto depende del lenguaje. En Java, los objetos se crean siempre en el *heap*, mientras que la referencia al objeto suele almacenarse en la *pila* (stack). En C++, sin embargo, el programador puede decidir si crear un objeto en la pila o en el heap usando new.

### No todos los lenguajes gestionan la memoria de la misma forma. Java utiliza un mecanismo llamado *recolección de basura* (garbage collector), que elimina automáticamente los objetos que ya no están en uso. Esto evita liberar memoria manualmente, a diferencia de C o C++.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Un *método* es una función asociada a una clase y que actúa sobre los objetos de esa clase. Representa comportamientos o acciones que los objetos pueden realizar, utilizando o modificando sus propios atributos.

### La *sobrecarga de métodos* consiste en definir varios métodos con el mismo nombre pero con distintos parámetros. Esto permite que una misma operación pueda aplicarse con diferentes tipos o cantidades de argumentos, facilitando un uso más flexible.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### CÓDIGO
public class Punto {
    int x;
    int y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen());
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### El punto de entrada de cualquier aplicación Java es el método *main*, que debe tener una firma exacta. Este método debe ser estático, ya que se llama sin necesidad de crear un objeto previamente.

### La palabra clave *static* indica que un atributo o método pertenece a la clase, no a una instancia concreta. No se usa solo en *main*, sino también para constantes, funciones utilitarias o variables compartidas entre todos los objetos. Cuando se combina con *final*, suele emplearse para definir constantes inmutables.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Para compilar un archivo Java desde la línea de comandos se usa *javac Nombre.java*, lo que genera un archivo *.class*. Después, la ejecución se realiza con *java NombreSinExtension*, usando el nombre de la clase que contiene *main*.

### Java es un lenguaje *compilado a bytecode*, no directamente a código máquina. El bytecode es ejecutado por la *Máquina Virtual de Java (JVM)*, que funciona como una capa intermedia y permite que el mismo programa funcione en diferentes sistemas operativos.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
