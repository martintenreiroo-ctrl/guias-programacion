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
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
En orientación a objetos, la herencia es un mecanismo que permite definir una clase nueva a partir de otra ya existente. La relación que se establece se suele expresar como “A es-un B”, lo que significa que un objeto del subtipo puede considerarse también como un objeto del tipo base. Por ejemplo, si se dice que un Artillero es un Soldado, cualquier Artillero puede tratarse como un Soldado. Esta idea es clave para modelar jerarquías y evitar repetir código.
La primera implicación importante de la herencia es la compatibilidad de tipos. Cuando una clase hereda de otra, los objetos de la subclase son compatibles con el tipo de la superclase. Esto permite, por ejemplo, almacenar objetos de distintos subtipos en una referencia o estructura (como un array) del tipo base. Este comportamiento recuerda al uso de punteros a struct en C, pero en Java es automático y está controlado por el sistema de tipos.
La segunda implicación es la herencia de estado y comportamiento. Los atributos y métodos definidos en la superclase pasan a existir también en las subclases, sin necesidad de volver a escribirlos. De este modo, si Soldado tiene un atributo nombre y un método saludar(), tanto Artillero como Zapador disponen de ellos automáticamente, además de poder añadir sus propios atributos y métodos específicos.
A continuación se muestra un ejemplo sencillo en Java. Se define una clase base Soldado y dos subclases que heredan de ella. En el main se aprovecha la compatibilidad de tipos creando un array de Soldado que contiene objetos de distintos subtipos, y se recorre para que todos saluden.
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
``
public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Luis", 5);
        ejercito[1] = new Zapador("Ana", 3);
        ejercito[2] = new Artillero("Carlos", 2);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Al crear un objeto de una subclase (por ejemplo, un Artillero o un Zapador), se ejecutan dos constructores: primero el de la clase base (Soldado) y después el de la clase derivada concreta. El orden es siempre el mismo: de arriba hacia abajo en la jerarquía de herencia. Esto garantiza que el estado común definido en la superclase se inicialice antes de inicializar el estado específico de la subclase.
