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

### Respuesta
La encapsulación en Programación Orientada a Objetos consiste en agrupar dentro de una clase tanto los datos (atributos) como las operaciones que trabajan sobre esos datos (métodos). Su objetivo principal es que cada objeto controle su propio estado y defina una interfaz clara para interactuar con él. Gracias a esto, el programador puede trabajar con objetos como unidades independientes, lo que mejora la organización del código, la modularidad y la capacidad de reutilización.
Por su parte, la ocultación de información busca impedir que el código externo acceda directamente a los detalles internos del objeto. Esto se logra usando modificadores de acceso como private, de manera que solo los métodos públicos controlen cómo se leen o modifican los datos. Entre sus ventajas se encuentran la protección del estado interno del objeto, la reducción de errores por uso indebido, la posibilidad de cambiar la implementación sin afectar a otros módulos y una mayor claridad en la interfaz pública de la clase. En resumen, ocultar información contribuye a software más seguro, mantenible y estable.




## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La interfaz pública de un objeto o clase en POO es el conjunto de métodos y atributos que se declaran como public y que pueden ser utilizados desde fuera de la clase. Representa la “puerta de entrada” a un objeto: todo aquello que otros módulos, funciones o clases pueden ver y usar para interactuar con él. Normalmente incluye los métodos necesarios para realizar operaciones importantes (como getters, setters o funciones específicas), dejando ocultos los detalles internos que no deben ser manipulados directamente.
Esta interfaz pública está directamente relacionada con la ocultación de información, ya que sirve como filtro que controla qué partes de la clase son visibles y cuáles permanecen privadas. Gracias a este mecanismo, la clase puede mantener sus datos internos protegidos y solo exponer lo estrictamente necesario, evitando accesos indebidos y permitiendo modificar la implementación interna sin afectar al código externo que usa la clase. En resumen, la interfaz pública define lo que se muestra, y la ocultación define lo que se protege.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La interfaz pública de una clase debe diseñarse con cuidado porque es la parte del código que otros módulos, funciones u objetos utilizarán para interactuar con ella. Todo lo que se haga público queda “grabado” como una forma oficial de uso, y cuanto más simple, coherente y bien pensada sea esa interfaz, más fácil será utilizar la clase sin errores y mantener el programa ordenado. Una mala interfaz pública puede obligar a los usuarios de la clase a entender detalles internos que no deberían conocer, o puede permitir usos incorrectos que dañen el estado del objeto.
Además, cambiar la interfaz pública no es fácil, porque cualquier modificación afecta a todo el código que dependa de ella. Si eliminas, renombras o cambias el comportamiento de un método público, tendrás que revisar y ajustar todas las partes del programa que lo estaban usando. Por eso es importante exponer solo lo necesario y mantener estables esos elementos públicos, evitando así modificaciones costosas y errores en cascada en el resto del software.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase son condiciones que deben cumplirse siempre que un objeto esté en un estado “estable”, es decir, antes y después de ejecutar cualquiera de sus métodos públicos. Representan reglas que describen cómo debe ser un objeto válido: por ejemplo, que un saldo no sea negativo, que una fecha sea correcta o que un contador nunca sea inferior a cero. Estas invariantes ayudan a asegurar que la clase se comporta de manera coherente y que sus objetos mantienen siempre un estado lógico y válido.
La ocultación de información facilita mantener estas invariantes porque impide que el estado interno de la clase pueda ser modificado directamente desde fuera. Si los atributos son privados y solo se puede acceder a ellos mediante métodos controlados, la propia clase puede garantizar que cualquier cambio respete sus reglas internas. De esta manera se evita que código externo rompa accidentalmente la coherencia del objeto, y se asegura que solo los métodos definidos por la clase pueden alterar su estado de forma segura.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
    public class Punto {
    // Atributos ocultos (encapsulados)
    private double x;
    private double y;

    // Constructor: inicializa el estado del objeto
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Getters (acceso controlado a los datos)
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    // Setters (modificación controlada, aquí no imponemos restricciones,
    // pero podríamos validar si hubiera invariantes)
    public void setX(double x) {
        this.x = x;
    }

    public void setY(double y) {
        this.y = y;
    }

    // Método público que usa el estado interno para calcular la distancia al origen (0,0)
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores public y private se pueden aplicar tanto a clases, como a atributos y métodos dentro de una clase. Su función es controlar el nivel de acceso que tendrán otras partes del programa. Por ejemplo, una clase puede declararse public para que cualquier paquete pueda usarla, y dentro de ella podemos marcar métodos o variables como public si queremos que formen parte de la interfaz pública y estén disponibles para otros objetos.
Por otro lado, el modificador private solo puede aplicarse a miembros internos de una clase (atributos, métodos y constructores), pero no a clases de nivel superior. Se utiliza para ocultar detalles internos y evitar que sean accesibles desde fuera de la clase. Esto facilita la encapsulación y protege el estado interno del objeto, permitiendo que solo los métodos públicos actúen sobre esos datos de manera controlada.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
En POO existen más tipos de visibilidad además de la pública y privada. Aunque el concepto general varía según el lenguaje, muchos incorporan niveles intermedios que permiten un control más fino sobre qué partes del código pueden acceder a los atributos y métodos de una clase. La idea es ofrecer varios grados de exposición para equilibrar encapsulación y flexibilidad.
En Java, además de public y private, existen dos visibilidades adicionales: protected, que permite el acceso desde la propia clase, desde clases del mismo paquete y desde subclases aunque estén en otro paquete; y la visibilidad por defecto o package-private (sin poner modificador), que permite el acceso solo dentro del mismo paquete. En otros lenguajes también existen niveles similares: por ejemplo, C++ tiene public, private y protected, y permite aplicarlos también a herencia; C# añade aún más opciones como internal y protected internal. Cada lenguaje adapta estos niveles a su propio modelo de organización, pero la idea común es la misma: controlar con precisión qué partes del código pueden acceder a los elementos de una clase.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
En Java, los miembros de instancia privados están ocultos para otras clases (opción a), pero no para otras instancias de la misma clase. La restricción de private se aplica a nivel de clase, no de objeto: cualquier método definido dentro de la clase puede acceder a los atributos privados de cualquier instancia de esa misma clase (incluida la instancia recibida como parámetro). Por eso, desde un método de Punto podemos leer otro.x aunque x sea private, siempre que el acceso ocurra dentro de la clase Punto.
Esto no contradice la ocultación de información: lo que se oculta es el estado interno frente al código externo (otras clases), no frente a la propia clase. Así, se preserva la capacidad de implementar lógica entre objetos del mismo tipo (por ejemplo, comparar dos Punto) sin romper encapsulación.

public class Punto {
    // Atributos privados (ocultación de información)
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Nuevo método: distancia entre "este" punto y "otro" punto.
    // Observa que accedemos a "otro.x" y "otro.y" aunque sean privados:
    // esto es válido porque el acceso ocurre dentro de la misma clase Punto.
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x; // acceso permitido (misma clase)
        double dy = this.y - otro.y; // acceso permitido (misma clase)
        return Math.sqrt(dx * dx + dy * dy);
    }

    // (Opcional) Getters/Setters públicos según tu diseño de interfaz.
    public double getX() { return x; }
    public double getY() { return y; }
    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }
}

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos getter y setter son funciones que permiten leer y modificar los atributos privados de un objeto respetando la encapsulación.


