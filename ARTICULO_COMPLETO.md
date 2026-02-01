# NASA: El Poder de los 10
## Reglas para el desarrollo de código crítico y seguro

### Gerard J. Holzmann
### NASA/JPL Laboratory for Reliable Software
### Pasadena, CA 91109 


<!-- Most serious software development projects use coding guidelines. These guidelines are meant to state what the ground rules are for the software to be written: how it should be structured and which language features should and should not be used. Curiously, there is little consensus on what a good coding standard is. Among the many that have been written there are remarkable few patterns to discern, except that each new document tends to be longer than the one before it. 

The result is that most existing guidelines contain well over a hundred rules, sometimes with questionable justification. Some rules, especially those that try to stipulate the use of white-space in programs, may have been introduced by personal preference; others are meant to prevent very specific and unlikely types of error from earlier coding efforts within the same organization. 

Not surprisingly, the existing coding guidelines tend to have little effect on what developers actually do when they write code. The most dooming aspect of many of the guidelines is that they rarely allow for comprehensive tool-based compliance checks. Tool-based checks are important, since it is often infeasible to manually review the hundreds of thousands of lines of code that are written for larger applications -->

La mayoría de los grandes e importantes proyectos de desarrollo utiliza algún tipo de directrices de codificación. Estas directrices tienen como objetivo establecer las reglas fundamentales para el software a escribir: cómo debe estructurarse y qué características del lenguaje deben o no utilizarse. Curiosamente, hay poco consenso sobre lo que constituye un buen estándar de codificación. Entre los muchos que se han escrito, se pueden discernir muy pocos patrones notables, salvo que cada nuevo documento tiende a ser más largo que el anterior. 

El resultado es que el grueso de las directrices existentes contienen más de cien reglas, a veces con una justificación cuestionable. Algunas reglas, especialmente aquellas que tratan de establecer cuántos espacios en blanco se deben usar al escribir un programa, pueden haber sido introducidas por preferencias personales; otras están destinadas a prevenir un tipo de error improbable y muy específico que remonta a los esfuerzos tempranos de codificación de la organización. 

No es sorprendente, por tanto, que existan directrices que tengan poco o nulo efecto en las tareas que realmente realizan los desarrolladores. El aspecto más desalentador de muchas de las directrices es que rara vez permiten realizar verificaciones de cumplimiento exhaustivas basadas en herramientas. Las verificaciones basadas en herramientas son importantes, ya que muy a menudo es inviable la verificación manual para las miles de líneas de código que se escriben en grandes aplicaciones.

<!-- The benefit of existing coding guidelines is therefore often small, even for critical applications. A verifiable set of well-chosen coding rules could, however, make critical software components more thoroughly analyzable, for properties that go beyond compliance with the set of rules itself. 

To be effective, though, the set of rules has to be small, and must be clear enough that it can easily be understood and remembered. The rules will have to be specific enough that they can be checked mechanically. To put an easy upper-bound on the number of rules for an effective guideline, I will argue that we can get significant benefit by restricting to no more than ten rules. Such a small set, of course, cannot be all-encompassing, but it can give us a foothold to achieve measurable effects on software reliability and verifiability. 

To support strong checking, the rules are somewhat strict – one might even say Draconian. The trade-off, though, should be clear. When it really counts, especially in the development of safety critical code, it may be worth going the extra mile and living within stricter limits than may be desirable. In return, we should be able to demonstrate more convincingly that critical software will work as intended.  -->

Por tanto, el beneficio de unas directrices tiende a ser pequeño, incluso para aplicaciones críticas. Un conjunto de directrices verificables y bien escogidas puede, sin embargo, hacer que los componentes críticos de un software sean más analizables para propiedades que van más allá del cumplimiento de las reglas mismas.

Sin embargo, para ser efectivas, el conjunto de reglas debe ser lo suficientemente pequeño y claro para que resulte fácil de entender y recordar. Las mismas deben ser suficientemente específicas de modo que puedan verificarse de forma mecánica.
Para establecer un límite superior simple en el número de reglas para unas directrices efectivas, se puede argumentar que podemos obtener un beneficio significativo al restringirnos a no más de diez reglas. Un conjunto pequeño, obviamente, no puede abarcar la totalidad, pero puede darnos un punto de apoyo para lograr efectos medibles en la fiabilidad del software y su verificabilidad.

Para permitir una verificación robusta, las reglas deben ser algo estrictas — uno podría decir draconianas. Sin embargo, la contrapartida debería ser clara. Lo que cuenta, especialmente en el desarrollo de código crítico y seguro, es hacer un esfuerzo adicional y aceptar límites más estrictos de lo que sería deseable. A cambio, deberíamos ser capaces de demostrar de manera más convincente que el software crítico funcionará según lo previsto.

