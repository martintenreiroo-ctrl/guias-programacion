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

### Respuesta
Un enfoque clásico para permitir que una **estructura de datos almacene cualquier tipo** es utilizar un **array primitivo de punteros genéricos en C (`void*`)** o un **array de referencias a `Object` en Java**. En ambos casos, la idea es similar: el contenedor no conoce el tipo concreto de los elementos que almacena, sino que guarda referencias genéricas. Este concepto es un antecedente directo de la **genericidad**, pero sin garantías de seguridad de tipos.

En **C**, se puede definir una estructura que contenga un array de `void*`. Dado que `void*` puede apuntar a cualquier tipo de dato, permite almacenar direcciones de enteros, estructuras o cualquier otro tipo. Sin embargo, al recuperar un elemento es necesario **hacer un casting explícito** al tipo correcto, ya que el compilador no puede comprobar si dicho tipo es compatible. Esto implica que los errores se detectan solo en tiempo de ejecución y que el programador debe gestionar manualmente la coherencia de tipos.

En **Java**, un diseño equivalente consiste en utilizar un array de `Object`, ya que todas las clases heredan directa o indirectamente de `Object`. Esto permite almacenar objetos de cualquier tipo, usando el **polimorfismo básico** del lenguaje. No obstante, al recuperar los elementos también es necesario hacer *downcasting* al tipo esperado, con el riesgo de provocar una `ClassCastException` si el tipo no coincide. Este problema motivó la introducción de los **genéricos**, que proporcionan seguridad de tipos en tiempo de compilación.

Este tipo de soluciones son flexibles, pero **no seguras desde el punto de vista del tipo**, y por ello se consideran una solución de bajo nivel. Tanto en C como en Java, representan una forma de “heterogeneidad manual”, similar a lo que se haría con punteros genéricos en C estructurado, y sirven como base conceptual para entender por qué la genericidad es una mejora importante.

***

### Ejemplo en C usando `void*`

```c
typedef struct {
    void* datos[10];
    int size;
} Lista;
```

***

### Ejemplo en Java usando `Object`

```java
class Lista {
    private Object[] datos = new Object[10];
    private int size = 0;

    public void add(Object obj) {
        datos[size++] = obj;
    }

    public Object get(int i) {
        return datos[i];
    }
}
```

En ambos casos, la estructura permite almacenar cualquier tipo, pero obliga al programador a **recordar y convertir manualmente** el tipo original al recuperar los datos.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La **programación genérica** consiste en diseñar **algoritmos y estructuras de datos independientes del tipo concreto de los datos que manejan**. En lugar de escribir una versión distinta de una estructura (lista, pila, array, etc.) para cada tipo, se define una única implementación que puede trabajar con muchos tipos distintos. El objetivo principal es **reutilizar código sin perder coherencia conceptual ni seguridad**, evitando duplicaciones innecesarias.

Desde un punto de vista conceptual, la programación genérica busca separar **la lógica del contenedor o del algoritmo** de **los tipos específicos que se almacenan o procesan**. En Java moderno esto se consigue mediante **genéricos (`<T>`)**, que permiten expresar esa independencia de tipos y, además, comprobar en **tiempo de compilación** que los tipos usados son correctos. En C estructurado no existe este mecanismo de forma nativa, por lo que se recurren a soluciones manuales.

El ejemplo anterior basado en `void*` en C o en `Object` en Java **no es todavía programación genérica en sentido estricto**, aunque se le considera un **antecedente**. En ese enfoque, el contenedor acepta cualquier tipo, pero el compilador **no puede garantizar la corrección de los tipos**, obligando a usar conversiones explícitas al recuperar los datos. Esto introduce riesgos de errores en tiempo de ejecución, como accesos inválidos o fallos de tipo.

Por tanto, puede afirmarse que el ejemplo anterior muestra una **heterogeneidad no tipada**, pero no verdadera programación genérica. La programación genérica real aparece cuando el lenguaje permite **expresar el tipo como un parámetro**, garantizando que los datos insertados y recuperados sean coherentes, algo que los genéricos de Java resuelven directamente y que no se consigue solo con `Object` o `void*`.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema respecto al **chequeo de tipos** al emplear `void*` en C o `Object` en Java es que **se pierde la información del tipo original** al almacenar los elementos en la estructura de datos. El contenedor acepta cualquier tipo, pero el compilador ya no puede verificar si los datos insertados y recuperados son compatibles. Como consecuencia, se delega completamente en el programador la responsabilidad de recordar qué tipo hay en cada posición.