La palabra clave super, utilizada dentro de un constructor, sirve para invocar explícitamente un constructor de la clase base. Con ello se indica qué constructor de la superclase debe ejecutarse y con qué parámetros. Conceptualmente, super(...) equivale a decir “inicializa primero la parte del objeto que corresponde a la clase padre”. Si no se escribe super(...), el compilador inserta automáticamente una llamada a super() sin parámetros.
Si la clase base no tiene un constructor sin parámetros visible (por ejemplo, solo define constructores con argumentos), entonces es obligatorio llamar a super(...) de forma explícita desde el constructor de la subclase. En caso contrario, el código no compila, ya que Java no sabría cómo inicializar la parte heredada del objeto. Por tanto, siempre que el constructor por defecto no exista o no sea accesible, debe indicarse explícitamente qué constructor de la superclase se debe ejecutar.
En resumen, en la creación de un objeto con herencia siempre se inicializa primero la superclase, super permite controlar esa inicialización, y su uso es obligatorio cuando no existe un constructor sin parámetros accesible en la clase base. Esto refuerza la idea de que un objeto derivado contiene internamente un objeto de la clase base completamente válido.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
Sí, los atributos privados de la superclase forman parte físicamente de la instancia de la subclase en memoria. Cuando se crea un objeto de una subclase, el objeto completo incluye tanto los atributos definidos en la superclase como los propios de la subclase. Esto se explica porque, conceptualmente, un objeto de la subclase contiene un “subobjeto” de la clase base, que se inicializa mediante el constructor de la superclase antes de ejecutar el constructor de la subclase.
Sin embargo, el hecho de que esos atributos privados existan en memoria no implica que puedan usarse directamente desde el código de la subclase. En Java, la palabra clave private restringe el acceso exclusivamente a la propia clase donde se declara el atributo. Por tanto, una subclase no puede acceder directamente a los atributos privados de su superclase, aunque dichos atributos formen parte del mismo objeto.
Por ejemplo, en la clase Soldado, el atributo nombre es privado. Esto significa que una subclase como Artillero hereda dicho atributo en memoria, pero no puede referirse a él directamente:
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    // Esto NO sería válido:
    // System.out.println(nombre);  // error de compilación
}
## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
Que los objetos de distintas subclases sean compatibles a nivel de tipos implica una ventaja fundamental en términos de extensibilidad del código. Significa que el código que trabaja con el tipo base (Soldado) no necesita conocer los detalles ni los tipos concretos de los objetos que maneja. De este modo, el comportamiento común puede ampliarse añadiendo nuevas subclases sin tocar el código existente, lo que reduce errores y facilita el mantenimiento.
Desde el punto de vista del diseño, esta compatibilidad permite escribir código abierto a extensión pero cerrado a modificación. Una vez definido un bloque de código que opera con Soldado (por ejemplo, un bucle que pide que todos saluden), ese bloque seguirá funcionando correctamente aunque se añadan nuevos tipos de soldados. La única condición es que el nuevo tipo herede de Soldado y, por tanto, sea compatible con él.
Para ilustrarlo, se añade un nuevo tipo de soldado, por ejemplo Medico, que hereda de Soldado y tiene un atributo específico. No es necesario modificar el código que recorre el array de soldados y les pide que saluden, ya que todos siguen siendo tratados como Soldado.
public class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Luis", 5);
        ejercito[1] = new Zapador("Ana", 3);
        ejercito[2] = new Medico("Elena", 4);

        for (Soldado s : ejercito) {
            s.saludar(); // no se modifica este código
        }
    }
}

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
Sí, en Java es posible tener una referencia del supertipo que apunte a un objeto real de un subtipo. Esto es una consecuencia directa de la herencia y de la compatibilidad de tipos: si Artillero hereda de Soldado, entonces un objeto Artillero puede almacenarse en una variable de tipo Soldado. Este mecanismo es la base del polimorfismo y permite tratar de forma uniforme a objetos distintos que comparten una misma superclase.
Sin embargo, usando una referencia del supertipo solo se pueden invocar los métodos que están definidos en el supertipo. Aunque el objeto real sea, por ejemplo, un Artillero, el compilador solo permite llamar a los métodos públicos conocidos por Soldado. Los métodos específicos del subtipo no son accesibles directamente, ya que no todos los Soldado tienen por qué ser Artillero.
El upcasting consiste en tratar un objeto de un subtipo como si fuera del supertipo. Este proceso es automático y seguro, y ocurre, por ejemplo, al guardar un Artillero en una referencia Soldado. El downcasting, en cambio, consiste en convertir una referencia del supertipo en una referencia de un subtipo concreto. Este proceso no es automático y puede provocar errores en tiempo de ejecución si el objeto real no es del tipo esperado.
Para comprobar el tipo real del objeto antes de hacer un downcasting se utiliza el operador instanceof, que indica si un objeto es una instancia de una clase concreta o de alguna de sus subclases. A continuación se muestra un ejemplo recorriendo un array de Soldado y, solo si el objeto real es un Artillero, se accede a su número de cohetes:
for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting seguro
        System.out.println("Cohetes: " + a.getCohetes());
    }
}

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
En orientación a objetos, el acceso protegido forma parte de los mecanismos de ocultación de información y se sitúa entre el acceso private y el acceso public. Un miembro protegido es accesible desde la propia clase donde se declara y desde sus subclases, pero no desde código externo no relacionado por herencia. Esto permite que una clase base exponga cierta información o comportamiento solo a sus descendientes, manteniéndolo oculto para el resto del programa.
En Java, el acceso protegido se implementa mediante la palabra clave protected. A diferencia de private, que impide cualquier acceso desde las subclases, protected facilita que las clases derivadas reutilicen o amplíen parte de la lógica interna de la superclase. Este tipo de acceso es útil cuando se quiere permitir la extensión del comportamiento sin romper la encapsulación hacia el exterior.
Aplicado al ejemplo de Soldado, se puede declarar el atributo nombre como protegido. De este modo, seguirá sin ser accesible desde clases externas, pero podrá ser utilizado directamente dentro de una subclase como Zapador. Esto resulta adecuado si, por ejemplo, el método de poner bombas necesita usar el nombre del soldado para mostrar mensajes específicos.
public class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        System.out.println(nombre + " ha puesto una mina");
    }
}

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En los lenguajes orientados a objetos suele existir la idea de una **clase base común** de la que derivan, directa o indirectamente, todos los objetos del sistema. Esta clase base define el comportamiento mínimo compartido por todos los objetos, como la posibilidad de compararse, representarse como texto o gestionarse en memoria. No obstante, esta característica **no es obligatoria en todos los lenguajes orientados a objetos**, ya que depende del diseño concreto de cada lenguaje.

En algunos lenguajes, especialmente los más cercanos a C o C++, **no existe una clase base universal obligatoria**. En C++ puede definirse una jerarquía de clases sin un ancestro común explícito, y aunque existe la clase `std::object` en algunas bibliotecas, no forma parte del lenguaje en sí. Por tanto, no todos los objetos comparten necesariamente una misma clase raíz, y esta decisión queda en manos del programador.