Un getter devuelve el valor de un atributo.
Ejemplo: getX() devuelve la coordenada x.


Un setter modifica el valor de un atributo.
Ejemplo: setX(double x) asigna un nuevo valor a x.


Se usan porque los atributos suelen ser private, y así la clase controla cómo se accede y cómo se cambian sus datos, pudiendo validar valores o mantener invariantes si hace falta.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
No, no se refiere a que el programa sea más difícil de hackear en el sentido de seguridad informática o ciberataques.

Cuando decimos que la ocultación de información mejora la “seguridad”, hablamos de seguridad interna del software, es decir:

que el estado de los objetos no pueda romperse accidentalmente,
que no se puedan modificar atributos de forma incorrecta,
que se respeten las invariantes de clase,
que el programa sea más estable y resistente a errores.

En otras palabras: seguridad = protección frente a errores de programación, no frente a hackers.
La idea es que, si los datos están privados, solo la propia clase controla cómo se cambian, y así se evitan estados incoherentes o fallos difíciles de detectar.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un miembro de instancia es un atributo o método cuyo valor pertenece a cada objeto individual. Es decir, cada instancia de la clase tiene su propia copia. Por ejemplo, en una clase Punto, cada objeto tiene su propio x e y.
Un miembro de clase (también llamado estático, usando static en Java) pertenece a la clase en sí, no a cada objeto. Solo existe una copia compartida por todas las instancias. Por ejemplo, un contador de cuántos objetos se han creado.
Sí, los miembros de clase también se pueden ocultar usando private, igual que los miembros de instancia. Un static private solo será accesible desde dentro de la propia clase, lo que permite ocultar datos o métodos estáticos que no deben ser utilizados desde fuera.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Sí, tiene sentido que los constructores sean privados, aunque solo en situaciones concretas.

