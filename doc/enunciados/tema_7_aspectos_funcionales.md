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

### Respuesta
Un **puntero a función** en C es una variable que almacena la dirección de memoria de una función. Esto permite tratar funciones como datos, pudiendo pasarlas como parámetros, almacenarlas en estructuras o invocarlas dinámicamente. Su sintaxis puede resultar poco intuitiva al principio, ya que combina la declaración de punteros con la firma de la función (tipo de retorno y parámetros).

El uso de punteros a funciones resulta especialmente útil cuando se desea desacoplar el código, por ejemplo, al implementar callbacks o estrategias de ejecución. En lugar de llamar directamente a una función concreta, se puede usar un puntero para decidir en tiempo de ejecución qué función ejecutar, lo que aporta flexibilidad similar a algunos mecanismos vistos en Java como el polimorfismo (aunque sin orientación a objetos).

A continuación se muestra un ejemplo en C donde se define una función que recibe una cadena y la convierte a mayúsculas. Posteriormente, se crea un puntero a dicha función llamado `aMayusculas` y se invoca a través de él:

```c
#include <stdio.h>
#include <ctype.h>

// Función que convierte una cadena a mayúsculas
void convertirAMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    char texto[] = "Hola mundo";

    // Declaración del puntero a función
    void (*aMayusculas)(char *);

    // Asignación de la función al puntero
    aMayusculas = convertirAMayusculas;

    // Invocación de la función a través del puntero
    aMayusculas(texto);

    printf("%s\n", texto);  // Resultado: HOLA MUNDO

    return 0;
}
```

En este ejemplo, el puntero `aMayusculas` tiene la misma firma que la función `convertirAMayusculas`. Se observa que no es necesario usar el operador `&` al asignar la función, ya que el nombre de la función ya representa su dirección. Finalmente, la llamada mediante el puntero permite ejecutar la función de forma indirecta, demostrando cómo las funciones pueden manipularse como valores en C.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una **función lambda** es una función anónima (es decir, sin nombre) que se puede definir directamente en el lugar donde se necesita. Se caracteriza por ser concisa y, normalmente, utilizarse como valor que se asigna a una variable o se pasa como argumento. Conceptualmente, son una evolución de la idea de punteros a función en C, pero con una sintaxis mucho más simple y común en lenguajes modernos, facilitando un estilo más funcional de programación.

El uso de funciones lambda permite escribir código más compacto y expresivo, especialmente cuando se requiere una función pequeña que no merece ser definida de forma independiente. A diferencia de C, donde se trabajan direcciones de memoria explícitas, en lenguajes como JavaScript o Java las funciones se tratan como objetos de alto nivel, lo que simplifica su manipulación. En Java, además, las lambdas se apoyan en interfaces funcionales (como `Function<T, R>`), lo que encaja bien con el modelo de tipos del lenguaje.

Ejemplo en **JavaScript**:

```javascript
// Definición de la función lambda
let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

// Invocación
let resultado = aMayusculas("Hola mundo");
console.log(resultado);  // HOLA MUNDO
```