En **Java**, sí existe una clase base común para todos los objetos: la clase `Object`, que pertenece al paquete `java.lang`. Todas las clases de Java heredan de `Object` de forma explícita o implícita. Si una clase no indica con `extends` que hereda de otra, el compilador asume automáticamente que hereda de `Object`. Gracias a esto, todos los objetos en Java comparten métodos básicos como `toString()`, `equals()` o `getClass()`, lo que facilita el uso uniforme de objetos y refuerza la coherencia del sistema de tipos del lenguaje.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La **herencia múltiple** es un mecanismo de la orientación a objetos mediante el cual una clase puede **heredar directamente de más de una clase base**. Esto implica que la subclase adquiere simultáneamente el estado y el comportamiento de varias superclases. El objetivo es reutilizar código de distintos orígenes, pero este enfoque introduce problemas conceptuales y técnicos, como ambigüedades cuando dos clases base definen el mismo método o atributo (el conocido “problema del diamante”).

La **herencia múltiple no existe en Java para clases**. En Java, una clase solo puede heredar de **una única clase base** usando `extends`. Esta decisión de diseño se tomó para simplificar el modelo de herencia y evitar los conflictos y ambigüedades que pueden aparecer en lenguajes que sí permiten herencia múltiple directa de clases, como C++. De esta forma, Java favorece jerarquías más claras y fáciles de mantener.

No obstante, Java ofrece una alternativa parcial mediante las **interfaces**. Una clase puede implementar varias interfaces, lo que permite heredar múltiples conjuntos de métodos abstractos (y, en versiones modernas, métodos por defecto) sin heredar estado. Esto proporciona parte de la flexibilidad de la herencia múltiple, pero sin sus problemas principales, ya que las interfaces no aportan atributos que generen conflictos.

En resumen, Java **no permite herencia múltiple de clases**, pero sí permite combinar comportamientos mediante interfaces. Esta solución encaja bien con el énfasis de Java en la seguridad, la claridad del diseño y la facilidad de comprensión de las jerarquías de tipos.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
En los lenguajes orientados a objetos, las **excepciones son objetos**, lo que permite definir excepciones personalizadas del mismo modo que cualquier otra clase. Esto resulta útil para representar errores propios del dominio del problema y aportar más información que una excepción genérica. En Java, una excepción personalizada se define creando una clase que herede de `Exception` o de `RuntimeException`.

Una excepción **no controlada** es aquella que hereda de `RuntimeException`. Este tipo de excepciones no obliga a ser declarada ni capturada explícitamente, lo que es adecuado cuando el error representa una situación anómala grave o un fallo de programación. Además, como cualquier objeto, una excepción puede estar **compuesta** con otros objetos, permitiendo guardar información adicional sobre el contexto en el que se produjo el error.

En el siguiente ejemplo se define una excepción personalizada `UsuarioNoEncontradoException`, que es no controlada y que contiene un objeto `Usuario` para indicar qué usuario provocó el problema. Se proporcionan dos constructores: uno básico y otro que permite encadenar la causa subyacente, de forma similar a las excepciones estándar de Java.