Un constructor privado impide que otros creen objetos libremente desde fuera de la clase. Esto se usa cuando la propia clase debe controlar cómo y cuándo se crean sus instancias. Un caso típico es el patrón Singleton, donde solo puede existir un único objeto y por eso el constructor se hace private. También se usa cuando queremos obligar a los usuarios de la clase a crear objetos mediante métodos estáticos de fábrica (como crear(), from(...), etc.), porque así la clase puede validar, limitar o modificar la creación de instancias.
En resumen: sí tiene sentido, pero solo cuando quieres controlar por completo el proceso de creación de objetos.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
Los miembros de clase se declaran con la palabra clave static.

Atributos estáticos (static) → pertenecen a la clase (una sola copia compartida por todas las instancias).
Métodos estáticos (static) → se invocan sobre la clase (no necesitan un objeto).

public class Punto {
    // Atributos de instancia (ocultos)
    private double x;
    private double y;

    // Miembros de clase (estáticos): máximos globales de x e y
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    // Getters y setters (interfaz pública de instancia)
    public double getX() { return x; }
    public double getY() { return y; }

    public void setX(double x) {
        this.x = x;
        actualizarMaximos(x, this.y);
    }

    public void setY(double y) {
        this.y = y;
        actualizarMaximos(this.x, y);
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x; // permitido: misma clase
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // --- Interfaz pública de clase (estática) ---
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // (Opcional) Para pruebas o reinicios controlados
    public static void resetMaximos() {
        maxX = Double.NEGATIVE_INFINITY;
        maxY = Double.NEGATIVE_INFINITY;
    }

    // --- Lógica interna común para mantener invariantes de clase ---
    // Nota: método estático porque opera sobre miembros estáticos.
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}

Es static porque es un método de clase (factoría).
Recibe dos double, los redondea al entero más cercano con Math.round(...), y devuelve un nuevo Punto.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
public class Punto {
    // Representación interna cambiada: array de 2 posiciones [x, y]
    private final double[] coords = new double[2];

    // --- Interfaz pública sin cambios ---

    // Constructor público
    public Punto(double x, double y) {
        coords[0] = x; // x
        coords[1] = y; // y
    }

    // Getters públicos
    public double getX() { return coords[0]; }

    public double getY() { return coords[1]; }

    // Setters públicos
    public void setX(double x) { coords[0] = x; }

    public void setY(double y) { coords[1] = y; }

    // Método público: distancia al origen (0,0)
    public double calcularDistanciaAOrigen() {
        double x = coords[0];
        double y = coords[1];
        return Math.sqrt(x * x + y * y);
    }

