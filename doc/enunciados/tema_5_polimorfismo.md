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

### Respuesta
El polimorfismo es una característica de la programación orientada a objetos que permite que una referencia de una clase base (padre) pueda apuntar a objetos de distintas clases derivadas (hijas) y que, al invocar un método, se ejecute la implementación correspondiente al tipo real del objeto. Es decir, la llamada al método es la misma, pero el comportamiento puede variar según el objeto concreto que la recibe.
Este mecanismo permite escribir código más general y flexible, ya que el programador puede trabajar con referencias del tipo padre sin preocuparse del subtipo exacto. A diferencia de C o C++ estructurado, donde habría que usar if, switch o punteros a función para distinguir comportamientos, en POO el lenguaje gestiona esta selección automáticamente en tiempo de ejecución.
El polimorfismo es especialmente útil para extender programas sin modificar código existente, ya que basta con crear nuevas clases hijas que implementen su propio comportamiento. Esto mejora la reutilización del código y facilita el mantenimiento y la evolución de las aplicaciones.
La sobreescritura de métodos consiste en que una clase hija redefine un método heredado de su clase padre, proporcionando una implementación propia. Para que exista sobreescritura, el método debe tener la misma firma (mismo nombre y mismos parámetros) que el método definido en la clase padre.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La ligadura dinámica o enlace tardío consiste en que la decisión de qué implementación concreta de un método se ejecuta no se toma en tiempo de compilación, sino en tiempo de ejecución, en función del tipo real del objeto al que apunta una referencia. Esto contrasta con la ligadura estática (o enlace temprano), típica de C o del C++ no polimórfico, donde el método que se va a ejecutar queda determinado en compilación. La ligadura dinámica es un mecanismo esencial para que el comportamiento de un programa pueda variar según el objeto concreto sin cambiar el código que lo invoca.
La relación con el polimorfismo es directa: el polimorfismo se apoya en la ligadura dinámica para funcionar. Gracias al enlace tardío, cuando se llama a un método a través de una referencia de la clase padre, el lenguaje puede decidir en tiempo de ejecución si debe ejecutarse la versión del padre o la versión sobreescrita en la clase hija. Sin ligadura dinámica, el polimorfismo se limita o directamente no existe, ya que siempre se ejecutaría el método asociado al tipo de la referencia y no al del objeto.
En C++, la ligadura dinámica no es automática: el programador debe indicar explícitamente qué métodos usan enlace tardío mediante la palabra clave virtual. Si un método no es virtual, se aplica ligadura estática aunque exista herencia. En Java, en cambio, la ligadura dinámica es el comportamiento por defecto para los métodos de instancia: todos los métodos no static, no final y no private se enlazan dinámicamente sin que el programador tenga que indicarlo. En Python, el enlace es también dinámico y automático, incluso más flexible que en Java, ya que el lenguaje es dinámicamente tipado y la resolución de métodos se hace siempre en tiempo de ejecución, lo que facilita el polimorfismo sin ningún tipo de anotación especial.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma general.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("El zapador saluda y habla de explosivos.");
    }
}

class Artillero extends Soldado {
    // No sobrescribe el método saludar
}

public class Main {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[2];
        soldados[0] = new Zapador();
        soldados[1] = new Artillero();

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}
Al ejecutar este programa, la llamada a saludar sobre cada elemento del array invoca dinámicamente el método adecuado, demostrando claramente el funcionamiento del polimorfismo en Java.



## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, al sobreescribir un método es posible invocar el método de la clase base para reutilizar su comportamiento y luego añadir o modificar partes. Esto es muy habitual cuando la clase hija no quiere reemplazar totalmente la funcionalidad heredada, sino extenderla. De este modo se evita duplicar código y se mantiene una relación clara entre el comportamiento del padre y el del hijo.
En Java, esto se consigue usando la palabra clave super, que permite acceder explícitamente a los métodos (y atributos) de la clase base. Cuando una clase hija llama a super.metodo(), se ejecuta la versión del método definida en la clase padre, incluso aunque ese método esté sobreescrito en la clase hija. Esto es especialmente útil en escenarios de herencia y polimorfismo.
En este caso, Zapador sobreescribe el método saludar, pero primero invoca el saludo normal del Soldado mediante super.saludar() y después añade su propio mensaje. Así, el comportamiento original no se pierde, sino que se amplía. La palabra clave usada para invocar al método de la clase base es super.
class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda de forma general.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al sobreescribir un método en Java, existen restricciones claras sobre los parámetros y el tipo de retorno. Los parámetros deben ser exactamente los mismos que en el método de la clase base: mismo número, mismo orden y mismos tipos. En cuanto al tipo de retorno, debe ser el mismo o un subtipo del retorno original (esto se llama retorno covariante). No se permite cambiar el tipo de retorno por uno incompatible, ya que rompería el contrato del método heredado y el polimorfismo dejaría de ser seguro.
La sobreescritura (overriding) ocurre entre una clase hija y su clase padre, y permite redefinir el comportamiento de un método heredado, siendo la decisión de qué método ejecutar tomada en tiempo de ejecución (ligadura dinámica). En cambio, la sobrecarga (overloading) consiste en definir varios métodos con el mismo nombre pero distinta lista de parámetros dentro de una misma clase (o entre padre e hijo), y se resuelve en tiempo de compilación. Es decir, la sobreescritura está relacionada con herencia y polimorfismo, mientras que la sobrecarga no lo está.
La anotación @Override se utiliza para indicar explícitamente que un método pretende sobreescribir un método de la clase base. Aunque no es obligatoria, es muy recomendable usarla siempre porque el compilador comprobará que realmente existe un método compatible en la clase padre. Si hay un error (por ejemplo, un parámetro mal escrito), el compilador avisará.
En resumen, @Override no cambia el comportamiento del programa, pero sí mejora la seguridad, claridad y mantenibilidad del código. En proyectos grandes o en trabajos académicos, su uso evita errores difíciles de detectar y deja claro que el método forma parte de un comportamiento polimórfico.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí: en Java se empieza a usar polimorfismo prácticamente desde el principio, aunque al inicio no se sea plenamente consciente de ello. Esto ocurre porque muchas clases heredan directa o indirectamente de Object, y cuando una clase sobreescribe métodos como toString o equals, está modificando el comportamiento que se ejecutará cuando esos métodos se llamen a través de una referencia de tipo Object. Esa elección del método en tiempo de ejecución es ya polimorfismo.
Por ejemplo, cuando haces System.out.println(objeto), Java llama internamente a toString. Si ese objeto pertenece a una clase que ha sobrescrito toString, se ejecuta la versión de esa clase concreta y no la de Object. Aunque el programador no esté creando explícitamente referencias del tipo base ni pensando en jerarquías complejas, el mecanismo polimórfico está funcionando igual gracias a la ligadura dinámica.
Lo mismo ocurre con equals. Muchas colecciones y métodos estándar trabajan con referencias del tipo Object; cuando comparan objetos, llaman a equals, y si la clase lo ha sobrescrito correctamente, se ejecuta su versión específica. Así, al redefinir equals para comparar atributos propios de la clase, se está usando polimorfismo de forma implícita y natural.
En resumen, sí: al sobrescribir toString, equals u otros métodos heredados, ya estás utilizando polimorfismo, aunque todavía no estés diseñando jerarquías complejas como en ejemplos clásicos con clases padre e hijas. Esto demuestra que el polimorfismo en Java no es un concepto “avanzado” aislado, sino una característica fundamental que se usa desde las primeras etapas del aprendizaje del lenguaje.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es una clase que no representa un objeto concreto, sino un concepto general que sirve como base para otras clases. Se utiliza cuando tiene sentido compartir código y estructura común entre varias clases hijas, pero no tiene sentido crear instancias directas de esa clase base. Por eso, no se pueden crear objetos de una clase abstracta; solo se pueden crear objetos de sus subclases concretas.
Un método abstracto es un método que no tiene implementación en la clase base; únicamente declara qué se puede hacer, pero no cómo se hace. Al declarar un método como abstracto, se obliga a que todas las clases hijas no abstractas lo implementen. Esto asegura que todas las subclases tengan un comportamiento propio para ese método, lo que encaja perfectamente con el polimorfismo.
En Java, cuando una clase tiene al menos un método abstracto, la clase completa debe declararse como abstract. Además, las clases hijas deben implementar los métodos abstractos, o de lo contrario también deberán ser abstractas. De esta forma, Java garantiza que cuando se use una referencia polimórfica, el método abstracto tendrá siempre una implementación concreta en tiempo de ejecución.
Aplicado al ejemplo, Soldado se convierte en una clase abstracta porque no tiene sentido definir cómo ataca un soldado genérico. El método atacar se declara como abstracto en Soldado, y cada tipo concreto de soldado (Zapador, Artillero, etc.) define su propia acción al atacar. La palabra clave abstract debe ponerse tanto en la declaración de la clase como en la del método abstracto.
abstract class Soldado {
    public void saludar() {
        System.out.println("El soldado saluda.");
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
``

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave **`final`** en Java se usa para **restringir la herencia o la modificación del comportamiento**. Cuando se aplica a un **método**, indica que ese método **no puede ser sobreescrito** por ninguna clase hija. Cuando se aplica a una **clase**, significa que esa clase **no puede ser heredada**. De esta forma, `final` sirve para fijar definitivamente un diseño y evitar que ciertas partes del código sean modificadas mediante herencia.

En relación con el **polimorfismo**, `final` actúa como una **limitación directa**. El polimorfismo se basa en la posibilidad de sobrescribir métodos para que, mediante ligadura dinámica, se ejecute el comportamiento específico de cada clase hija. Si un método está declarado como `final`, **no puede existir sobreescritura**, y por tanto ese método **no participa en el polimorfismo**. Del mismo modo, una clase `final` no puede tener subclases, así que tampoco puede usarse como clase base en un diseño polimórfico.

El uso de `final` suele responder a razones de **seguridad, diseño o eficiencia**. A veces se quiere garantizar que un método crítico se comporte siempre igual, o que una clase represente un concepto cerrado que no deba extenderse. Esto es habitual en librerías y APIs, donde el diseñador quiere evitar usos incorrectos por parte de otros programadores.

Un ejemplo claro en la **API estándar de Java** es la clase **`String`**, que es `final`. Esto impide crear subclases de `String` y garantiza que su comportamiento es siempre consistente. Otras clases `final` conocidas son `Integer`, `Double` o `System`. En todos estos casos, Java prioriza la seguridad y la estabilidad frente a la extensibilidad mediante polimorfismo.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
En Java, una **interfaz** es una construcción que define un **contrato**: especifica **qué métodos debe tener una clase**, pero no cómo se implementan. Una interfaz contiene principalmente **métodos abstractos** (y constantes), y las clases que la usan se comprometen a proporcionar una implementación concreta de esos métodos. Desde el punto de vista conceptual, sirve para definir capacidades comunes que pueden compartir clases muy distintas entre sí.

Las interfaces se parecen a las **clases abstractas** en que ambas se utilizan para el **polimorfismo** y no pueden instanciarse directamente. Sin embargo, hay diferencias importantes: una clase abstracta puede tener atributos y métodos con implementación, mientras que una interfaz se centra en definir comportamiento sin estado. Además, una clase abstracta representa una relación de *“es un”* más fuerte, mientras que una interfaz suele representar *“sabe hacer”* algo (por ejemplo, “puede volar”, “se puede comparar”).

Una diferencia clave frente a las clases abstractas es que **una clase en Java puede implementar múltiples interfaces**, pero solo puede heredar de una única clase (abstracta o no). Esto permite una forma de **herencia múltiple de comportamiento abstracto**, evitando los problemas que tendría la herencia múltiple de clases. Por eso, las interfaces son una herramienta fundamental para diseñar sistemas flexibles y polimórficos en Java.

***

### Ejemplo sencillo

```java
interface Atacante {
    void atacar();
}

class Soldado implements Atacante {
    @Override
    public void atacar() {
        System.out.println("El soldado ataca.");
    }
}

class Robot implements Atacante {
    @Override
    public void atacar() {
        System.out.println("El robot dispara láser.");
    }
}
```

En este ejemplo, `Soldado` y `Robot` no están relacionados por herencia, pero ambos pueden tratarse polimórficamente como `Atacante`, lo que ilustra el papel fundamental de las interfaces en Java.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
Vamos a plantear este ejemplo usando **polimorfismo, clases abstractas, `instanceof` y *downcasting***, manteniendo el diseño coherente con lo que ya conoces de Java. La idea es que la clase `Punto` represente un concepto general, sin concretar si es 2D o 3D, y que delegue en las subclases el cálculo concreto de la distancia. Por ello, `Punto` será una **clase abstracta** y el método `calcularDistanciaA` también será abstracto.

Las clases `Punto2D` y `Punto3D` implementan ese método, pero antes de realizar el cálculo deben comprobar que el punto recibido es del **mismo subtipo**, ya que no tendría sentido calcular la distancia entre un punto 2D y uno 3D. Para ello se emplea `instanceof` y, una vez comprobado, se realiza un *downcasting* seguro. Este enfoque recuerda al uso de `struct` + comprobaciones manuales en C, pero aquí se encuadra dentro de un diseño orientado a objetos.

Gracias a este diseño polimórfico, es posible crear una clase `Linea` que **trabaje únicamente con referencias de tipo `Punto`**, sin saber si los puntos son 2D o 3D. La clase `Linea` delega el cálculo de la distancia en los propios puntos, lo que hace que el cálculo de la longitud funcione correctamente sin necesidad de condicionales ni conocimiento de las dimensiones. Esto muestra cómo el polimorfismo permite desacoplar el código y hacerlo extensible.

En conjunto, este ejemplo ilustra cómo el uso combinado de **métodos abstractos, ligadura dinámica y comprobaciones de tipo** permite diseñar sistemas flexibles, donde nuevas dimensiones (por ejemplo, puntos 4D) podrían añadirse sin modificar la clase `Linea`.

***

### Ejemplo en Java

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro; // downcasting
            double dx = x - p.x;
            double dy = y - p.y;
            return Math.sqrt(dx * dx + dy * dy);
        }
        throw new IllegalArgumentException("Punto incompatible");
    }
}