```java
public class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public class UsuarioNoEncontradoException extends RuntimeException {
    private final Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

Este diseño permite que, al capturar la excepción, se tenga acceso tanto al mensaje de error como al objeto `Usuario` implicado y, si existe, a la causa original del problema. De este modo, las excepciones se convierten en una herramienta más expresiva y coherente dentro del modelo orientado a objetos.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
En orientación a objetos se desaconseja emplear **herencia únicamente con el objetivo de reutilizar código** porque la herencia no solo reutiliza implementación, sino que **establece una relación fuerte de tipo** entre clases (“A es‑un B”). Al usar herencia, se afirma que la subclase es conceptualmente un caso especial de la superclase, y no solo que aprovecha parte de su código. Si esta relación no es real desde el punto de vista del dominio, el diseño resulta forzado y difícil de entender.

Además, la herencia introduce un **acoplamiento fuerte** entre la subclase y la superclase. La subclase depende de la implementación interna de la superclase, por lo que cambios en esta última pueden afectar de forma inesperada a todas las subclases. Esto reduce la flexibilidad del diseño y dificulta su evolución, especialmente cuando la jerarquía crece o se reutiliza en contextos distintos a los inicialmente previstos.

Frente a esto, la **composición** permite reutilizar código sin imponer una relación de tipo. Mediante composición, una clase **tiene un** objeto de otra clase, en lugar de **ser un** tipo de esa clase. Esto favorece diseños más flexibles, donde los comportamientos se combinan sin herencias innecesarias y pueden cambiarse o sustituirse con menor impacto sobre el resto del sistema.

Por todo ello, se considera una buena práctica **preferir composición frente a herencia** cuando el objetivo principal es reutilizar código. La herencia debe reservarse para los casos en los que exista una relación clara de especialización y compatibilidad de tipos, mientras que la composición resulta más adecuada cuando solo se pretende aprovechar funcionalidad ya implementada.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Se dice que se debe **favorecer la composición frente a la herencia** porque la composición conduce, en general, a diseños **más flexibles y menos acoplados**. Mientras que la herencia establece una relación rígida de tipo (“A es‑un B”), la composición establece una relación más débil (“A tiene‑un B”), lo que permite combinar comportamientos sin imponer una jerarquía fija entre las clases.

La herencia crea una dependencia fuerte entre la superclase y la subclase, de modo que cualquier cambio en la superclase puede afectar a todas sus subclases. Esto dificulta la evolución del código y puede introducir errores difíciles de detectar. En cambio, con composición, los cambios suelen quedar encapsulados dentro de las clases compuestas, reduciendo el impacto sobre el resto del sistema.

Además, la composición facilita la **reutilización y sustitución de comportamientos**. Al delegar responsabilidades en objetos internos, es posible variar la implementación sin modificar la clase principal, lo que resulta especialmente útil cuando los requisitos cambian. En herencia, estas variaciones suelen requerir nuevas subclases o reestructurar la jerarquía existente.

Por estas razones, se recomienda usar la herencia solo cuando exista una relación clara de especialización y compatibilidad de tipos, y preferir la composición cuando el objetivo sea construir objetos complejos a partir de otros más simples. Este enfoque suele producir diseños más mantenibles, extensibles y alineados con los principios de la orientación a objetos.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Cuando se dice que la **herencia rompe la encapsulación**, se hace referencia a que una subclase pasa a depender no solo de la interfaz pública de la superclase, sino también de **detalles internos de su implementación**. Aunque formalmente la encapsulación siga existiendo (gracias a `private`, `protected`, etc.), en la práctica la subclase queda fuertemente acoplada a cómo está diseñada la clase base, y no solo a qué servicios ofrece.

Esto ocurre porque una subclase hereda estado y comportamiento y, para funcionar correctamente, suele asumir cómo se inicializan los atributos, qué métodos llaman a otros métodos internos o qué invariantes mantiene la superclase. Si la implementación de la superclase cambia, incluso sin modificar su interfaz pública, ese cambio puede afectar al comportamiento de las subclases, rompiendo código que aparentemente no debería verse afectado. De este modo, detalles internos “se filtran” hacia las subclases.

Además, el uso de miembros `protected` agrava este efecto. Aunque `protected` es útil, permite que las subclases accedan directamente al estado interno de la superclase, lo que hace que ese estado deje de estar realmente encapsulado. Las subclases pasan a conocer y depender de la representación interna, lo que limita la libertad para modificarla en el futuro.

Por esta razón se afirma que la herencia puede romper la encapsulación: no porque elimine los modificadores de acceso, sino porque **fuerza una dependencia estructural fuerte** entre clases. La composición, en cambio, preserva mejor la encapsulación, ya que las clases solo interactúan a través de interfaces bien definidas y no dependen de la implementación interna de otras clases.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
Se puede modelar la relación entre `Estudiante` y `Trabajador` de distintas formas, ya que ambos comparten datos comunes como el DNI y el nombre. Una primera alternativa consiste en usar **herencia**, creando una superclase que agrupe los datos comunes. En este enfoque se establece una relación “es‑una”: tanto un estudiante como un trabajador **son** una persona, y por tanto heredan su estado común.

En el modelo por herencia, la clase `Persona` contiene los atributos compartidos y las clases `Estudiante` y `Trabajador` heredan de ella. Esto permite reutilizar directamente el estado común, pero introduce una relación fuerte de tipo entre las clases, que solo es adecuada si conceptualmente tiene sentido afirmar esa relación en el dominio del problema.

```java
public class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

```java
public class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```

Una segunda alternativa utiliza **composición**, extrayendo los datos comunes a una clase independiente `DatosPersonales`. En este caso, ni `Estudiante` ni `Trabajador` son una `DatosPersonales`, sino que **tienen** unos datos personales. Este enfoque evita crear una jerarquía de herencia y reduce el acoplamiento entre clases.

En el modelo por composición, las clases reciben una instancia de `DatosPersonales` en su constructor y delegan en ella el acceso a la información común. Este diseño suele ser más flexible y facilita cambios futuros en la estructura de los datos compartidos sin afectar a las clases que los usan.

```java
public class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}
```

```java
public class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}
```

```java
public class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```

Ambos modelos son válidos, pero presentan implicaciones distintas: la herencia enfatiza la compatibilidad de tipos, mientras que la composición prioriza la flexibilidad y un menor acoplamiento entre clases.