Ejemplo en **Java** usando `Function<String, String>`:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Definición de la función lambda
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        // Invocación
        String resultado = aMayusculas.apply("Hola mundo");
        System.out.println(resultado);  // HOLA MUNDO
    }
}
```

En ambos casos, la variable `aMayusculas` almacena una referencia a una función anónima. Se observa que, en JavaScript, la invocación es directa como si fuera una función tradicional, mientras que en Java se emplea el método `apply`, ya que la lambda está encapsulada dentro de un objeto que implementa la interfaz `Function`. Esto evidencia una diferencia clave respecto a C: en lugar de manejar direcciones de memoria, se trabaja con abstracciones de más alto nivel.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El **paradigma funcional** es un estilo de programación que se centra en el uso de funciones como elemento principal del programa. En este enfoque, las funciones se consideran entidades que reciben datos y devuelven resultados sin modificar el estado externo (evitando efectos secundarios). Se promueve el uso de funciones puras, la inmutabilidad de los datos y la composición de funciones, lo que permite escribir código más predecible y fácil de razonar, en contraste con el enfoque imperativo típico de C, donde el estado y las variables se modifican constantemente.

A partir de Java 8, se dice que lenguajes como **Java son multi-paradigma** porque combinan varios estilos de programación dentro del mismo lenguaje. Tradicionalmente, Java es orientado a objetos, pero con la introducción de lambdas, interfaces funcionales y streams, también permite aplicar conceptos del paradigma funcional. Esto significa que el programador puede elegir entre un enfoque más clásico basado en objetos o uno más funcional, según convenga en cada caso, integrando ambos sin necesidad de cambiar de lenguaje.

Decir que las funciones son **“ciudadanos de primera clase”** significa que se pueden tratar como cualquier otro valor en el lenguaje. Es decir, se pueden almacenar en variables, pasar como parámetros a otras funciones y devolver como resultado. Esto ya se empieza a ver en C con los punteros a función, pero de forma más limitada y menos cómoda. En lenguajes modernos como Java (con lambdas) o JavaScript, esta característica está mucho más integrada y simplificada.

En conjunto, estas ideas acercan la programación funcional a desarrolladores que vienen de paradigmas imperativos u orientados a objetos. Se puede observar que muchos conceptos, como el uso de funciones como parámetros o valores, tienen cierto paralelismo con mecanismos ya conocidos (por ejemplo, el polimorfismo en Java), pero con un enfoque más basado en funciones que en objetos.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis básica de una **función lambda en Java** se introdujo a partir de Java 8 y permite definir funciones anónimas de forma compacta. Su forma general es:

```java
(parámetros) -> expresión
```

o, si el cuerpo es más complejo:

```java
(parámetros) -> {
    // varias instrucciones
    return valor;
}
```

En esta sintaxis, a la izquierda de `->` se especifican los parámetros de entrada (con o sin tipo, si puede inferirse), y a la derecha el cuerpo de la función. Si el cuerpo es una única expresión, no es necesario usar llaves ni la palabra clave `return`, ya que el valor se devuelve automáticamente.

Las lambdas en Java siempre se utilizan en el contexto de una **interfaz funcional**, es decir, una interfaz con un único método abstracto. Por ejemplo, `Function<T, R>` define un método `apply(T t)` que devuelve un valor de tipo `R`. La lambda proporciona directamente la implementación de ese método, sin necesidad de crear una clase explícita ni una clase anónima como se hacía antes.

Un ejemplo sencillo usando `Function<String, String>` sería:

```java
import java.util.function.Function;

Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();
```

En este caso, se puede observar que el parámetro `cadena` no tiene tipo explícito porque Java lo infiere (`String`). La lambda implementa el método `apply`, y su invocación se realiza mediante:

```java
String resultado = aMayusculas.apply("hola");
```

En resumen, la sintaxis de las funciones lambda simplifica notablemente la definición de comportamientos pequeños y reutilizables. Permite escribir código más claro y cercano al paradigma funcional, evitando la sobrecarga sintáctica de definir clases o métodos completos cuando solo se necesita una operación sencilla.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Para recibir una función como parámetro y utilizarla dentro de un método, se aplica la idea de que las funciones pueden tratarse como valores (“ciudadanos de primera clase”). En este caso, el método `transformar` recibe dos parámetros: una cadena y una función que define cómo transformarla. Desde dentro del método, simplemente se invoca esa función con la cadena recibida, devolviendo el resultado.

En **JavaScript**, esto es muy natural porque las funciones son objetos de primera clase. Se puede pasar directamente una lambda como argumento o almacenarla en una variable y reutilizarla. El método `transformar` recibe la cadena y la función, y la ejecuta como una llamada normal:

```javascript
// Función lambda
let aMayusculas = (cadena) => cadena.toUpperCase();