<!-- Ten Rules for Safety Critical Coding -->
## 10 reglas para el desarrollo de código crítico y seguro 

<!-- The choice of language for a safety critical code is in itself a key consideration, but we will not debate it much here. At many organizations, JPL included, critical code is written in C. 

With its long history, there is extensive tool support for this language, including strong source code analyzers, logic model extractors, metrics tools, debuggers, test support tools, and a choice of mature, stable compilers. For this reason, C is also the target of the majority of coding guidelines that have been developed. For fairly pragmatic reasons, then, our coding rules primarily target C and attempt to optimize our ability to more thoroughly check the reliability of critical applications written in C.  -->

Escoger un lenguaje seguro en sí mismo es una consideración clave, pero no debatiremos mucho al respecto aquí. En muchas organizaciones, el código crítico se escribe en C, incluyendo en el JPL (Jet Propulsion Laboratory).
Con su larga historia, hay un amplio soporte de herramientas en este lenguaje, incluyendo potentes analizadores de código fuente, extractores de modelos lógicos, herramientas de métricas, depuradores, herramientas de soporte para pruebas, y una selección de compiladores maduros y estables. Dadas estas razones, C también es el objetivo de la mayoría de las directrices de codificación que se han desarrollado.
Entonces, por pragmátismo, nuestras reglas de codificación tienen a C como objetivo principal y buscan optimizar nuestra capacidad de verificar más a fondo la fiabilidad de las aplicaciones críticas escritas en C.

### I
<!-- Restrict all code to very simple control flow constructs—do not use goto statements, setjmp or longjmp constructs, or direct or indirect recursion. -->

**REGLA**: Restringir todo el código a estructuras de control de flujo muy simples. No utilizar sentencias `goto`, construcciones `setjmp` o `longjmp`, ni recursión directa o indirecta.

<!-- Rationale: Simpler control flow translates into stronger capabilities for verification and often results in improved code clarity. The banishment of recursion is perhaps the biggest surprise here. Without recursion, though, we are guaranteed to have an acyclic function call graph, which can be exploited by code analyzers, and can directly help to prove that all executions that should be bounded are in fact bounded. (Note that this rule does not require that all functions have a single point of return – although this often also simplifies control flow. There are enough cases, though, where an early error return is the simpler solution.)  -->
**RAZÓN**: Un flujo de control simple se traduce en mejores capacidades para la verificación y a menudo resulta en una mayor claridad del código. La prohibición de la recursividad es quizás la mayor sorpresa. Sin recursión, sin embargo, se garantiza un grafo de llamadas a funciones acíclico, lo cual puede ser explotado por los analizadores de código y puede ayudar directamente a probar que todas las ejecuciones que deberían estar acotadas, de hecho lo están. (Notese que esta regla no requiere que todas las funciones tengan un único punto de retorno; sin embargo, a menudo también simplifica el flujo de control. Hay suficientes casos, creemos, donde un retorno de error temprano es la solución más simple).

### II

<!-- Give all loops a fixed upper bound. It must be trivially possible for a checking tool to prove statically that the loop cannot exceed a preset upper bound on the number of iterations. If a tool cannot prove the loop bound statically, the rule is considered violated. -->
**REGLA**: Asignar a todos los bucles un límite superior fijo. Debe ser `trivialmente` posible para una herramienta de comprobación demostrar estáticamente que el bucle no puede exceder un límite preestablecido de iteraciones. Si una herramienta no puede demostrar el límite del bucle de forma estática, se considerará que la regla ha sido vulnerada.