Al recuperar un elemento, es necesario realizar un **casting explícito** al tipo esperado. Este proceso no está garantizado por el compilador y puede ser incorrecto sin que se detecte en tiempo de compilación. En C, un *cast* erróneo puede provocar accesos a memoria inválidos o comportamientos indefinidos. En Java, un *downcasting* incorrecto produce una `ClassCastException` en tiempo de ejecución. En ambos lenguajes, el error se detecta **tarde**, cuando el programa ya se está ejecutando.

Otro problema importante es que estas estructuras **no documentan el tipo de dato que esperan contener**, lo que dificulta el mantenimiento del código y aumenta la probabilidad de errores al modificar o reutilizar la estructura. En proyectos grandes, resulta especialmente peligroso que distintas partes del programa asuman tipos distintos para los mismos datos almacenados en un contenedor genérico no tipado.

En resumen, el uso de `void*` o `Object` permite flexibilidad, pero **elimina el chequeo estático de tipos**, que es una de las principales herramientas para prevenir errores. Precisamente para resolver estos problemas se introducen los **genéricos**, que permiten expresar el tipo de los elementos de una estructura y hacer que el compilador garantice la coherencia entre inserciones y accesos desde el inicio.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los **parámetros de tipo** son un mecanismo de la **programación genérica** que permite **parametrizar clases, métodos o interfaces con uno o varios tipos**, haciendo que el tipo de los datos manejados no quede fijado en la definición, sino que se especifique en el momento de su uso. En Java, estos parámetros suelen representarse mediante identificadores como `T`, `E` o `K`, y actúan como **variables de tipo** que el compilador sustituye por tipos concretos al crear objetos o invocar métodos.

Gracias a los parámetros de tipo, es posible definir **estructuras de datos y algoritmos independientes del tipo concreto**, pero manteniendo el **chequeo estático de tipos en tiempo de compilación**. A diferencia de soluciones basadas en `Object`, el compilador conoce el tipo real asociado al parámetro genérico y puede verificar que solo se insertan y recuperan datos compatibles. Esto elimina la necesidad de *casting* manual y reduce la aparición de errores en tiempo de ejecución.

Desde un punto de vista conceptual, el uso de parámetros de tipo implica que **la estructura es genérica, pero su utilización es específica**. Por ejemplo, una lista puede definirse una sola vez y luego usarse como lista de enteros, de cadenas o de cualquier otro tipo, sin duplicar código. Esta idea conecta con prácticas en C estructurado, donde se imita este comportamiento con macros o `void*`, pero sin el respaldo del compilador.

En resumen, los parámetros de tipo permiten expresar explícitamente **la intención genérica del diseño**, combinando flexibilidad y seguridad de tipos. Constituyen la base de los genéricos en Java y representan una mejora fundamental frente a las técnicas tradicionales que ocultan los tipos y trasladan los errores al tiempo de ejecución.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
En **Java** y en **C++** existen mecanismos nativos de **programación genérica** que permiten definir estructuras de datos reutilizables, manteniendo **seguridad de tipos en tiempo de compilación**. En Java este mecanismo se denomina **genéricos (*generics*)**, mientras que en C++ se denomina **plantillas (*templates*)**. En ambos casos, el tipo concreto se especifica al instanciar la estructura, evitando el uso de `Object`, `void*` o conversiones explícitas inseguras.

En **Java**, los genéricos permiten declarar colecciones indicando el tipo de elementos que pueden almacenar. Al instanciar una lista como `List<String>`, el compilador garantiza que solo se insertan objetos de tipo `String` y que, al recorrer la colección, cada elemento recuperado es ya de ese tipo concreto. Esto elimina completamente el *casting* manual y previene errores típicos que antes solo se detectaban en tiempo de ejecución.

En **C++**, los *templates* permiten un efecto similar, pero con una diferencia conceptual: el compilador genera **una versión concreta del código para cada tipo usado**. Al utilizar `std::vector<std::string>`, el contenedor queda especializado para manejar únicamente cadenas, y cualquier intento de insertar otro tipo produce un error de compilación. Desde el punto de vista del programador, el uso es muy similar al de Java, aunque el mecanismo interno sea distinto.

En ambos lenguajes, estos enfoques representan **programación genérica real**, ya que combinan reutilización de código con chequeo estático de tipos. Frente a soluciones basadas en `Object` o `void*`, se gana claridad, seguridad y mantenibilidad, manteniendo la flexibilidad necesaria para trabajar con distintos tipos de datos.