    // Método público: distancia a otro punto
    public double calcularDistanciaAPunto(Punto otro) {
        // Acceso permitido: estamos dentro de la misma clase Punto
        double dx = this.coords[0] - otro.coords[0];
        double dy = this.coords[1] - otro.coords[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    // (Opcional) Método factoría de la pregunta 14: interfaz pública ya existente
    public static Punto crearRedondeado(double x, double y) {
        return new Punto(Math.round(x), Math.round(y));
    }
}


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
Aunque un atributo tenga un getter y un setter públicos, no es buena idea declararlo público. Si un atributo fuera público, cualquier parte del programa podría cambiarlo libremente, sin pasar por los métodos de acceso. En cambio, con atributos privados la clase mantiene el control: puede validar valores en el setter, mantener invariantes, registrar cambios, o modificar la representación interna sin afectar a quien usa la clase.
La convención más habitual en POO (y especialmente en Java) es que todos los atributos sean privados, incluso cuando tengan getters y setters públicos. Esto forma parte de la encapsulación y es una práctica estándar en prácticamente todas las guías de estilo.
Sí, esto tiene relación con las invariantes de clase: si el atributo fuera público, cualquier código externo podría poner al objeto en un estado inválido rompiendo esas invariantes. En cambio, con los atributos privados, la clase garantiza que cualquier modificación pasa por los setters (o por su propia lógica interna), lo que permite mantener siempre un estado coherente.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase es inmutable cuando, una vez creado un objeto, su estado no puede cambiar. Es decir, todos sus atributos se fijan en el constructor y después no existe ningún método que los modifique. Esto implica que no hay setters y que todos los atributos deben ser privados y, normalmente, final. Los objetos inmutables son como “valores” en lugar de contenedores modificables.
Un método modificador es cualquier método que cambia el estado interno del objeto. Un setter es un tipo de método modificador, pero no todos los métodos modificadores son setters: por ejemplo, un método mover(int dx, int dy) podría modificar varias coordenadas internas aunque no se llame “set”.
Las clases inmutables tienen varias ventajas: son más seguras, porque no se pueden romper sus invariantes; son más fáciles de razonar, ya que su estado no cambia; evitan errores por interferencias entre métodos; y facilitan la programación concurrente, porque no hay riesgo de que dos hilos modifiquen el mismo objeto. Por eso muchos lenguajes modernos fomentan el uso de objetos inmutables cuando es posible.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No, no es recomendable incluir setters siempre ni por convención. Incluir un setter implica que el atributo puede cambiarse libremente desde fuera, lo que muchas veces no es deseable. Los setters solo deben existir cuando realmente tenga sentido que el estado del objeto sea modificable.
La buena práctica es justo la contraria:
por defecto, los atributos deben ser privados y sin setter, salvo que el diseño lo requiera claramente. Esto ayuda a mantener invariantes de clase, evita estados incoherentes y da mayor seguridad y control sobre el objeto.
Una clase con atributos privados pero sin setters puede ser más segura, más fácil de mantener e incluso inmutable, lo cual suele ser una ventaja.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase String en Java es inmutable. Esto significa que, una vez creada una cadena, no puede cambiar. Cada vez que parece que modificamos una cadena —por ejemplo, al concatenar— en realidad se crea un nuevo objeto String y se deja el antiguo sin modificar. Por eso, operaciones como s = s + "hola"; generan nuevas cadenas en memoria.
Al concatenar dos cadenas, Java crea un nuevo String que contiene el resultado de la concatenación. Si lo haces muchas veces dentro de un bucle, se crean montones de objetos intermedios, lo que es ineficiente.
Si vamos a construir una cadena paso a paso con muchas concatenaciones (por ejemplo, dentro de un for), lo recomendable es usar StringBuilder (o StringBuffer si se necesita sincronización). Estas clases sí son mutables, por lo que permiten añadir texto sin crear objetos nuevos constantemente. Al final se convierte a String con toString().
Resumen rápido:

String → inmutable.
Concatenación → crea nuevos objetos, es lenta en bucles.
Para muchas concatenaciones → usar StringBuilder.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO, los objetos pueden compararse por identidad (si son exactamente el mismo objeto en memoria) o por contenido (si sus atributos son iguales). En Java, la identidad se compara con el operador ==, que solo devuelve true si ambas referencias apuntan al mismo objeto. Para comparar por contenido, Java utiliza el método equals.
El método equals es un método heredado de la clase Object y sirve para comparar objetos por su contenido lógico, siempre que la clase lo haya redefinido. Por defecto, equals en la clase Object solo compara identidades, es decir, hace lo mismo que ==. Por eso, si queremos que dos objetos de una clase se consideren “iguales” por sus atributos, debemos sobrescribir equals.
En Java, las cadenas (String) son objetos, por lo que no deben compararse con ==, ya que == solo compara referencias. Para comparar el contenido de dos cadenas se debe usar equals, por ejemplo: "hola".equals(otraCadena).
Resumen rápido:

== → compara identidad (misma referencia).
equals → compara contenido lógico, si está bien redefinido.
equals por defecto → compara identidad, igual que ==.
Para comparar String → usar equals, nunca ==.



## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Las clases wrapper (o clases envoltorio) son clases que representan en forma de objeto a los tipos primitivos de un lenguaje orientado a objetos. En Java, cada tipo primitivo tiene su clase wrapper correspondiente: int → Integer, double → Double, boolean → Boolean, etc. Sirven para poder tratar valores primitivos como objetos, lo cual es necesario cuando queremos usarlos en colecciones genéricas (como ArrayList), pasarlos donde se espera un objeto, o utilizar métodos adicionales que los primitivos no tienen.
En Java, transformar un primitivo en su wrapper se hace con autoboxing, y la operación inversa se llama unboxing. Ambos procesos son automáticos. Ejemplo: Integer n = 5; convierte automáticamente el int 5 en un Integer. Del mismo modo, si tenemos Integer x = 7; y hacemos int y = x;, Java convierte x en un primitivo sin que tengamos que hacer nada especial.
Las clases wrapper ofrecen varias ventajas: permiten almacenar valores en estructuras que solo admiten objetos, dan métodos útiles (como Integer.parseInt), y hacen posible trabajar con valores nulos, cosa que un primitivo no puede hacer. Además, ayudan a unificar el comportamiento cuando queremos que un valor tenga características de objeto.
No todos los lenguajes orientados a objetos tienen tipos primitivos. Lenguajes como Python, Ruby o Smalltalk no los necesitan, porque todo es un objeto desde el principio. En esos lenguajes no existe la distinción entre tipos primitivos y tipos objeto, así que no requieren wrappers. Java y C#, en cambio, sí tienen primitivos y por eso necesitan estas clases envoltorio.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un tipo de dato enumerado en POO es un tipo especial que define un conjunto limitado y fijo de valores posibles (como colores, direcciones o estados), y en Java un enum es realmente una clase especial cuyos valores son instancias únicas y controladas; esto mejora la encapsulación porque no se pueden crear valores nuevos fuera de los definidos, se oculta completamente la representación interna y la clase controla sus propios objetos, garantizando invariantes y evitando errores al usar valores no permitidos.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta
public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),   // Sin contemplar bisiestos
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    // Atributos privados encapsulados
    private final int dias;
    private final int ordinal;

