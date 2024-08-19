# Temas Avanzados en Java 8 (Programación Funcional)

---
## Indice

1. [Expresiones Lambda](#1)
2. [Interfaces Funcionales](#2)
3. [Stream API](#3)
4. [Optional](#4)
5. [Nuevas API de Date y Time](#5)
6. [Default Methods en Interfaces](#6)
7. [Static Methods en Interfaces](#7)
8. [Nueva API de Concurrencia](#8)
9. [Mapeo de Colecciones y Streams](#9)
10. [Collectors y Reduction](#10)
11. [Nashorn JavaScript Engine](#11)
12. [Type Annotations](#12)
13. [Métodos de Fábrica de Colecciones](#13)
14. [Metodología Funcional y Conceptos Avanzados](#14)

----

## 1. Expresiones Lambda
---

#### Introducción a las expresiones lambda <a name="1"></a>

Las expresiones lambda en Java permiten escribir funciones de manera concisa sin tener que crear clases anónimas. Son una forma de implementar interfaces funcionales (interfaces con un solo método abstracto) utilizando una sintaxis más compacta.

#### Sintaxis y uso de lambdas

La sintaxis general de una expresión lambda es:

```
    (parametros) -> expresión
```

- Parametros: Lista de parámetros para el método de la interfaz funcional. Puede ser vacía, tener uno o varios parámetros.
- -> : Operador lambda que separa los parámetros de la expresión o el bloque de código.
- Expresión: Cuerpo de la lambda. Si es una única expresión, el valor de esta expresión se retorna automáticamente. Si es un bloque de código, se usa return para devolver el valor.

###### Ejemplo:

Implementa la interfaz funcional **Function<T, R>** que toma un parámetro y devuelve un resultado:

```
    Function<String, Integer> longitud = s -> s.length();
    System.out.println(longitud.apply("Hola"));
```

#### Contextos donde se pueden usar lambdas (como interfaces funcionales)

Las expresiones lambda en Java se utilizan principalmente con interfaces funcionales, que son interfaces que tienen exactamente un método abstracto. Algunos contextos comunes donde se pueden usar lambdas:

- **Interfaces Funcionales en la API de Java:**
    Las interfaces funcionales son ideales para utilizar expresiones lambda.
    ```
        Runnable tarea = () -> System.out.println("Tarea en ejecución");
        new Thread(tarea).start();
    ```
- **Streams API:**
    Las lambdas son útiles para operaciones sobre secuencias de datos usando Streams.
    `map`: Transforma cada elemento en un Stream.
    ```
        List<String> nombres = Arrays.asList("Ana", "Juan", "Pedro");
        List<Integer> longitudes = nombres.stream()
                                        .map(s -> s.length())
                                        .collect(Collectors.toList());
        System.out.println(longitudes);
    ```
- **Eventos en Interfaces Gráficas (Swing y JavaFX):**
    Las lambdas son útiles para manejar eventos en interfaces gráficas.

    `En JavaFX:`
    ```
        Button boton = new Button("Haz clic");
        boton.setOnAction(event -> System.out.println("Botón clickeado"));
    ```

    `En Swing:`
    ```
        JButton boton = new JButton("Haz clic");
        boton.addActionListener(e -> System.out.println("Botón clickeado"));
    ```
- **Uso en Métodos de Ordenación y Comparación**
    Las lambdas se pueden usar para definir criterios de ordenación o comparación.
    `Comparator<T>`: Define el orden de los elementos.
    ```
        List<String> nombres = Arrays.asList("Ana", "Juan", "Pedro");
        nombres.sort((s1, s2) -> s1.compareTo(s2));
        System.out.println(nombres);  // Imprime [Ana, Juan, Pedro]
    ```
- **Callbacks y Manejo de Asincronía:**
    Las lambdas son útiles para manejar callbacks o métodos asíncronos.
    ```
        CompletableFuture.supplyAsync(() -> "Hola")
                 .thenAccept(result -> System.out.println(result));
    ```

las expresiones lambda pueden utilizarse en una amplia gama de contextos donde se necesitan implementaciones concisas de interfaces funcionales. Esto incluye la manipulación de datos, el manejo de eventos, la programación concurrente, y más.

###  2. Interfaces Funcionales <a name="2"></a>
---

#### Definición y uso de interfaces funcionales.

Una interfaz funcional en Java es una interfaz que tiene exactamente un método abstracto. Las interfaces funcionales están diseñadas para ser implementadas usando expresiones lambda o referencias a métodos, lo que facilita la escritura de código más conciso y expresivo. Las interfaces funcionales pueden tener métodos adicionales, como métodos predeterminados (default) y estáticos, pero solo un método abstracto.

Las interfaces funcionales son útiles en diversas situaciones, especialmente cuando se combinan con expresiones lambda o referencias a métodos.

Algunos contextos comunes:

- En la API de Streams.
- En el Manejo de Eventos.
- Para Tareas Asíncronas.
- En Métodos de Ordenación y Comparación.
- Para Implementaciones Simples.

**Ventajas de las Interfaces Funcionales**

- Concisión: Facilitan la escritura de código más compacto y legible.
- Expresividad: Permiten expresar comportamientos de manera más clara.
- Flexibilidad: Se pueden utilizar con expresiones lambda y referencias a métodos, aumentando la flexibilidad y reutilización del código.
  
Las interfaces funcionales, junto con las expresiones lambda, proporcionan un poderoso conjunto de herramientas para escribir código en Java de manera más funcional y elegante.

#### Principales interfaces funcionales en java.util.function.

El paquete java.util.function en Java contiene una serie de interfaces funcionales que se utilizan para representar funciones y operaciones que pueden ser aplicadas a datos. Estas interfaces son fundamentales para trabajar con expresiones lambda y la API de Streams en Java 8 y versiones posteriores.

Las principales interfaces funcionales de este paquete:

###### 1. `Consumer<T>`: Método Abstracto: void accept(T t):

Descripción: Acepta un argumento y no devuelve ningún resultado. Generalmente se usa para realizar operaciones sobre los elementos sin necesidad de devolver un valor.

```
    Consumer<String> imprimir = s -> System.out.println(s);
    imprimir.accept("Hola");  // Imprime "Hola"
```
###### 2. `Function<T, R>`: Método Abstracto: R apply(T t)

Descripción: Toma un argumento y devuelve un resultado. Es útil para transformar datos de un tipo a otro.

```
    Function<String, Integer> longitud = s -> s.length();
    System.out.println(longitud.apply("Java"));  // Imprime 4
```

###### 3. Predicate<T>: Método Abstracto: boolean test(T t):

Descripción: Toma un argumento y devuelve un valor booleano. Se utiliza para evaluar condiciones o filtros.

```
    Predicate<String> esLargo = s -> s.length() > 5;
    System.out.println(esLargo.test("JavaScript"));  // Imprime true
```

###### 4. Supplier<T>: Método Abstracto: T get():

Descripción: No toma argumentos y devuelve un resultado. Es útil para proporcionar valores o resultados de forma perezosa.

```
    Supplier<Double> numeroAleatorio = () -> Math.random();
    System.out.println(numeroAleatorio.get());  // Imprime un número aleatorio
```

###### 5. UnaryOperator<T>: Método Abstracto: T apply(T t):

Descripción: Es una especialización de Function que toma un solo argumento y devuelve un resultado del mismo tipo.

```
    UnaryOperator<String> mayusculas = s -> s.toUpperCase();
    System.out.println(mayusculas.apply("java"));  // Imprime "JAVA"
```

###### 6. BinaryOperator<T>: Método Abstracto: T apply(T t1, T t2):

Descripción: Es una especialización de BiFunction que toma dos argumentos del mismo tipo y devuelve un resultado del mismo tipo.

```
    BinaryOperator<Integer> suma = (a, b) -> a + b;
    System.out.println(suma.apply(5, 3));  // Imprime 8
```

###### 7. BiFunction<T, U, R>: Método Abstracto: R apply(T t, U u):

Descripción: Toma dos argumentos y devuelve un resultado. Es útil para operaciones que requieren dos entradas.

```
    BiFunction<Integer, Integer, Integer> multiplicacion = (a, b) -> a * b;
    System.out.println(multiplicacion.apply(4, 5));  // Imprime 20
```

###### 8. BiPredicate<T, U>: Método Abstracto: boolean test(T t, U u):

Descripción: Toma dos argumentos y devuelve un valor booleano. Es útil para evaluaciones que requieren dos entradas.

```
BiPredicate<String, Integer> tieneLongitud = (s, len) -> s.length() > len;
System.out.println(tieneLongitud.test("Java", 3));  // Imprime true
```

###### 9. BiConsumer<T, U>: Método Abstracto: void accept(T t, U u):

Descripción: Toma dos argumentos y no devuelve ningún resultado. Se usa para operaciones que requieren dos entradas.

```
    BiConsumer<String, Integer> imprimirConLongitud = (s, len) -> System.out.println(s + " tiene longitud " + len);
    imprimirConLongitud.accept("Java", 4);  // Imprime "Java tiene longitud 4"
```

#### Uso de la anotación @FunctionalInterface.

La anotación @FunctionalInterface en Java se utiliza para marcar una interfaz como una interfaz funcional.

###### Propósito de @FunctionalInterface
- **Garantizar la Intención:** Sirve para indicar explícitamente que una interfaz está destinada a ser una interfaz funcional, lo cual es útil para la programación funcional y el uso con expresiones lambda.
- **Validación de Compilación:** Permite al compilador verificar que la interfaz cumple con la definición de una interfaz funcional. Si la interfaz tiene más de un método abstracto, el compilador generará un error.

###### Sintaxis
```
    @FunctionalInterface
    public interface MiInterfazFuncional {
        void metodoUnico();
    }
```

- **@FunctionalInterface:** La anotación que marca la interfaz como funcional.
- **void metodoUnico():** El único método abstracto en la interfaz.


###  3. Stream API <a name="3"></a>
---
#### Conceptos básicos y flujo de datos.

La API de Streams en Java proporciona una forma moderna y funcional de procesar colecciones de datos. Introducida en Java 8, permite realizar operaciones sobre secuencias de datos de manera declarativa y paralela.

**¿Qué es un Stream?**
   
Un Stream es una **secuencia de elementos** que se pueden procesar de manera funcional y en paralelo. A diferencia de las colecciones, los Streams no almacenan datos. En su lugar, operan sobre los datos que se les pasan, proporcionando una forma fluida de realizar operaciones sobre ellos.

- No Almacena Datos: Los Streams no almacenan datos; simplemente definen un pipeline de operaciones sobre los datos.
- Operaciones Lazy: Las operaciones sobre Streams son perezosas, lo que significa que no se ejecutan hasta que se realiza una operación terminal.

El flujo de datos en Streams sigue un proceso de **pipeline** que incluye:

1. Creación del Stream: Se crea un Stream a partir de una fuente de datos.
2. Aplicación de Operaciones Intermedias: Se aplican una o más operaciones intermedias que devuelven un nuevo Stream.
3. Operación Terminal: Se realiza una operación terminal que produce un resultado o efecto colateral, y ejecuta todas las operaciones intermedias definidas.

##### Ejemplo:
```
    import java.util.*;
    import java.util.stream.*;

    public class EjemploStream {
        public static void main(String[] args) {
            List<String> nombres = Arrays.asList("Ana", "Juan", "Pedro", "Ana");

            // Pipeline de Stream
            List<String> nombresUnicosLargos = nombres.stream()
                .distinct()                 // Elimina duplicados
                .filter(s -> s.length() > 3)  // Filtra nombres largos
                .sorted()                   // Ordena
                .collect(Collectors.toList());  // Recolecta en una lista

            System.out.println(nombresUnicosLargos);  // Imprime [Pedro, Juan]
        }
    }
```

**Creación de Streams**

Es posible crear Streams a partir de diferentes fuentes:

`Desde Colecciones:`
```
    List<String> nombres = Arrays.asList("Ana", "Juan", "Pedro");
    Stream<String> stream = nombres.stream();
```

`Desde Arrays:`
```
    String[] nombresArray = {"Ana", "Juan", "Pedro"};
    Stream<String> stream = Arrays.stream(nombresArray);
```
`Desde Valores Estáticos:`
```
    Stream<String> stream = Stream.of("Ana", "Juan", "Pedro");
```

`Desde Rangos de Números:`
```
    IntStream stream = IntStream.range(1, 4);  // Genera 1, 2, 3
```
#### Operaciones intermedias (como filter(), map(), sorted()).

Las operaciones intermedias producen un nuevo Stream y son perezosas (no se ejecutan hasta que se realiza una operación terminal). Ejemplos incluyen:

**filter(Predicate<T> predicate):** Filtra elementos que cumplen con una condición.
```
    Stream<String> nombresLargos = nombres.stream().filter(s -> s.length() > 3);
```

**map(Function<T, R> mapper):** Transforma cada elemento en otro tipo de dato.
```
    Stream<Integer> longitudes = nombres.stream().map(String::length);
```

**distinct(): Elimina duplicados.**
```
    Stream<String> nombresUnicos = nombres.stream().distinct();
```

**sorted(): Ordena los elementos.**
```
    Stream<String> nombresOrdenados = nombres.stream().sorted();
```

#### Operaciones terminales (como forEach(), collect(), reduce()).

Las operaciones terminales producen un resultado o efecto colateral y finalizan el Stream. Ejemplos incluyen:

**forEach(Consumer<T> action):** Realiza una acción sobre cada elemento.
```
    nombres.stream().forEach(System.out::println);
```

**collect(Collector<T, A, R> collector):** Acumula los elementos en una colección.
```
    List<String> lista = nombres.stream().collect(Collectors.toList());
```

**reduce(T identity, BinaryOperator<T> accumulator):** Realiza una reducción sobre los elementos.
```
    Optional<Integer> suma = nombres.stream().map(String::length).reduce(Integer::sum);
```
**count():** Cuenta el número de elementos.
```
    long cantidad = nombres.stream().count();
```

**findFirst():** Devuelve el primer elemento opcionalmente.
```
    Optional<String> primerNombre = nombres.stream().findFirst();
```

#### Paralelismo con Streams (parallelStream()).

La API de Streams en Java no solo permite trabajar con datos de manera funcional y fluida, sino que también facilita la ejecución paralela de operaciones para aprovechar mejor los sistemas de múltiples núcleos. La función parallelStream() de la API de Streams permite convertir un Stream en un Stream paralelo, lo que puede mejorar el rendimiento para operaciones de procesamiento intensivo.

`parallelStream():` Es un método que se utiliza para obtener un Stream paralelo a partir de una colección. En un Stream paralelo, las operaciones son ejecutadas en paralelo en varios hilos, lo que puede llevar a mejoras significativas en el rendimiento, especialmente con grandes volúmenes de datos y operaciones computacionales costosas.

Es posible obtener un Stream paralelo llamando al método parallelStream() en una colección, como una lista:
```
    List<Integer> numeros = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    Stream<Integer> paralelo = numeros.parallelStream();
```

Las operaciones en un Stream paralelo se ejecutan en paralelo en múltiples hilos. A pesar de que la mayoría de las operaciones sobre Streams paralelos funcionan de manera similar a los Streams secuenciales, la ejecución es paralela.

```
    import java.util.*;
    import java.util.stream.*;

    public class ParalelismoConStreams {
        public static void main(String[] args) {
            List<Integer> numeros = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

            // Stream paralelo
            long startTime = System.currentTimeMillis();
            int suma = numeros.parallelStream()
                            .mapToInt(Integer::intValue)
                            .sum();
            long endTime = System.currentTimeMillis();

            System.out.println("Suma: " + suma);
            System.out.println("Tiempo de ejecución: " + (endTime - startTime) + " ms");
        }
    }
```

##### Ventajas del Paralelismo con Streams

- **Mejora del Rendimiento:** El procesamiento paralelo puede reducir el tiempo de ejecución al distribuir el trabajo entre varios núcleos del procesador.
- **Simplicidad:** Facilita la paralelización de tareas sin necesidad de manejar explícitamente hilos y sincronización.

#### Manejo de Optional y operaciones como findFirst() y findAny().

Cuando se trabaja con Streams, los métodos findFirst() y findAny() son útiles para obtener un valor de un Stream en forma de Optional.

**findFirst():** Devuelve el primer elemento del Stream, si está presente. El orden es garantizado, por lo que es útil cuando se necesita el primer elemento en un Stream ordenado o con una secuencia específica.
```
    List<String> nombres = Arrays.asList("Ana", "Juan", "Pedro");
    Optional<String> primerNombre = nombres.stream().findFirst();
    primerNombre.ifPresent(System.out::println);  // Imprime "Ana"
```

**findAny():** Devuelve cualquier elemento del Stream, si está presente. No garantiza el orden y es más eficiente en Streams paralelos, ya que puede devolver el primer elemento disponible de manera no determinista.
```
    List<String> nombres = Arrays.asList("Ana", "Juan", "Pedro");
    Optional<String> cualquierNombre = nombres.stream().findAny();
    cualquierNombre.ifPresent(System.out::println);  // Imprime cualquier nombre de la lista
```

###  4. Optional <a name="4"></a>
---
#### Concepto de Optional y su uso para evitar NullPointerException.

En Java, el uso de valores null puede ser una fuente frecuente de errores y excepciones, especialmente NullPointerException. La clase Optional, introducida en Java 8, es una solución diseñada para manejar la presencia o ausencia de valores de manera más segura y expresiva, evitando la necesidad de valores null.

`Optional:` Es una clase contenedora que representa un valor que puede estar presente o ausente. No es una colección ni un contenedor de múltiples valores, sino un mecanismo para expresar la posible ausencia de un único valor sin usar null.

- **Contenedor de Valor:** Optional puede contener un valor no nulo o estar vacío.
- **Seguridad y Expresión:** Proporciona métodos para verificar la presencia de un valor y obtener el valor de manera segura.

##### Creación de Optional

**Optional.of(T value):** Crea un Optional que contiene el valor proporcionado. Lanza una excepción si el valor es null.

```
    Optional<String> optional = Optional.of("Hola");
```

**Optional.ofNullable(T value):** Crea un Optional que puede estar vacío si el valor es null. Es útil cuando no se puede garantizar que el valor no sea null.
```
    Optional<String> optional = Optional.ofNullable(possibleNullValue);
```

**Optional.empty():** Crea un Optional vacío. Utilizado cuando no hay un valor presente.
```
    Optional<String> emptyOptional = Optional.empty();
```

#### Métodos principales (isPresent(), ifPresent(), orElse(), orElseGet(), orElseThrow()).

Métodos Comunes de Optional:

`isPresent():` Verifica si el Optional contiene un valor.
```
    if (optional.isPresent()) {
        System.out.println("El valor está presente");
    }
```

`ifPresent(Consumer<? super T> action):` Ejecuta una acción si el Optional tiene un valor. Es una forma de evitar el uso de null y if anidados.
```
    optional.ifPresent(value -> System.out.println("Valor: " + value));
```

`get():` Obtiene el valor del Optional. Lanza NoSuchElementException si el Optional está vacío, por lo que debe usarse con cuidado.
```
    String valor = optional.get();
```

`orElse(T other):` Devuelve el valor si está presente; de lo contrario, devuelve el valor alternativo proporcionado.
```
    String valor = optional.orElse("Valor por defecto");
```

`orElseGet(Supplier<? extends T> other):` Devuelve el valor si está presente; de lo contrario, usa el Supplier para proporcionar un valor alternativo. Esto es útil si el valor alternativo requiere cálculo.
```
    String valor = optional.orElseGet(() -> "Valor calculado");
```

`orElseThrow(Supplier<? extends X> exceptionSupplier):` Devuelve el valor si está presente; de lo contrario, lanza la excepción proporcionada.
```
String valor = optional.orElseThrow(() -> new IllegalArgumentException("No hay valor presente"));
```

#### Uso de Optional en Streams y otros contextos.

Cuando se trabaja con Streams, es común encontrar situaciones donde una operación puede no devolver un resultado. Optional proporciona una forma conveniente de manejar estos casos.

Puedes encadenar métodos para realizar transformaciones y comprobaciones de manera más fluida y segura:
```
    import java.util.*;

    public class EjemploEncadenado {
        public static void main(String[] args) {
            List<String> nombres = Arrays.asList("Ana", "Juan", "Pedro");

            // Buscar un nombre, convertirlo a mayúsculas y verificar longitud
            Optional<String> nombreConLongitud = nombres.stream()
                                                        .filter(nombre -> nombre.startsWith("J"))
                                                        .map(String::toUpperCase)
                                                        .filter(nombre -> nombre.length() > 3)
                                                        .findFirst();

            nombreConLongitud.ifPresent(nombre -> System.out.println("Nombre encontrado: " + nombre));
        }
    }
```

- **En Streams:** Optional se utiliza con métodos como findFirst() y findAny() para manejar la ausencia de valores de manera segura.
- **En Otros Contextos:** Optional ayuda a manejar valores que pueden ser nulos, proporcionando métodos para manejar valores de manera segura y evitar NullPointerException.
- **Ventajas:** Mejora la legibilidad del código, reduce errores y hace explícito el manejo de la ausencia de valores.

###  5. Nuevas API de Date y Time (java.time) <a name="5"></a>
---
LocalDate, LocalTime, LocalDateTime.
ZonedDateTime y manejo de zonas horarias.
Period y Duration.
Uso de DateTimeFormatter y conversión entre fechas.
###  6. Default Methods en Interfaces <a name="6"></a>
---
Concepto de métodos por defecto en interfaces.
Creación y uso de métodos por defecto.
Resolución de conflictos cuando una clase implementa múltiples interfaces con métodos por defecto.
###  7. Static Methods en Interfaces <a name="7"></a>
---
Definición y uso de métodos estáticos en interfaces.
Diferencias entre métodos estáticos y métodos por defecto.
###  8. Nueva API de Concurrencia <a name="8"></a>
---
Mejoras en la concurrencia, como CompletableFuture.
Ejecución asincrónica y manejo de tareas complejas.
Métodos avanzados de concurrencia (ForkJoinPool, parallelStream).
###  9. Mapeo de Colecciones y Streams <a name="9"></a>
---
Transformación de colecciones usando Streams.
Uso de map(), flatMap() y collect() para agrupar, transformar, y procesar colecciones.
###  10. Collectors y Reduction <a name="10"></a>
---
Uso de Collectors para operaciones avanzadas en Streams.
Reducción y agrupación de datos con métodos como groupingBy(), partitioningBy(), y joining().
###  11. Nashorn JavaScript Engine <a name="11"></a>
---
Introducción al motor JavaScript Nashorn.
Ejecución de código JavaScript desde Java.
Integración de Java con scripts en tiempo de ejecución.
###  12. Type Annotations <a name="12"></a>
---
Concepto y uso de anotaciones en diferentes partes del código.
Anotaciones en parámetros, tipos genéricos, y más.
###  13. Métodos de Fábrica de Colecciones (Java 9) <a name="13"></a>
---
Aunque esta característica se introdujo en Java 9, es útil conocer los métodos de fábrica (List.of(), Set.of(), Map.of()) para crear colecciones inmutables.
###  14. Metodología Funcional y Conceptos Avanzados <a name="14"></a>
---
Patrones de diseño aplicados en el contexto de Java 8.
Composición de funciones y técnicas avanzadas de programación funcional.
Desarrollo de APIs usando conceptos funcionales.