***

### Ejemplo en Java (generics)

```java
import java.util.ArrayList;
import java.util.List;

List<String> lista = new ArrayList<>();
lista.add("uno");
lista.add("dos");
lista.add("tres");

for (String s : lista) {
    System.out.println(s.toUpperCase());
}
```

***

### Ejemplo en C++ (templates)

```cpp
#include <vector>
#include <string>
#include <iostream>

std::vector<std::string> v;
v.push_back("uno");
v.push_back("dos");
v.push_back("tres");

for (const std::string& s : v) {
    std::cout << s << std::endl;
}
```

En ambos ejemplos, cada elemento recuperado del contenedor es **del tipo concreto `String` / `std::string` con total seguridad**, validada por el compilador antes de ejecutar el programa.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Cuando se **instancia una clase genérica**, el compilador debe adaptar esa definición genérica a un **tipo concreto**. Es decir, una clase definida con un parámetro de tipo (por ejemplo, `<T>`) no se usa tal cual, sino que se especializa para el tipo indicado en su uso (`String`, `Integer`, etc.). El objetivo es que el código pueda escribirse una sola vez y reutilizarse con distintos tipos, manteniendo coherencia y seguridad.

En **Java**, el compilador aplica un mecanismo llamado **type erasure** (borrado de tipos). Durante la compilación, la información de los parámetros genéricos **se elimina**, y el código resultante trabaja internamente como si usara `Object` (o el límite superior del tipo). El chequeo de tipos se realiza **solo en tiempo de compilación**, y en tiempo de ejecución no existe información sobre `T`. Por esta razón, no se pueden usar genéricos con tipos primitivos ni consultar el tipo genérico real en ejecución.

En **C++**, el mecanismo es distinto y se denomina **instanciación de plantillas** (*template instantiation*). El compilador genera **una versión concreta del código para cada tipo utilizado**. Por ejemplo, `vector<int>` y `vector<string>` producen dos implementaciones diferentes. Esto implica que el tipo se conserva completamente en tiempo de compilación y ejecución, lo que permite una mayor flexibilidad (como trabajar con tipos primitivos), a costa de un mayor tamaño del código generado.

En resumen, **Java y C++ no hacen lo mismo**: Java usa **type erasure**, priorizando compatibilidad y simplicidad del lenguaje, mientras que C++ usa **instanciación real de código**, priorizando eficiencia y expresividad. Ambos enfoques permiten programación genérica con seguridad de tipos, pero con diferencias claras en funcionamiento interno y capacidades.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
Una **clase con parámetros de tipo** permite definir una estructura que puede almacenar valores de **tipos distintos sin fijarlos en la definición**, quedando dichos tipos determinados en el momento de uso. En Java, esto se expresa indicando los parámetros entre `< >` en la declaración de la clase. De esta forma, una clase puede reutilizarse para combinar distintos tipos manteniendo **seguridad de tipos en tiempo de compilación**, sin recurrir a `Object` ni a conversiones explícitas.

La clase `Par` es un ejemplo típico de este enfoque: permite almacenar **dos valores potencialmente de distinto tipo**, por ejemplo `<A, B>`. Cada parámetro de tipo actúa como una variable de tipo que el compilador sustituye por tipos concretos cuando se instancia la clase. Esto hace posible que el compilador verifique que los valores asignados y recuperados son coherentes, eliminando errores de tipo en tiempo de ejecución.

Este diseño resulta especialmente útil para **valores de retorno compuestos**, donde una función necesita devolver más de un resultado. Frente a soluciones clásicas en C (como usar punteros de salida o estructuras específicas), el uso de una clase genérica permite expresar de forma clara y tipada qué información se devuelve, sin crear una clase distinta para cada combinación posible de tipos.

En el ejemplo siguiente, se utiliza `Par<Double, Double>` para devolver conjuntamente la **media** y la **desviación típica** de un array de `double`. El uso de genéricos garantiza que ambos valores se recuperan directamente como `Double`, sin *casting* manual y con plena seguridad de tipos.

***

### Definición de la clase `Par` en Java

```java
class Par<A, B> {
    private A primero;
    private B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}
```

***

### Ejemplo de uso: media y desviación típica

```java
public static Par<Double, Double> mediaYDesviacion(double[] datos) {
    double suma = 0.0;
    for (double d : datos) {
        suma += d;
    }
    double media = suma / datos.length;

    double sumaCuadrados = 0.0;
    for (double d : datos) {
        sumaCuadrados += Math.pow(d - media, 2);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}
```