// Método que recibe una función
function transformar(cadena, funcion) {
    return funcion(cadena);
}

// Uso
let resultado = transformar("Hola mundo", aMayusculas);
console.log(resultado);  // HOLA MUNDO
```

En **Java**, el mecanismo es similar conceptualmente, pero se basa en interfaces funcionales. El método `transformar` recibe un `Function<String, String>`, y se utiliza su método `apply` para invocar la lambda. Esto permite desacoplar el comportamiento concreto de la operación, de forma parecida a como se hacía con polimorfismo, pero con un enfoque funcional:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        String resultado = transformar("Hola mundo", aMayusculas);
        System.out.println(resultado);  // HOLA MUNDO
    }

    public static String transformar(String cadena, Function<String, String> funcion) {
        return funcion.apply(cadena);
    }
}
```

En ambos ejemplos se observa que el método `transformar` no depende de una implementación concreta, sino que recibe el comportamiento como parámetro. Esto aporta gran flexibilidad y reutilización del código, alineándose con el paradigma funcional y complementando las técnicas ya conocidas de la programación orientada a objetos.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Para este caso, se utiliza el mismo método `transformar`, pero en lugar de pasar una función previamente almacenada en una variable, se define directamente una **función lambda en el momento de la llamada**. Esto muestra una de las ventajas clave del paradigma funcional: poder crear comportamientos “al vuelo” sin necesidad de declararlos por separado.

En **JavaScript**, esto resulta especialmente simple, ya que la lambda se puede escribir directamente como segundo argumento de la función `transformar`. En el ejemplo siguiente, la función lambda invierte la cadena recibida:

```javascript
function transformar(cadena, funcion) {
    return funcion(cadena);
}

// Llamada con lambda inline que invierte la cadena
let resultado = transformar("Hola mundo", (cadena) => {
    return cadena.split("").reverse().join("");
});

console.log(resultado);  // odnum aloH
```

En este caso, la lambda se define en la propia llamada, convirtiendo la cadena en un array de caracteres (`split`), invirtiéndolo (`reverse`) y reconstruyendo la cadena (`join`). No es necesario darle nombre a la función, ya que solo se utiliza en este punto.

En **Java**, el concepto es el mismo, aunque se mantiene la necesidad de utilizar una interfaz funcional como `Function<String, String>`. La lambda también se puede definir directamente en la llamada:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        String resultado = transformar("Hola mundo", (cadena) -> {
            return new StringBuilder(cadena).reverse().toString();
        });

        System.out.println(resultado);  // odnum aloH
    }

    public static String transformar(String cadena, Function<String, String> funcion) {
        return funcion.apply(cadena);
    }
}
```

Aquí se utiliza `StringBuilder` para invertir la cadena mediante su método `reverse`. Al igual que en JavaScript, se observa que la función no necesita ser declarada previamente, lo que permite escribir código más compacto y expresivo. Este estilo es especialmente útil cuando la lógica es sencilla y no se va a reutilizar en otros lugares del programa.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un **cierre** o *closure* es una característica por la cual una función (incluida una lambda) puede acceder a variables de su contexto externo, incluso después de que ese contexto haya terminado su ejecución. Es decir, la función “recuerda” el entorno en el que fue creada y puede utilizar las variables definidas fuera de ella. Esto es fundamental en el paradigma funcional, ya que permite construir funciones más flexibles y reutilizables.

En **Java**, las lambdas pueden capturar variables locales del entorno donde se definen, pero con una restricción importante: dichas variables deben ser **finales o efectivamente finales** (es decir, que no cambien su valor tras ser inicializadas). Esto garantiza seguridad y evita problemas en la ejecución, ya que Java no permite modificar directamente variables locales capturadas dentro de una lambda.

A continuación se muestra una modificación del ejemplo anterior, donde se define una variable local externa que se usa dentro de la función lambda para concatenar texto:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        String sufijo = "!!!";  // Variable local capturada por la lambda

        String resultado = transformar("Hola mundo", (cadena) -> {
            return cadena + sufijo;
        });

        System.out.println(resultado);  // Hola mundo!!!
    }

    public static String transformar(String cadena, Function<String, String> funcion) {
        return funcion.apply(cadena);
    }
}
```

