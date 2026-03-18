<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### En C, ante la ausencia de excepciones, el control de errores se basa en técnicas manuales que ayudan a comunicar al llamador que algo ha fallado. Una forma común consiste en devolver un valor especial que indique el error. En cálculos con números reales, puede elegirse un valor imposible, como *-1.0* para una raíz cuadrada, aunque esto obliga al llamador a distinguir cuidadosamente entre valores válidos y señal de error.

### Otra estrategia típica es utilizar parámetros de salida adicionales, como introducir un puntero a un entero que actúe como bandera de error. De esta manera, la función devuelve el resultado, pero escribe en la variable señalada si se ha producido un problema. Esta aproximación separa el valor del resultado y la indicación del fallo, evitando confusiones respecto a valores especiales.

// Opción 1: devolver código de error
double raiz(double x) {
    if (x < 0) return -1.0;
    return sqrt(x);
}

// Opción 2: parámetro de estado
double raiz2(double x, int *error) {
    if (x < 0) {
        *error = 1;
        return 0.0;
    }
    *error = 0;
    return sqrt(x);
}


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Una excepción es un mecanismo que permite interrumpir el flujo normal de un programa cuando ocurre una situación anómala. Su objetivo es separar la lógica funcional del tratamiento de errores, evitando que cada función deba comprobar continuamente condiciones especiales que dificultan la claridad del código.

### Para quien implementa funciones, las excepciones ofrecen una forma explícita de indicar problemas sin mezclar valores especiales con resultados válidos. Para quien llama a dichas funciones, facilitan decidir si el error se gestiona en ese punto o se deja que se propague hacia niveles superiores, permitiendo un diseño más flexible y modular.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### En Java, puede implementarse un método que lance una excepción al recibir un número negativo, lo que obliga al código llamador a encargarse del error. Esta separación mejora la claridad del flujo del programa y evita confusiones con valores de retorno ambiguos. Además, permite expresar de forma explícita que el error no es parte del resultado, sino una condición extraordinaria.

### La clase *Calculadora* puede encapsular la operación, mientras que el método *main* muestra cómo capturar la excepción. Esto ilustra la diferencia esencial con C: no se emplean valores especiales, sino un mecanismo dedicado para gestionar situaciones incorrectas de ejecución.

 class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede con negativo");
        }
        return Math.sqrt(x);
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            System.out.println(Calculadora.raiz(-4));
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Lanzar una excepción significa interrumpir la ejecución normal mediante la instrucción *throw*. En ese momento, la función no continúa y se busca un bloque *catch* adecuado en los niveles superiores. Capturar o controlar la excepción consiste en interceptar este evento anómalo y ejecutar un código específico para gestionarlo, como mostrar un mensaje o corregir la situación.

### Si una excepción no se captura en una función, esta se propaga hacia arriba por la pila de llamadas. Cada función intermedia se abandona sin reanudarse y sin ejecutar el código restante. En el ejemplo anterior, si *raiz* lanza la excepción, no vuelve a su punto de llamada, sino que el método main recibe directamente la excepción, y solo allí se decide cómo reaccionar ante el error.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### La propagación natural tiene la ventaja de permitir que el error sea gestionado en el nivel adecuado sin ensuciar cada función con comprobaciones repetitivas. Esto evita llenar el código con constantes condiciones *if* para verificar si ocurrió un fallo, lo que mejora su legibilidad y reduce la posibilidad de errores no detectados.

### Además, la excepción puede transportar información sobre el problema hasta el punto más conveniente del programa, incluso si está varios niveles por encima en la pila de llamadas. Con ello se facilita un diseño más modular, donde cada función se centra en su comportamiento previsto y no en la gestión de errores ajenos a su propósito.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### En Java, las excepciones son objetos, lo que permite encapsular en ellas toda la información relevante sobre el error. Esto incluye el mensaje descriptivo, datos adicionales y la traza de la ejecución. Esta capacidad proporciona un modo estructurado de transportar información específica del problema sin recurrir a códigos numéricos poco expresivos.