    // Constructor privado (propio de los enum)
    Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    // Métodos públicos (interfaz del enum)
    public int getDias() {
        return dias;
    }

    public int getOrdinalEnAno() {
        return ordinal;
    }

    // (Opcional) Si quisieras contemplar bisiestos:
    // public int getDias(int anio) {
    //     if (this == FEBRERO && esBisiesto(anio)) return 29;
    //     return dias;
    // }
    //
    // private static boolean esBisiesto(int anio) {
    //     return (anio % 400 == 0) || (anio % 4 == 0 && anio % 100 != 0);
    // }
}

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
// Dentro del enum Mes (añadir debajo de los getters existentes):

public boolean esDePrimavera(boolean esHemisferioNorte) {
    int m = this.ordinal; // 1..12 tal como definiste en el ejercicio 23
    return perteneceAEstacion(m, esHemisferioNorte ? 3 : 9,
                                 esHemisferioNorte ? 5 : 11);
}

public boolean esDeVerano(boolean esHemisferioNorte) {
    int m = this.ordinal;
    return perteneceAEstacion(m, esHemisferioNorte ? 6 : 12,
                                 esHemisferioNorte ? 8 : 2);
}

public boolean esDeOtoño(boolean esHemisferioNorte) {
    int m = this.ordinal;
    return perteneceAEstacion(m, esHemisferioNorte ? 9 : 3,
                                 esHemisferioNorte ? 11 : 5);
}

public boolean esDeInvierno(boolean esHemisferioNorte) {
    int m = this.ordinal;
    return perteneceAEstacion(m, esHemisferioNorte ? 12 : 6,
                                 esHemisferioNorte ? 2  : 8);
}

// --- Ayudante privado ---
// Devuelve true si el mes m (1..12) está entre inicio..fin (inclusive).
// Soporta rangos que "cruzan" de diciembre (12) a enero (1).
private static boolean perteneceAEstacion(int m, int inicio, int fin) {
    if (inicio <= fin) {
        return m >= inicio && m <= fin;
    } else {
        // Rango envolvente (ej.: dic..feb → 12..2)
        return m >= inicio || m <= fin;
    }
}