En este ejemplo, la lambda accede a la variable `sufijo`, que está definida fuera de ella. Esto demuestra el concepto de closure: la función no solo utiliza su parámetro (`cadena`), sino también información del entorno donde fue creada. Este comportamiento resulta muy potente, ya que permite personalizar funciones sin necesidad de pasar todos los datos explícitamente como parámetros, manteniendo el código más limpio y expresivo.


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La diferencia principal entre una **función lambda** y un **puntero a función en C** reside en el nivel de abstracción y en cómo se integran en el lenguaje. Un puntero a función en C es simplemente una dirección de memoria que apunta al código de una función concreta. No hay información adicional ni contexto asociado: solo permite invocar esa función. En cambio, una función lambda en lenguajes modernos como Java o JavaScript es un objeto de alto nivel que puede encapsular comportamiento junto con el entorno donde fue definida.

Otra diferencia clave es que las lambdas permiten la creación de **cierres (closures)**, lo que significa que pueden acceder a variables externas a su propio cuerpo. Esto no ocurre con los punteros a función en C, donde no existe un mecanismo automático para capturar el entorno. Si se necesitara algo similar en C, habría que simularlo pasando explícitamente estructuras con datos adicionales como parámetros, lo que resulta más complejo y menos natural.

Además, las funciones lambda se integran con el sistema de tipos del lenguaje. En Java, por ejemplo, una lambda siempre está asociada a una **interfaz funcional**, lo que permite verificar en tiempo de compilación que su firma es correcta. En C, aunque el compilador también comprueba tipos, el uso de punteros a función es más propenso a errores y menos expresivo, ya que no hay una abstracción equivalente a “funciones como objetos” con comportamiento rico.

Finalmente, desde el punto de vista conceptual, los punteros a función pertenecen a un enfoque más **imperativo y de bajo nivel**, mientras que las funciones lambda forman parte del **paradigma funcional**, donde las funciones son tratadas como valores de primera clase. Esto hace que las lambdas sean más flexibles, seguras y fáciles de combinar, acercándose más a ideas como pasar comportamiento, composición de funciones o programación declarativa, que no están presentes de forma natural en C.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Se puede definir una función que **devuelva otra función** gracias a las lambdas y a las interfaces funcionales de Java. En este caso, `crearDescuento` recibe un porcentaje y devuelve una función que, dada una cantidad, aplica dicho descuento. Esto encaja directamente con el paradigma funcional, donde las funciones no solo reciben datos, sino que también pueden generarse dinámicamente como resultado.