class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro; // downcasting
            double dx = x - p.x;
            double dy = y - p.y;
            double dz = z - p.z;
            return Math.sqrt(dx * dx + dy * dy + dz * dz);
        }
        throw new IllegalArgumentException("Punto incompatible");
    }
}

class Linea {
    private Punto p1, p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.calcularDistanciaA(p2);
    }
}
```

Este código permite que `Linea` funcione correctamente con puntos 2D o 3D sin conocer su dimensión, apoyándose exclusivamente en el **polimorfismo**.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La **herencia de interfaces** en Java consiste en que **una interfaz puede extender a otra interfaz**, heredando todos sus métodos. A diferencia de las clases, las interface no heredan implementación (tradicionalmente), sino **contratos**: una interfaz hija amplía el conjunto de métodos que las clases deberán implementar. Esto permite construir jerarquías de capacidades de forma clara y modular, sin imponer una relación de herencia entre clases concretas.

En Java **sí existe herencia múltiple de interfaces**. Una interfaz puede extender **varias interfaces a la vez**, lo que significa que una clase que implemente esa interfaz deberá cumplir todos los contratos heredados. Esto es seguro porque no hay estado ni implementación conflictiva (al contrario de lo que ocurriría con herencia múltiple de clases). Gracias a esto, Java permite combinar comportamientos abstractos sin los problemas clásicos de la herencia múltiple.

Esta característica está muy relacionada con el **polimorfismo**, ya que permite tratar objetos distintos de forma uniforme a través de una interfaz común, incluso cuando sus clases no comparten una jerarquía de herencia. En la práctica, las interfaces se usan mucho para definir APIs, desacoplar componentes y permitir que clases muy diferentes puedan usarse indistintamente si cumplen el mismo contrato.

En el ejemplo, `Fichero` define la capacidad básica de leer contenido, y `FicheroEscribible` **extiende** esa interfaz para añadir funcionalidades adicionales como escribir y eliminar. Una clase que implemente `FicheroEscribible` estará obligada a implementar **todos los métodos**, tanto los heredados como los nuevos.

***

### Ejemplo en Java

```java
interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

class FicheroTexto implements FicheroEscribible {
    @Override
    public String leerContenido() {
        return "Contenido del fichero";
    }

    @Override
    public void escribirContenido(String contenido) {
        System.out.println("Escribiendo: " + contenido);
    }

    @Override
    public void eliminar() {
        System.out.println("Fichero eliminado");
    }
}
```

Este diseño permite trabajar polimórficamente con referencias de tipo `Fichero` o `FicheroEscribible`, sin depender de la clase concreta que implementa el comportamiento.