<!-- Rationale: The absence of recursion and the presence of loop bounds prevents runaway code. This rule does not, of course, apply to iterations that are meant to be non-terminating (e.g., in a process scheduler). In those special cases, the reverse rule is applied: it should be statically provable that the iteration cannot terminate. One way to support the rule is to add an explicit upper-bound to all loops that have a variable number of iterations (e.g., code that traverses a linked list). When the upperbound is exceeded an assertion failure is triggered, and the function containing the failing iteration returns an error. (See Rule 5 about the use of assertions.)  -->
**RAZÓN**: La ausencia de recursión y la presencia de límites fijos para los bucles previene el código descontrolado. Por supuesto, esta regla no aplica a iteraciones que por definición no deben terminar nunca (un planificador de procesos/*process scheduler* por ejemplo). En esos casos, la regla opuesta debe aplicarse: debe ser estáticamente comprobable que la iteración no puede terminar. Una manera de promover esta regla es añadir un límite superior explícito a todos los bucles que tienen un número variable de iteraciones (por ejemplo, código que navega a través de una lista enlazada). Cuando el límite superior es excedido, la aserción de falla es activada, y la función que contiene la falla retorna un error. (Ver Regla 5 sobre el uso de las aserciones).

### III

<!-- Do not use dynamic memory allocation after initialization. -->

**REGLA**: No utilizar asignación dinámica de memoria después de la fase de inicialización.

<!-- Rationale: This rule is common for safety critical software and appears in most coding guidelines. The reason is simple: memory allocators, such as malloc, and garbage collectors often have unpredictable behavior that can significantly impact performance. A notable class of coding errors also stems from mishandling of memory allocation and free routines: forgetting to free memory or continuing to use memory after it was freed, attempting to allocate more memory than physically available, overstepping boundaries on allocated memory, etc. Forcing all applications to live within a fixed, pre-allocated, area of memory can eliminate many of these problems and make it easier to verify memory use. Note that the only way to dynamically claim memory in the absence of memory allocation from the heap is to use stack memory. In the absence of recursion (Rule 1), an upper-bound on the use of stack memory can derived statically, thus making it possible to prove that an application will always live within its pre-allocated memory means.  -->

**RAZÓN**: Esta regla es muy común para el código crítico y aparece en la mayoría de las directrices. La razón es simple: los asignadores de memoria como `malloc` y los `recolectores de basura` (*GC*) tienden a tener comportamientos impredecibles que pueden impactar significativamente en el desempeño. Una clase de errores notables surge también del mal manejo de la asignación de memoria y las rutinas de liberación: olvidar liberar la memoria o continuar usando un bloque después de haber sido liberado, intentar asignar más memoria de la que hay físicamente disponible, sobrepasar límites en la memoria asignada, etc.

Forzar a todas las aplicaciones a operar dentro de un área de memoria fija y preasignada puede eliminar muchos de estos problemas y facilitar la verificación del uso de la memoria. Nótese que la única manera de reclamar memoria dinámicamente sin recurrir a la asignación de la `memoria dinámica` (*Heap*) es usar la `memoria de la pila` (*Stack*). En ausencia de la recursión (Regla 1), un límite superior en el uso de la `memoria de la pila` (*Stack*) puede ser derivado estáticamente, haciendo por tanto posible demostrar que una aplicación siempre operará dentro de los límites de memoria preasignados.

### IV
<!-- No function should be longer than what can be printed on a single sheet of paper in a standard format with one line per statement and one line per declaration. Typically, this means no more than about 60 lines of code per function. -->

**REGLA**: No extender la longitud de ninguna función más allá de lo que se puede imprimir en una sola hoja de papel en un formato estándar, con una línea por sentencia y una línea por declaración. Típicamente, esto significa un máximo de 60 líneas de código por función.

> **Nota:** El estándar de referencia es la hoja tamaño carta (Letter) comúnmente usada en EE.UU.

<!-- Rationale: Each function should be a logical unit in the code that is understandable and verifiable as a unit. It is much harder to understand a logical unit that spans multiple screens on a computer display or multiple pages when printed. Excessively long functions are often a sign of poorly structured code.  -->

**RAZÓN**: Cada función debe ser una unidad lógica que sea comprensible y verificable como una unidad. Es mucho más difícil entender una unidad lógica que abarca múltiples pantallas en una computadora o múltiples páginas si es impresa. Funciones excesivamente largas a menudo implican código deficientemente estructurado.

### V
#### 5. Densidad de Aserciones

<!-- The code's assertions density should average to minimally two assertions per function. Assertions must be used to check for anomalous conditions that should never happen in real-life executions. Assertions must be side-effect free and should be defined as Boolean tests. When an assertion fails, an explicit recovery action must be taken such as returning an error condition to the caller of the function that executes the failing assertion. Any assertion for which a static checking tool can prove that it can never fail or never hold violates this rule. -->

Asegurar un mínimo de dos aserciones por función. Deben usarse para verificar condiciones anómalas que nunca deberían ocurrir en ejecuciones reales. 

Deben carecer de efectos secundarios y deben definirse como expresiones booleanas. Cuando una aserción falla, se debe ejecutar una acción de recuperación explícita, como devolver un código  de error al invocador de la función que falló. 

Cualquier aserción para la cual una herramienta de comprobación estática pueda demostrar que nunca fallará o que nunca se cumplirá, viola esta regla.

<!-- Rationale: Statistics for industrial coding efforts indicate that unit tests often find at least one defect per 10 to 100 lines of code written. The odds of intercepting defects increase with assertion density. Use of assertions is often also recommended as part of a strong defensive coding strategy. Assertions can be used to verify pre- and postconditions of functions, parameter values, return values of functions, and loopinvariants. Because assertions are side-effect free, they can be selectively disabled -->
<!-- after testing in performance-critical code. -->
<!--  -->
<!-- A typical use of an assertion would be as follows: -->
    <!-- if (!c_assert(p >= 0) == true) { -->
    <!-- return ERROR; -->
    <!-- } -->
<!--  -->
<!-- with the assertion defined as follows: -->
    <!-- #define c_assert(e) ((e) ? (true) : \ -->
    <!-- tst_debugging(”%s,%d: assertion ’%s’ failed\n”, \ -->
    <!-- __FILE__, __LINE__, #e), false) -->
    <!--  -->
<!-- In this definition, __FILE__ and __LINE__ are predefined by the macro preprocessor to produce the filename and line-number of the failing assertion. The syntax #e turns the assertion condition e into a string that is printed as part of the error message. In code destined for an embedded processor there is of course no place to print the error message itself – in that case, the call to tst_debugging is turned into a no-op, and the assertion turns into a pure Boolean test that enables error recovery from anomolous behavior.  -->

RAZON: Las estadistica para los efuerzos de codigo industrial parecen indicar que los test unitarios encuentran al menos un defecto por cada 10 a 100 lineas de codigo escritas. El chance de encontrar un defecto se incrementa con la densidad de las aserciones. El uso de aserciones es comunmente recomendado como para de una fuerte estrategia defensiva. Las aserciones  pueden ser usadas para verificar pre y post condiciones de las funciones, el valor de sus parametros y sus retornos, los bucles. Como las aserciones estan libres de efectos secundarios, pueden ser libremente desactivadas despues de probar el redimiento del codigo critico.
rn values of functions, and loopinvariants. Because assertions are side-effect free, they can be selectively disabled -->
<!-- after testing in performance-critical code. -->
<!--  -->
<!-- A typical use of an assertion would be as follows: -->
    <!-- if (!c_assert(p >= 0) == true) { -->
    <!-- return ERROR; -->
    <!-- } -->
<!--  -->
<!-- with the assertion defined as follows: -->
    <!-- #define c_assert(e) ((e) ? (true) : \ -->
    <!-- tst_debugging(”%s,%d: assertion ’%s’ failed\n”, \ -->
    <!-- __FILE__, __LINE__, #e), false) -->
    <!--  -->
<!-- In this definition, __FILE__ and __LINE__ are predefined by the macro preprocessor to produce the filename and line-number of the failing assertion. The syntax #e turns the assertion condition e into a string that is printed as part of the error message. In code destined for an embedded processor there is of course no place to print the error message itself – in that case, the call to tst_debugging is turned into a no-op, and the assertion turns into a pure Boolean test that enables error recovery from anomolous behavior.  -->

**RAZÓN**: Las estadísticas para los esfuerzos de código industrial parecen indicar que las pruebas unitarias encuentran al menos un defecto por cada 10 a 100 líneas de código escritas. La probabilidad de encontrar un defecto se incrementa con la densidad de las aserciones. El uso de aserciones es comúnmente recomendado como parte de una fuerte estrategia defensiva. Las aserciones pueden usarse para verificar precondiciones y postcondiciones de funciones, el valor de sus parámetros y sus valores de retorno, e invariantes de bucle. Como las aserciones están libres de efectos secundarios, pueden ser desactivadas selectivamente después de las pruebas del código crítico para mejorar el rendimiento.

Un ejemplo de uso de una aserción sería el siguiente:

```c
if (!c_assert(p >= 0) == true) {
    return ERROR;
}
```

Con la sercion definida asi:
```c
#define c_assert(e) ((e) ? (true) : \
    tst_debugging(”%s,%d: assertion ’%s’ failed\n”, \
    __FILE__, __LINE__, #e), false)
```
En esta definición, __FILE__ y __LINE__ están predefinidas por el preprocesador de macros para producir el nombre del archivo y el número de línea de la aserción que falla. La sintaxis #e transforma la condición de la aserción e en una cadena de texto (string) que se imprime como parte del mensaje de error. En código destinado a un procesador embebido, por supuesto, no hay lugar para imprimir el mensaje de error en sí; en ese caso, la llamada a tst_debugging se convierte en una operación nula (no-op), y la aserción se transforma en una prueba booleana pura que habilita la recuperación de errores ante un comportamiento anómalo.