A continuación se muestra una implementación usando `Function<Double, Double>`:

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Crear dos funciones de descuento distintas
        Function<Double, Double> descuento10 = crearDescuento(10.0);
        Function<Double, Double> descuento25 = crearDescuento(25.0);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio));  // 90.0
        System.out.println(descuento25.apply(precio));  // 75.0
    }

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return (cantidad) -> cantidad * (1 - porcentaje / 100.0);
    }
}
```

En este ejemplo, `crearDescuento` no devuelve un valor numérico, sino una función que después puede aplicarse a distintas cantidades. Cada llamada a `crearDescuento` genera una nueva lambda con su propio comportamiento, en función del porcentaje recibido. Esto permite reutilizar la lógica sin duplicar código y sin necesidad de clases adicionales.

El concepto de **closure** aparece porque la lambda devuelta utiliza la variable `porcentaje`, que está definida en el contexto externo de `crearDescuento`. Aunque el método haya terminado su ejecución, la función resultante “recuerda” el valor de `porcentaje` con el que fue creada. Cada función de descuento mantiene su propio valor capturado, lo que permite que `descuento10` y `descuento25` funcionen de manera independiente. Este comportamiento es una de las diferencias fundamentales respecto a C, donde no existe captura automática del entorno.


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una **interfaz funcional** en Java es una interfaz que define un único método abstracto, y que se utiliza como tipo para representar funciones lambda. Es el mecanismo que permite integrar las lambdas dentro del sistema de tipos estático de Java. Por ejemplo, cuando se usa `Function<String, String>`, se está utilizando una interfaz funcional cuyo único método abstracto es `apply`.

El requisito fundamental de una interfaz funcional es que tenga **exactamente un método abstracto**. Puede tener más métodos, pero estos deben ser `default` o `static`, ya que no cuentan como abstractos. Además, puede sobrescribir métodos de `Object` como `toString` o `equals`, sin perder su condición de funcional. Para dejarlo explícito, Java proporciona la anotación `@FunctionalInterface`, que no es obligatoria, pero ayuda a detectar errores en compilación si se incumple esta regla.

Las interfaces funcionales son necesarias porque Java no trata las funciones como tipos independientes (a diferencia de JavaScript). En su lugar, una lambda es una forma abreviada de crear una instancia de una interfaz funcional. Internamente, la lambda implementa el único método abstracto de esa interfaz. Esto permite mantener la comprobación estática de tipos y la compatibilidad con el modelo orientado a objetos.

Algunos ejemplos habituales de interfaces funcionales en Java son `Function<T, R>`, `Predicate<T>` o `Consumer<T>`. Todas ellas siguen la misma idea: representan distintos tipos de funciones (transformaciones, condiciones, acciones, etc.). Gracias a este mecanismo, Java puede soportar programación funcional sin abandonar su diseño basado en tipos estáticos y clases.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Una **interfaz funcional propia** se define de la misma manera que cualquier otra interfaz en Java, pero cumpliendo la condición de tener un único método abstracto. En este caso, se quiere modelar una función que recibe un `String` y devuelve otro `String`, por lo que se puede definir una interfaz llamada `Transformador` con ese comportamiento.

La definición sería la siguiente:

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String cadena);
}
```

La anotación `@FunctionalInterface` no es obligatoria, pero es recomendable porque permite al compilador verificar que la interfaz cumple correctamente con el requisito de tener un solo método abstracto. Si se añadiese accidentalmente otro método abstracto, se produciría un error de compilación.

Una vez definida la interfaz, se puede utilizar con funciones lambda de forma similar a como se hacía con `Function<String, String>`. Por ejemplo:

```java
public class Main {
    public static void main(String[] args) {
        Transformador aMayusculas = (cadena) -> cadena.toUpperCase();

        String resultado = aMayusculas.transformar("Hola mundo");
        System.out.println(resultado);  // HOLA MUNDO
    }
}
```

En este caso, la lambda proporciona la implementación del método `transformar` de la interfaz. Esto demuestra cómo se pueden crear interfaces funcionales personalizadas adaptadas a necesidades concretas, manteniendo la misma filosofía que las interfaces funcionales estándar de Java, pero con nombres y semántica más expresivos para el dominio del problema.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Para hacer la interfaz funcional más general, se puede utilizar **genericidad (generics)**, de forma que el tipo de entrada y el tipo de salida no estén fijados a `String`, sino que puedan adaptarse a cualquier tipo. Esto es similar a lo que ya ocurre con `Function<T, R>` en la librería estándar, pero en este caso se define una interfaz propia más flexible y reutilizable.