Este ejemplo muestra cómo una clase genérica permite **expresar claramente el significado del retorno**, combinando flexibilidad, reutilización de código y chequeo estático de tipos.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
En Java es posible declarar **parámetros de tipo a nivel de método**, independientemente de si la clase es genérica o no. Un **método genérico** define sus propios parámetros de tipo entre `< >` antes del tipo de retorno, lo que le permite trabajar de forma genérica sin depender del estado ni de los tipos de la clase que lo contiene. Esto resulta especialmente útil para operaciones puntuales que deben ser reutilizables con distintos tipos, sin necesidad de crear una clase genérica completa.

Si se define un método `seleccionaUno` utilizando **`Object`**, el método puede aceptar cualquier tipo de objeto, pero se pierden dos garantías importantes. En primer lugar, el método solo puede devolver un `Object`, obligando al código cliente a realizar *downcasting*, con el consiguiente riesgo de errores en tiempo de ejecución. En segundo lugar, no existe ninguna restricción que impida pasar dos objetos de **tipos distintos**, lo que puede dar lugar a incoherencias lógicas que el compilador no detecta.

En cambio, si se define `seleccionaUno` como un **método genérico con un parámetro de tipo `<T>`**, el compilador garantiza que ambos argumentos son del **mismo tipo** y que el valor devuelto también lo es. Esto permite evitar completamente el *downcasting* y trasladar el chequeo de tipos al **tiempo de compilación**, reforzando la seguridad y la claridad del código. Este enfoque es un ejemplo claro de programación genérica bien tipada, frente a soluciones más débiles basadas en `Object`.

En resumen, el uso de parámetros de tipo en métodos permite escribir código más expresivo y seguro. Frente al uso de `Object`, los métodos genéricos evitan errores, documentan mejor las intenciones del programador y aprovechan el sistema de tipos del lenguaje para detectar incoherencias lo antes posible.

***

### Versión sin genéricos (usando `Object`)

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
```

Uso problemático:

```java
String s = (String) seleccionaUno("hola", "adios"); // requiere casting
Object o = seleccionaUno("hola", 5); // permitido, pero incoherente
```

***

### Versión con método genérico

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
```

Uso seguro:

```java
String s = seleccionaUno("hola", "adios"); // sin casting
// seleccionaUno("hola", 5); // error de compilación
```

Esta diferencia ilustra claramente cómo los **métodos genéricos** mejoran tanto la seguridad de tipos como la corrección del diseño.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, en Java **es posible establecer restricciones en los parámetros de tipo**, lo que se conoce como **parámetros de tipo acotados** (*bounded type parameters*). Esto permite indicar que un tipo genérico no puede ser cualquiera, sino que debe **heredar de una clase concreta o implementar una interfaz determinada**. De este modo, el compilador garantiza que el tipo genérico tiene al menos ciertos métodos o comportamientos disponibles, algo fundamental cuando se quiere “tratarlo como un número”.

Si se define simplemente una clase `Punto` con coordenadas de tipo `Number`, se consigue flexibilidad, ya que `Number` es la superclase de `Integer`, `Double`, `Float`, etc. Sin embargo, se pierde precisión en el chequeo de tipos: no queda reflejado si un punto trabaja con `Double`, `Integer` o una mezcla de ambos. Además, todas las operaciones deben hacerse usando la interfaz común de `Number`, lo que conduce a conversiones a `double`, `int`, etc., en cada cálculo.

Una alternativa más tipada consiste en usar **genéricos con restricción**, por ejemplo `<T extends Number>`. Con este enfoque, el compilador sabe que `T` es algún subtipo de `Number`, pero además **garantiza que ambos puntos usan exactamente el mismo tipo numérico**. Esto refuerza el diseño, documenta mejor la intención del código y evita incoherencias como mezclar `Integer` con `Double` sin querer, trasladando el error al tiempo de compilación.

Respecto al **type erasure**, en ambos casos Java elimina la información genérica tras la compilación. En tiempo de ejecución, el tipo genérico `T` se sustituye por su **límite superior**, que en este ejemplo es `Number`. Es decir, aunque el código fuente sea genérico, el bytecode final trabaja internamente con `Number`, manteniendo la seguridad de tipos gracias a las comprobaciones hechas en compilación.

***

### Solución 1: coordenadas directamente de tipo `Number`

```java
class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

***

### Solución 2: usando genéricos con restricción `<T extends Number>`

```java
class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