### El hecho de que las excepciones sean clases permite crear tipos personalizados que representen errores particulares del dominio de la aplicación. Con ello, un programa puede definir sus propios mensajes, atributos o comportamientos, adaptando el sistema de excepciones a sus necesidades concretas y reforzando la claridad del diseño.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Una diferencia clave respecto a C es que las excepciones en Java contienen información detallada sobre lo ocurrido. Todas incluyen un mensaje explicativo que puede consultarse desde el manejador, lo que facilita entender rápidamente el contexto del error. Además, llevan incorporada la traza de pila, que muestra el camino exacto recorrido por el programa hasta producirse el fallo.

### Esta información encapsulada resulta especialmente útil en depuración, porque permite reconstruir el origen del error sin depender de valores de retorno ambiguos. En cambio, en C, la responsabilidad de transportar esa información recae completamente en el programador, lo que aumenta las posibilidades de cometer errores o perder detalles importantes.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### En Java es posible colocar varios bloques *catch* debajo del mismo *try*, cada uno especializado en un tipo diferente de excepción. De este modo, el programa puede reaccionar de forma distinta dependiendo de la naturaleza del error detectado. Esta capacidad es útil cuando una operación puede fallar por causas diferentes.

### Sin embargo, aunque se declaren muchos bloques *catch*, solo se ejecuta uno: el primero cuyo tipo coincida con la excepción lanzada. Esta selección evita que el error sea tratado más de una vez y establece un modelo de manejo jerárquico en función de los tipos de excepción definidos.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### El bloque *finally* permite garantizar que un fragmento de código se ejecutará siempre, incluso si se produce una excepción o si la función retorna antes de tiempo. Esto resulta fundamental para realizar tareas como cerrar ficheros o liberar recursos externos al lenguaje, que deben completarse sin depender del éxito de la operación principal.

### Es posible combinar *try, catch y finally*, o utilizar únicamente *try y finally*. En ambos casos, Java asegura la ejecución del bloque final antes de continuar o propagar la excepción, lo que contribuye a que la gestión de recursos sea robusta y predecible.

// Con catch
try {
    abrirFichero();
} catch (IOException e) {
    System.out.println("Error");
} finally {
    cerrarFichero();
}

// Sin catch
try {
    abrirFichero();
} finally {
    cerrarFichero();
}
``


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Java permite utilizar un bloque *finally* sin necesidad de tener un *catch*. En este caso, el objetivo es únicamente garantizar la liberación de recursos sin intentar manejar la excepción directamente. Este patrón es útil cuando una función quiere delegar la gestión del fallo en un nivel superior, pero debe cumplir un trabajo de limpieza imprescindible.

### El bloque *finally* se ejecuta siempre, tanto si hay una excepción como si no. Incluso si hay un *return* dentro del *try*, primero se ejecutará el *finally* y después se devolverá el valor. La única excepción son situaciones extremas como un apagado abrupto del programa, pero en ejecución normal su cumplimiento está garantizado.



## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Las excepciones controladas son aquellas que el compilador obliga a declarar o capturar. Suelen representar errores previsibles relacionados con recursos externos, como lectura de ficheros o comunicación por red. En cambio, las no controladas derivan de *RuntimeException* y representan fallos de programación, como argumentos inválidos o divisiones entre cero.

### La clase *RuntimeException* agrupa los errores que indican fallos lógicos que deberían ser adelantados mediante comprobaciones previas. Las excepciones controladas típicas incluyen *IOException*, *SQLException* o *ClassNotFoundException*. Entre las no controladas se encuentran *NullPointerException*, *IllegalArgumentException* o *IndexOutOfBoundsException*.

### *Situaciones para excepción controlada*:

Error al abrir un fichero.
Problema al comunicarse con una base de datos.
Operación de red fallida.
Conversión insegura que depende del exterior.

*Situaciones para excepción no controlada*:

Argumento ilegal en un constructor.
Índice fuera de rango.
Cálculo numérico mal definido.
Intento de acceder a un objeto nulo.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### *throws* es una parte de la firma de un método que declara que este puede lanzar una excepción controlada sin gestionarla internamente. Esto indica al llamador que debe encargarse del problema o continuar propagándolo hacia arriba. Resulta útil cuando una función no tiene suficiente contexto para decidir cómo manejar el error.

### Esta declaración es una alternativa a capturar la excepción, ya que permite delegar la responsabilidad a un nivel superior del programa. De este modo, el desarrollador puede diseñar funciones más limpias, centradas en su cometido, y dejar la gestión del error en el punto más apropiado del flujo de ejecución.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Una función que abra un fichero y no quiera encargarse del error puede declararlo con throws *IOException*. Esto obliga al llamador a decidir cómo tratar la situación. No obstante, incluso en ese caso, la función puede necesitar un bloque *finally* para garantizar el cierre del recurso si se llegó a abrir correctamente antes del fallo.

import java.io.*;

class Lector {
    public void leer(String nombre) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(nombre));
            System.out.println(br.readLine());
        } finally {
            if (br != null) br.close();
        }
    }
}


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Es posible escribir en throws una excepción no controlada, como *RuntimeException*, aunque no es habitual porque el compilador no obliga a declararla. El método llamador tampoco está obligado a colocar un *try-catch*, ya que estas excepciones representan errores que deberían evitarse mediante validaciones previas y no mediante tratamiento explícito.

### No obstante, incluirlas en *throws* puede tener sentido para documentar que la función podría lanzar ese tipo de error. Aun así, no introduce ninguna responsabilidad adicional para el llamador y se usa principalmente como herramienta de claridad o diseño.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Las excepciones controladas se recomiendan cuando el error es esperable y puede resolverse de forma razonable por parte del programador. Suelen asociarse a recursos externos a la aplicación, donde las condiciones pueden fallar sin indicar un error lógico del código. En estos casos, obligar a su gestión ayuda a evitar pérdidas de información relevantes.

### Las excepciones no controladas se emplean para indicar fallos de programación, como argumentos incorrectos o estados inválidos. Se prefieren cuando el problema debe corregirse revisando el código en vez de establecer estrategias de recuperación. No todos los lenguajes distinguen ambas categorías; en los que solo existe una, suele usarse un modelo similar al de las no controladas.



## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Dentro de un bloque *catch* es posible lanzar una nueva excepción para indicar que el error detectado no puede manejarse completamente. Esto permite convertir un fallo de bajo nivel en uno de más alto nivel, adaptado al contexto del programa. Este patrón es útil para no exponer detalles innecesarios a niveles superiores.

### También puede relanzarse la misma excepción capturada, lo que resulta apropiado cuando se ha realizado alguna operación intermedia, como registrar el error, y después se decide que el problema debe seguir propagándose. Este enfoque mantiene la información original y no altera el comportamiento natural de la excepción.

// Lanzar nueva
catch(IOException e) {
    throw new MiExcepcionAlta("Fallo al leer", e);
}

// Relanzar la misma
catch(IllegalArgumentException e) {
    logError(e);
    throw e;
}



## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Una excepción puede contener otra como causa para preservar la información original del fallo. Esta técnica se usa cuando se captura un error de bajo nivel, pero se prefiere comunicar uno más significativo para el contexto actual. La excepción de alto nivel almacena internamente la excepción original, manteniendo la cadena de errores para futuras inspecciones.

### En Java, al imprimir la excepción mediante *printStackTrace()*, se muestra tanto la excepción principal como la causa encadenada. Esto facilita rastrear el origen del problema manteniendo la estructura completa del fallo. A continuación se muestra un ejemplo representativo:

class LecturaException extends Exception {
    public LecturaException(String msg, Throwable causa) {
        super(msg, causa);
    }
}

try {
    leerFichero();
} catch(IOException e) {
    throw new LecturaException("Error al procesar el fichero", e);
}