La interfaz genérica `Transformador` podría definirse de la siguiente manera:

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```

Aquí, `T` representa el tipo de entrada y `R` el tipo de salida. De este modo, la misma interfaz puede utilizarse para transformar cadenas, números u otros objetos, manteniendo siempre la misma estructura. Esta generalización es muy potente y está alineada con el uso de plantillas en C++ o generics en Java que ya se han visto previamente.

A partir de esta interfaz, se puede crear un ejemplo concreto, como un transformador que redondea un `Double` a `Integer`. Esto se puede hacer fácilmente mediante una función lambda:

```java
public class Main {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = (valor) -> (int) Math.round(valor);

        Integer resultado = redondear.transformar(3.7);
        System.out.println(resultado);  // 4
    }
}
```

En este ejemplo, la lambda implementa el método `transformar`, recibiendo un `Double` y devolviendo un `Integer`. Se utiliza `Math.round` para redondear el valor, seguido de un cast a `int`. Se observa cómo la interfaz genérica permite reutilizar el mismo patrón funcional en distintos contextos, mejorando la expresividad y reduciendo la necesidad de crear múltiples interfaces específicas para cada tipo de dato.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
En Java ya existen varias **interfaces funcionales predefinidas** dentro del paquete `java.util.function`, diseñadas para cubrir los casos más habituales al trabajar con funciones lambda. Estas interfaces evitan tener que definir versiones propias como `Transformador`, ya que proporcionan abstracciones estándar y reutilizables. Entre ellas, la más general es `Function<T, R>`, que representa una transformación de un tipo `T` a otro `R`.

Algunas de las interfaces funcionales más utilizadas son:

*   `Function<T, R>`: recibe un valor y devuelve otro (`R apply(T t)`).
*   `Predicate<T>`: recibe un valor y devuelve un booleano (`boolean test(T t)`).
*   `Consumer<T>`: recibe un valor y no devuelve nada (`void accept(T t)`).
*   `Supplier<T>`: no recibe parámetros y devuelve un valor (`T get()`).

Ejemplo breve de uso:

```java
Function<String, String> f = s -> s.toUpperCase();
Predicate<Integer> esPar = n -> n % 2 == 0;
Consumer<String> imprimir = s -> System.out.println(s);
Supplier<Double> aleatorio = () -> Math.random();
```

Además, existen variantes especializadas para trabajar con tipos primitivos y evitar el coste del *boxing*, como `IntFunction`, `DoublePredicate`, `IntConsumer`, etc. También hay versiones con dos parámetros como `BiFunction<T, U, R>` o `BiPredicate<T, U>`. Esto amplía aún más la utilidad de estas interfaces en situaciones reales.

En conjunto, estas interfaces funcionales forman una base estándar para la programación funcional en Java. Permiten escribir código más expresivo, reutilizable y consistente, evitando la necesidad de crear nuevas interfaces para cada caso concreto, como ocurría antes de Java 8.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
El método `forEach` de la interfaz `List` en Java es una forma funcional de recorrer colecciones, introducida en Java 8. En lugar de utilizar un bucle `for` tradicional, `forEach` recibe una función (lambda) que se ejecuta para cada elemento de la lista. Esto permite expresar el recorrido de manera más declarativa: se indica **qué hacer con cada elemento**, en lugar de describir paso a paso cómo iterar sobre la colección.

Desde el punto de vista funcional, `forEach` espera un `Consumer<T>`, es decir, una función que recibe un elemento y no devuelve ningún valor. Esto encaja con operaciones como imprimir, acumular o realizar comprobaciones. En comparación con un bucle `for`, se elimina la gestión explícita del índice o del iterador, lo que hace el código más conciso y legible, especialmente para operaciones simples.

A continuación se muestra un ejemplo donde se recorre una lista de enteros y se muestra un mensaje si el valor es positivo:

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = List.of(3, -1, 0, 7, -5, 2);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("Número positivo: " + n);
            }
        });
    }
}
```