✅ **Tipo final tras la compilación (type erasure):**  
En la segunda solución, el parámetro genérico `T` se borra y se sustituye por `Number`. Es decir, en el bytecode el tipo real es `Number`, aunque el compilador ya haya garantizado que el uso sea correcto y coherente antes de ejecutar el programa.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
Las **dos soluciones permiten trabajar con distintos tipos numéricos sin duplicar la clase `Punto`**, pero no ofrecen el mismo nivel de **chequeo de tipos**. En la solución basada únicamente en `Number`, nada impide crear un punto con una coordenada entera y otra real, por ejemplo `new Punto(3, 4.5)`. Esto es posible porque ambas coordenadas se almacenan como `Number`, lo que aporta flexibilidad, pero **no impone homogeneidad ni coherencia de tipos** entre las coordenadas de un mismo objeto.

En cambio, la solución basada en **genéricos con restricción `<T extends Number>`** refuerza el chequeo de tipos de forma clara. Al instanciar un `Punto<T>`, se fija un único tipo concreto para ambas coordenadas, como `Punto<Integer>` o `Punto<Double>`. En este caso **no es posible mezclar tipos numéricos distintos** en las coordenadas del mismo punto, ya que el compilador lo detecta como un error. Este refuerzo hace explícita la intención del diseño y evita incoherencias que, de otro modo, solo se manifestarían en tiempo de ejecución.

Respecto a los métodos de acceso, en la solución **sin genéricos**, el método `getX` devuelve siempre un objeto de tipo `Number`. Esto obliga al código cliente a trabajar con la abstracción más general o a realizar conversiones explícitas si necesita un tipo concreto. En la solución **con genéricos**, `getX` devuelve un objeto de tipo `T`, que en el uso será, por ejemplo, `Integer` o `Double`, proporcionando así **información de tipo más precisa y segura** al consumidor de la clase.

En conclusión, aunque ambas soluciones son funcionales, el uso de **genéricos refuerza el chequeo estático de tipos**, impide combinaciones inconsistentes y mejora la claridad del diseño. La solución con `Number` prioriza flexibilidad, mientras que la solución genérica prioriza **seguridad, coherencia y expresividad**, trasladando posibles errores al tiempo de compilación.


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

### Respuesta
En este diseño se desea **reforzar el chequeo de tipos** para garantizar que el método `distanciaA` de un punto solo pueda calcular la distancia respecto a **otro punto del mismo subtipo**, evitando comprobaciones dinámicas con `instanceof` y conversiones explícitas (*downcasting*). Para lograrlo, se puede parametrizar la interfaz `Punto` con **genéricos autorreferenciados**, de forma que cada implementación concrete el tipo exacto con el que puede operar. Este patrón es habitual cuando se quiere combinar polimorfismo con genericidad segura.

La idea consiste en definir la interfaz como `Punto<T extends Punto<T>>`, indicando que el parámetro genérico representa **el propio subtipo concreto**. De este modo, el método `distanciaA` recibe directamente un parámetro del mismo tipo concreto `T`. El compilador garantiza así que una implementación como `Punto2D` solo pueda calcular distancias respecto a otros `Punto2D`, y lo mismo ocurre con `Punto3D`. Cualquier intento de mezclar tipos distintos se detecta en **tiempo de compilación**, no en ejecución.

Este enfoque elimina por completo la necesidad de `instanceof` y *downcasting*, ya que el tipo correcto está asegurado por el sistema de tipos del lenguaje. Además, el código resultante es más limpio, más seguro y expresa mejor la intención del diseño. Desde el punto de vista conceptual, se trata de un ejemplo avanzado de cómo los **genéricos refuerzan el polimorfismo**, trasladando errores potenciales del tiempo de ejecución al tiempo de compilación.

Como ocurre con cualquier uso de genéricos en Java, tras la compilación se aplica **type erasure**, y el tipo genérico no existe en tiempo de ejecución. No obstante, el refuerzo del chequeo ya se ha realizado previamente, por lo que el diseño sigue siendo seguro aunque internamente el código opere sin información genérica explícita.

***

### Versión con genéricos (sin `instanceof` ni *downcasting*)

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        double dx = x - p.x;
        double dy = y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

```java
public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        double dx = x - p.x;
        double dy = y - p.y;
        double dz = z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
```

