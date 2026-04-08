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

### Respuesta
En C no hay excepciones, así que el error hay que indicarlo de forma explícita para que quien llame a la función raiz lo pueda detectar desde fuera. Hay varias formas de diseñarlo; aquí van dos opciones habituales, con ejemplo de cada una.
Una primera opción es usar el valor de retorno para señalar el error, devolviendo un valor especial cuando el argumento es incorrecto. En cálculos con float o double es muy común devolver NAN (Not a Number) y, opcionalmente, usar errno para dar más información. La función avisa del error, pero no lo imprime: el control queda fuera.
#include <math.h>
#include <errno.h>

double raiz(double x) {
    if (x < 0) {
        errno = EDOM;   // error de dominio
        return NAN;
    }
    return sqrt(x);
}

/* uso */
double r = raiz(-4.0);
if (isnan(r)) {
    perror("Error al calcular la raíz");
}

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una excepción es un mecanismo que permite señalar que ha ocurrido una situación anómala durante la ejecución de un programa (por ejemplo, dividir entre cero, acceder a un fichero inexistente o recibir un valor no válido), interrumpiendo el flujo normal del programa y transfiriendo el control a un bloque preparado para manejar ese error.
El objetivo de usar excepciones al implementar funciones es separar claramente el código “normal” del código de tratamiento de errores, evitando comprobaciones constantes y haciendo las funciones más limpias y legibles. En lugar de devolver códigos de error, la función lanza una excepción cuando no puede cumplir lo que promete.
Cuando un programador llama a una función, las excepciones le permiten decidir dónde y cómo manejar el error, capturándolo en el nivel adecuado del programa. Así se centraliza el tratamiento de errores, se evita propagar comprobaciones innecesarias y se pueden tomar decisiones más coherentes, como reintentar la operación, mostrar un mensaje al usuario o terminar el programa de forma controlada.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
En Java sí existen las excepciones, así que el control del error se hace lanzando una excepción desde el método y capturándola fuera. En el caso de una raíz cuadrada, si el método recibe un número negativo, lo normal es lanzar una excepción indicando que el argumento no es válido, y dejar que el main decida cómo reaccionar.
Un ejemplo sencillo sería definir el método raiz dentro de una clase Calculadora y lanzar una IllegalArgumentException cuando el número sea negativo:


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
Lanzar una excepción significa que una función detecta una situación que no puede o no debe manejar y crea explícitamente una excepción, interrumpiendo la ejecución normal del método. En Java se hace con throw. En el ejemplo de la raíz, cuando el método recibe un número negativo, decide lanzar una IllegalArgumentException porque el argumento no es válido.
Controlar o capturar una excepción es interceptarla mediante un bloque try-catch para decidir qué hacer cuando ocurre el error. Normalmente se hace en un nivel superior del programa, por ejemplo en main, donde se puede informar al usuario, registrar el error o continuar la ejecución de forma controlada.
Cuando una excepción se propaga, significa que si una función la lanza y no la captura, Java abandona esa función inmediatamente y sube al método que la llamó. Si ese método tampoco la captura, sigue subiendo por la pila de llamadas hasta que encuentra un catch adecuado o hasta que llega a main. Durante este proceso, las funciones intermedias se abortan: no se reanudan ni continúan después de lanzar la excepción.
public static double raiz(double x) {
    if (x < 0) {
        throw new IllegalArgumentException("Número negativo");
    }
    return Math.sqrt(x);
}
Si raiz(-4) se llama desde main y no hay try-catch dentro de raiz, la excepción se lanza ahí y se propaga directamente a main. El método raiz termina en ese instante, no devuelve ningún valor y no continúa su ejecución. Si main la captura, el programa sigue desde el catch; si no la captura, el programa finaliza mostrando el error. Esto ilustra cómo las excepciones cortan el flujo normal y permiten manejar errores de forma centralizada.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
La propagación natural de excepciones en lenguajes como Java tiene la ventaja principal de que el error sube automáticamente por la pila de llamadas, sin que cada función tenga que comprobar y devolver códigos de error como ocurre en C. Esto evita repetir constantemente comprobaciones y reduce el riesgo de olvidar manejar un error en alguna función intermedia.
Además, hace el código más claro y limpio, porque las funciones se centran en su tarea principal y el tratamiento del error se deja para un nivel superior, como main. Así, el error se puede manejar en el lugar más adecuado y no se continúa la ejecución como si nada hubiera pasado, algo que en C es fácil que ocurra si se ignora un valor de retorno.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
Sí, en orientación a objetos las excepciones suelen ser objetos. Por ejemplo, en Java todas las excepciones heredan de la clase Exception. Esto significa que una excepción no es solo un error, sino un objeto que contiene información encapsulada sobre lo que ha ocurrido, como un mensaje descriptivo, el tipo de error o incluso la causa original.
La principal ventaja en términos de encapsulación es que toda la información y el comportamiento relacionado con el error queda agrupado en un único objeto. Así, quien captura la excepción no necesita saber cómo se ha detectado el error internamente, solo accede a los datos que la excepción expone mediante métodos como getMessage().
Gracias a esto, sí se pueden crear excepciones personalizadas, adaptadas a los errores específicos de una aplicación. El programador puede definir nuevas clases de excepción que hereden de Exception, con mensajes claros o campos propios, haciendo que los errores sean más expresivos, fáciles de entender y de manejar en programas grandes.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Comparando C con Java, una ventaja clave de la encapsulación es que cualquier objeto excepción lleva información esencial integrada, que resulta muy útil cuando se llega a un manejador. Normalmente incluye el tipo concreto del error, un mensaje descriptivo, y a menudo también el contexto del fallo, como el punto del programa donde ocurrió (traza de la pila o stack trace).
Esto es algo que en C no viene “de serie”: allí el programador debe pasar manualmente códigos de error, mensajes o variables globales como errno, perdiéndose información por el camino. En Java, cuando se captura una excepción, se sabe qué ha fallado, por qué ha fallado y dónde, todo empaquetado en el propio objeto, lo que facilita mucho diagnosticar el problema y manejarlo correctamente.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Sí, en Java se pueden tener varios bloques catch asociados a un mismo bloque try. Cada catch sirve para capturar un tipo distinto de excepción, normalmente de la más específica a la más general.
Cuando ocurre una excepción, solo se ejecuta un único catch, el primero cuyo tipo sea compatible con la excepción lanzada. Una vez se entra en ese catch, los demás se ignoran y el programa continúa después del bloque try-catch. Si ningún catch es compatible, la excepción se propaga al método llamador.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
Aunque una excepción rompa el flujo normal del código, Java ofrece el bloque finally para garantizar que cierto código se ejecute siempre, haya o no excepción. Esto se usa típicamente para cerrar ficheros, liberar memoria o recursos externos, incluso aunque la excepción siga propagándose.
Cuando hay catch, el finally se ejecuta después del try y del catch, pase lo que pase:
try {
    double r = Calculadora.raiz(-4);
    System.out.println(r);
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Liberando recursos");
}

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí, en Java el bloque finally puede ir sin catch. Es totalmente válido usar solo try y finally cuando no quieres manejar la excepción en ese punto, pero sí asegurarte de que cierto código se ejecute antes de que la excepción se propague.
El bloque finally se ejecuta siempre, tanto si ocurre una excepción como si no ocurre ninguna. Da igual que el código del try termine normalmente o que se lance una excepción: el finally se ejecuta antes de salir del bloque.
Incluso si hay un return dentro del try, el finally también se ejecuta. Es decir, antes de devolver el valor y salir del método, Java ejecuta el código del finally. Solo hay casos muy excepcionales (como cerrar la JVM con System.exit) en los que el finally no llega a ejecutarse.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java hay excepciones controladas (checked) y no controladas (unchecked). Las controladas son las que el compilador obliga a capturar o declarar, porque representan errores previsibles. Las no controladas no obligan a gestionarlas y suelen indicar fallos de programación. RuntimeException es clave porque todas sus subclases son no controladas.
Ejemplos típicos de controladas son IOException, FileNotFoundException o SQLException, usadas cuando el error depende del entorno (ficheros, red, base de datos). Se prefieren en situaciones como lectura de ficheros, acceso a recursos externos o operaciones de entrada/salida.
Ejemplos de no controladas son NullPointerException, IllegalArgumentException o IndexOutOfBoundsException, habituales cuando el programa usa mal los datos. Se prefieren cuando hay argumentos inválidos, estados internos incorrectos o errores que no deberían ocurrir si el código está bien hecho.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
En Java, throws se usa en la firma de un método para indicar que ese método puede lanzar una excepción controlada y que no la maneja internamente. Sirve para avisar al compilador y a quien llame al método de que ese error puede ocurrir y deberá ser tratado más arriba en la pila de llamadas.
Es una alternativa a capturar una excepción controlada porque, en lugar de usar un try-catch dentro del propio método, el programador delegar la responsabilidad del manejo del error al método llamador. Esto es útil cuando el método no sabe cómo reaccionar ante el error o cuando se quiere que la decisión se tome en un nivel superior, como en main, manteniendo el código más limpio y flexible.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
Un ejemplo típico sería un método que abre un fichero, pero no quiere capturar la excepción si el fichero no existe, sino dejar que se propague usando throws. Aun así, se usa finally para asegurar el cierre del recurso si llegó a abrirse.
import java.io.*;