En este ejemplo, la lambda `(n -> { ... })` se ejecuta para cada elemento de la lista. Se observa que no se define ningún bucle explícito, sino que se delega el recorrido en `forEach`, proporcionando únicamente la acción a realizar. Este estilo es característico de la programación funcional: se favorece la **aplicación de funciones sobre colecciones** frente al control manual del flujo de iteración, haciendo el código más expresivo y alineado con las nuevas características de Java.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
El uso de `Consumer<? super T>` en la firma de `forEach` está relacionado con las reglas de **variancia en generics** y con el principio conocido como **PECS** (*Producer Extends, Consumer Super*). Este principio indica que, si un tipo **produce** valores (solo se leen), se debe usar `? extends T`, y si **consume** valores (solo se escriben o reciben), se debe usar `? super T`. En el caso de `forEach`, el `Consumer` recibe elementos de la lista (los “consume”), por eso se utiliza `? super T` en lugar de `T`.

Esto permite mayor flexibilidad en el uso del método. Por ejemplo, si se tiene una lista de `Integer`, se puede pasar un `Consumer<Number>` (ya que `Number` es supertipo de `Integer`). Si la firma fuese estrictamente `Consumer<T>`, esto no sería posible. Con `? super T`, Java permite usar consumidores más generales, lo que hace el código más reutilizable y menos restrictivo.

Aplicando **PECS** al método `transformar`, la idea es analizar cómo se usa la función transformadora. Esta función **consume** un valor de tipo `T` (lo recibe como entrada) y **produce** un valor de tipo `R` (lo devuelve). Por tanto, siguiendo PECS:

*   Para el parámetro de entrada → `? super T` (consume)
*   Para el valor de salida → `? extends R` (produce)

Así, una versión más flexible de `transformar` sería:

```java
public static <T, R> R transformar(T valor, Function<? super T, ? extends R> funcion) {
    return funcion.apply(valor);
}
```

Con esta definición, el método es más general y admite funciones más amplias. Por ejemplo, se podría pasar una función que acepte `Object` en lugar de `T`, o que devuelva un subtipo de `R`. En resumen, el uso de **PECS** permite diseñar APIs genéricas más flexibles, evitando restricciones innecesarias y facilitando la reutilización del código en distintos contextos.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Las **referencias a métodos** permiten tratar métodos de objetos o clases como valores, de forma similar a las funciones lambda. En lugar de escribir una lambda explícita, se puede usar una sintaxis más compacta que referencia directamente a un método existente. Esto resulta útil cuando ya existe un método que encaja con la interfaz funcional requerida, mejorando la legibilidad del código.

En **JavaScript**, los métodos de un objeto pueden asignarse a una variable, pero hay que tener cuidado con el contexto (`this`). Si se extrae directamente el método, puede perder su referencia al objeto original, por lo que suele ser necesario usar `bind` para fijar el contexto:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

// Crear objeto
let p = new Persona("Martín");

// Obtener referencia al método (asegurando el contexto)
let refSaludar = p.saludar.bind(p);

// Invocar mediante la referencia
refSaludar();  // Hola, soy Martín
```

En **Java**, las referencias a métodos forman parte del lenguaje desde Java 8 y tienen una sintaxis específica con `::`. Se pueden obtener referencias tanto de métodos de instancia como de métodos estáticos, siempre que encajen con una interfaz funcional:

```java
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
```

```java
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {
        Persona p = new Persona("Martín");

        // Referencia al método de instancia
        Runnable refSaludar = p::saludar;

        // Invocación
        refSaludar.run();  // Hola, soy Martín
    }
}
```

En este caso, `p::saludar` es equivalente a una lambda como `() -> p.saludar()`, pero con una sintaxis más compacta. Se observa que Java gestiona automáticamente el contexto del objeto (`p`), a diferencia de JavaScript. Las referencias a métodos son especialmente útiles cuando se combinan con APIs funcionales como `forEach`, ya que permiten reutilizar métodos existentes sin necesidad de escribir nuevas lambdas.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
En Java existen varios **tipos de referencias a métodos**, todas con la sintaxis general `Clase::método` o `objeto::método`. Estas referencias son una forma abreviada de escribir lambdas cuando ya existe un método que coincide con la firma requerida por una interfaz funcional. Las principales categorías son: referencia a método estático, a constructor, a método de instancia de un objeto concreto y a método de instancia de cualquier objeto de un tipo.

Un ejemplo de **referencia a método estático** consiste en apuntar a un método de clase sin necesidad de crear objetos. Por ejemplo:

```java
import java.util.function.Function;