Con este diseño, cualquier uso incorrecto como calcular la distancia entre un `Punto2D` y un `Punto3D` queda **prohibido por el compilador**, logrando una solución más robusta y alineada con la programación genérica avanzada en Java.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
El hecho de que `String` sea subtipo de `Object` **no implica** que `List<String>` sea subtipo de `List<Object>`. En Java, los **tipos genéricos son invariantes** respecto a su parámetro de tipo. Esto significa que, aunque exista una relación de herencia entre `String` y `Object`, **no existe esa misma relación entre `List<String>` y `List<Object>`**. Permitirlo sería peligroso, ya que a través de una referencia `List<Object>` se podrían insertar objetos que no son `String`, rompiendo la seguridad de tipos del `List<String>` original.

En cambio, los **arrays en Java sí son covariantes**. Esto quiere decir que `String[]` **sí es subtipo** de `Object[]`. El lenguaje permite esta relación por razones históricas y de compatibilidad, pero introduce un problema potencial en tiempo de ejecución. Si un `String[]` se trata como un `Object[]`, el compilador permite insertar cualquier `Object`, como un `Integer`, lo que provoca un **`ArrayStoreException` en tiempo de ejecución**, ya que el array realmente solo puede almacenar `String`.

La diferencia esencial es que en los arrays el chequeo de tipos se completa parcialmente **en tiempo de ejecución**, mientras que en los genéricos Java prioriza la **seguridad estática en tiempo de compilación**. Por esta razón, los arrays permiten situaciones inseguras que fallan en ejecución, mientras que los genéricos directamente **prohíben esas situaciones en compilación**, evitando errores tardíos. Esta decisión de diseño explica por qué la respuesta es distinta en cada caso.

A partir de estos ejemplos, se puede definir que un tipo genérico es **covariante** si `Contenedor<Subtipo>` es subtipo de `Contenedor<Supertipo>` (como los arrays en Java), **contravariante** si ocurre lo contrario, e **invariante** si no existe relación alguna entre `Contenedor<Subtipo>` y `Contenedor<Supertipo>`. En Java, los tipos genéricos como `List<T>` son **invariantes** por defecto, precisamente para garantizar la seguridad de tipos y evitar errores en tiempo de ejecución como los que sí aparecen con los arrays.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un **wildcard** (`?`) en Java es un marcador que representa **un tipo desconocido** dentro de un tipo genérico. Se utiliza cuando no interesa fijar exactamente el parámetro de tipo, sino **expresar una relación de variancia** (covarianza o contravarianza) de forma controlada. Gracias a los wildcards, Java permite recuperar parte de la flexibilidad que los genéricos invariantes pierden, manteniendo al mismo tiempo la seguridad de tipos en tiempo de compilación.

El wildcard **`? extends T`** expresa **covarianza**: indica que la colección contiene elementos de un **subtipo desconocido de `T`**. Se utiliza cuando la colección se va a **leer**, pero no a modificar, ya que no se sabe el subtipo concreto que contiene. En este caso, es seguro tratar los elementos como `T`, pero **no es seguro insertar nuevos elementos** (salvo `null`). Es el caso típico de métodos que consumen datos, como recorrer una lista de números para calcular una suma.

Por el contrario, el wildcard **`? super T`** expresa **contravarianza**: indica que la colección acepta elementos de tipo `T` o de cualquiera de sus **supertipos**. Se emplea cuando la colección se va a **escribir**, ya que es seguro insertar valores de tipo `T`. Sin embargo, al leer, solo se puede garantizar que los elementos son de tipo `Object`, porque el subtipo concreto es desconocido. Este patrón es habitual en métodos que producen o insertan valores en una colección.

Esta distinción se resume a menudo en la regla **PECS** (*Producer Extends, Consumer Super*): si una estructura **produce** valores, usar `extends`; si **consume** valores, usar `super`. Los wildcards permiten así expresar correctamente la intención del método y reforzar el chequeo de tipos sin recurrir a *casting* ni perder generalidad.

***

### (i) Usando `? extends` para sumar números (lectura)

```java
public static double suma(List<? extends Number> lista) {
    double total = 0.0;
    for (Number n : lista) {
        total += n.doubleValue();
    }
    return total;
}
```

Este método puede trabajar con `List<Integer>`, `List<Double>`, etc., pero no puede añadir elementos a la lista.

***

### (ii) Usando `? super` para añadir enteros (escritura)

```java
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}
```

Este método acepta `List<Integer>`, `List<Number>` o `List<Object>`, y puede insertar enteros con total seguridad.

Los wildcards permiten así **recuperar covarianza y contravarianza de forma explícita y segura**, evitando los problemas en tiempo de ejecución que aparecen con los arrays y manteniendo el control estricto del sistema de tipos de Java.