public class Ejemplo {

    public static void leerFichero(String nombre) throws FileNotFoundException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(nombre));
            System.out.println(br.readLine());
        } catch (IOException e) {
            // se puede tratar aquí una IO distinta, o dejarla vacía
        } finally {
            try {
                if (br != null) br.close();
            } catch (IOException e) {
                // error al cerrar, no crítico
            }
        }
    }
}

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Sí, se pueden poner excepciones no controladas (RuntimeException y sus subclases) en throws, pero no es obligatorio ni necesario, porque el compilador no lo exige. Es decir, un método puede declararlas, pero no aporta ninguna ventaja técnica.
En ese caso, el método llamador no está obligado a usar try-catch, ya que las excepciones no controladas pueden propagarse sin ser capturadas. Usar try-catch solo tendría sentido si el llamador quiere reaccionar explícitamente ante ese error concreto, por ejemplo para mostrar un mensaje más claro o aplicar una recuperación específica.
El principal sentido de declarar una RuntimeException en throws es documentar el comportamiento del método, dejando claro que puede fallar por ciertos motivos (como argumentos inválidos). No es una obligación del lenguaje, sino una decisión de diseño para hacer el código más explícito y comprensible.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
Se recomienda usar excepciones controladas (como IOException) cuando el error es previsible y recuperable, y depende del entorno: ficheros que no existen, fallos de red, problemas de entrada/salida o acceso a recursos externos. En estos casos tiene sentido obligar al programador a pensar qué hacer, ya sea capturando la excepción o propagándola con throws.
Las excepciones no controladas (como IllegalArgumentException) se usan cuando el error indica un fallo de programación, por ejemplo pasar argumentos inválidos o llegar a un estado interno imposible. No suelen ser recuperables y no tiene sentido forzar al llamador a capturarlas, porque lo correcto es corregir el código que provoca el error.
No todos los lenguajes distinguen entre ambos tipos como Java. En muchos lenguajes modernos (C++, Python, JavaScript) todas las excepciones se comportan como no controladas, es decir, no existe la obligación de declararlas ni capturarlas. De hecho, este modelo es hoy el más habitual, mientras que el sistema de excepciones controladas es una característica particular de Java (y pocos lenguajes más) pensada para hacer explícito el manejo de errores previsibles.
Proporcione sus comentarios sobre BizChat

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Sí, tiene sentido lanzar excepciones dentro de un catch. Se hace cuando el método no puede resolver realmente el problema, pero sí puede traducirlo a una excepción más adecuada para su nivel o su responsabilidad. Es habitual capturar una excepción “técnica” y lanzar otra más “lógica” o de dominio.
Por ejemplo, capturar una IOException y lanzar una excepción más clara para la aplicación:
try {
    leerFichero(nombre);
} catch (IOException e) {
    throw new RuntimeException("No se pudo leer el fichero de configuración", e);
}
Aquí el catch no oculta el error, sino que lo envuelve y añade información más útil para niveles superiores.
También se puede relanzar la misma excepción capturada, simplemente usando throw e;. Esto tiene sentido cuando se quiere hacer alguna acción intermedia (como registrar el error o liberar recursos) pero dejando que el error siga su curso normal.
try {
    double r = Calculadora.raiz(-4);
} catch (IllegalArgumentException e) {
    System.out.println("Log: se recibió un valor incorrecto");
    throw e;  // se relanza la misma excepción
}

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
Que una excepción sea la “causa” de otra significa que una excepción nueva envuelve a otra que ocurrió antes, conservando la información del error original. Esto permite traducir un error de bajo nivel (técnico) en uno de alto nivel (más significativo para la aplicación), sin perder el motivo real del fallo.
Por ejemplo, se puede capturar una IOException (bajo nivel) y encapsularla en una excepción personalizada:
class ErrorAplicacion extends Exception {
    public ErrorAplicacion(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

try {
    new FileReader("datos.txt");
} catch (IOException e) {
    throw new ErrorAplicacion("Error al cargar los datos", e);
}