Function<Double, Double> raiz = Math::sqrt;

System.out.println(raiz.apply(16.0));  // 4.0
```

En cuanto a la **referencia a constructor**, permite crear objetos utilizando una interfaz funcional. Es equivalente a una lambda que invoca `new`:

```java
import java.util.function.Function;

Function<String, Persona> creador = Persona::new;

Persona p = creador.apply("Martín");
```

Para una **referencia a método de instancia de un objeto concreto**, se utiliza un objeto ya creado y se referencia su método. Es equivalente a una lambda sin parámetros que invoca dicho método:

```java
Persona p = new Persona("Martín");

Runnable ref = p::saludar;
ref.run();  // Hola, soy Martín
```

Finalmente, una **referencia a método de instancia sobre cualquier instancia** se usa cuando el objeto sobre el que se invoca el método se pasa como parámetro. Es más abstracta y muy útil con colecciones:

```java
import java.util.function.Function;

Function<String, String> aMayusculas = String::toUpperCase;

System.out.println(aMayusculas.apply("hola"));  // HOLA
```

En este último caso, `String::toUpperCase` equivale a una lambda como `(s) -> s.toUpperCase()`. En resumen, las referencias a métodos simplifican aún más la sintaxis de las lambdas cuando ya existe una implementación reutilizable, haciendo el código más limpio y expresivo dentro del estilo funcional de Java.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
Para ordenar una lista de objetos `Persona` en Java utilizando un estilo funcional, se puede emplear `Collections.sort` junto con una función lambda como comparador. Este comparador recibe dos objetos y devuelve un valor negativo, cero o positivo según el orden. En este caso, se requiere comparar primero por edad y, en caso de empate, ordenar alfabéticamente por nombre. Este enfoque permite definir la lógica de ordenación directamente en el punto de uso.

Primero, se crea la clase `Persona`:

```java
public class Persona {
    String nombre;
    int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
}
```

Una **primera versión (manual)** del comparador usando una lambda podría ser:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Luis", 30));
        personas.add(new Persona("Carlos", 25));

        Collections.sort(personas, (p1, p2) -> {
            if (p1.edad != p2.edad) {
                return p1.edad - p2.edad;
            } else {
                return p1.nombre.compareTo(p2.nombre);
            }
        });

        personas.forEach(p -> System.out.println(p.nombre + " " + p.edad));
    }
}
```

Una **segunda versión más expresiva** utiliza la clase `Comparator`, que proporciona métodos auxiliares para componer comparaciones de forma más clara y reutilizable:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>();
        personas.add(new Persona("Ana", 25));
        personas.add(new Persona("Luis", 30));
        personas.add(new Persona("Carlos", 25));

        Collections.sort(personas,
            Comparator.comparingInt((Persona p) -> p.edad)
                      .thenComparing(p -> p.nombre)
        );

        personas.forEach(p -> System.out.println(p.nombre + " " + p.edad));
    }
}
```

En la segunda versión, se observa un estilo más declarativo: primero se indica el criterio principal (`edad`) y luego el secundario (`nombre`). Esto mejora la legibilidad y facilita la extensión del código. Este enfoque es un ejemplo claro de cómo Java combina programación orientada a objetos y funcional, proporcionando herramientas más expresivas para tareas comunes como la ordenación.
