# ![NASA LOGO](./img/NASA_logo.svg) NASA: El Poder de Diez
## Reglas para el desarrollo de código crítico y seguro [1]

---
**Autor:** Gerard J. Holzmann
**Afiliación:** NASA/JPL Laboratory for Reliable Software
**Dirección:** Pasadena, CA 91109
---


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

<!-- The code's assertions density should average to minimally two assertions per function. Assertions must be used to check for anomalous conditions that should never happen in real-life executions. Assertions must be side-effect free and should be defined as Boolean tests. When an assertion fails, an explicit recovery action must be taken such as returning an error condition to the caller of the function that executes the failing assertion. Any assertion for which a static checking tool can prove that it can never fail or never hold violates this rule. -->

Asegurar un mínimo de dos aserciones por función. Deben usarse para verificar condiciones anómalas que nunca deberían ocurrir en ejecuciones reales. 

Deben carecer de efectos secundarios y deben definirse como expresiones booleanas. Cuando una aserción falla, se debe ejecutar una acción de recuperación explícita, como devolver un código  de error al invocador de la función que falló. 

Cualquier aserción para la cual una herramienta de comprobación estática pueda demostrar que nunca fallará o que nunca se cumplirá, viola esta regla.

<!-- Rationale: Statistics for industrial coding efforts indicate that unit tests often find at least one defect per 10 to 100 lines of code written. The odds of intercepting defects increase with assertion density. Use of assertions is often also recommended as part of a strong defensive coding strategy. Assertions can be used to verify pre- and postconditions of functions, parameter values, return values of functions, and loopinvariants. Because assertions are side-effect free, they can be selectively disabled -->
<!-- after testing in performance-critical code. -->
<!--  -->
<!-- A typical use of an assertion would be as follows: -->
<!-- 
    if (!c_assert(p >= 0) == true) {
        return ERROR; 
    }
-->
<!--  -->
<!-- with the assertion defined as follows: -->
<!--
    #define c_assert(e) ((e) ? (true) :
    tst_debugging(”%s,%d: assertion ’%s’ failed\n”,
    __FILE__, __LINE__, #e), false)
-->
<!-- In this definition, __FILE__ and __LINE__ are predefined by the macro preprocessor to produce the filename and line-number of the failing assertion. The syntax #e turns the assertion condition e into a string that is printed as part of the error message. In code destined for an embedded processor there is of course no place to print the error message itself – in that case, the call to tst_debugging is turned into a no-op, and the assertion turns into a pure Boolean test that enables error recovery from anomolous behavior.  -->

RAZON: Las estadistica para los efuerzos de codigo industrial parecen indicar que los test unitarios encuentran al menos un defecto por cada 10 a 100 lineas de codigo escritas. El chance de encontrar un defecto se incrementa con la densidad de las aserciones. El uso de aserciones es comunmente recomendado como para de una fuerte estrategia defensiva. Las aserciones  pueden ser usadas para verificar pre y post condiciones de las funciones, el valor de sus parametros y sus retornos, los bucles. Como las aserciones estan libres de efectos secundarios, pueden ser libremente desactivadas despues de probar el redimiento del codigo critico.

**RAZÓN**: Las estadísticas para los esfuerzos de código industrial parecen indicar que las pruebas unitarias encuentran al menos un defecto por cada 10 a 100 líneas de código escritas. La probabilidad de encontrar un defecto se incrementa con la densidad de las aserciones. El uso de aserciones es comúnmente recomendado como parte de una fuerte estrategia defensiva. Las aserciones pueden usarse para verificar precondiciones y postcondiciones de funciones, el valor de sus parámetros y sus valores de retorno, e invariantes de bucle. Como las aserciones están libres de efectos secundarios, pueden ser desactivadas selectivamente después de las pruebas del código crítico para mejorar el rendimiento.

Un ejemplo de uso de una aserción sería el siguiente:

```c
if (!c_assert(p >= 0) == true) {
    return ERROR;
}
```

Con la aserción definida así:
```c
#define c_assert(e) ((e) ? (true) : \
    tst_debugging(”%s,%d: assertion ’%s’ failed\n”, \
    __FILE__, __LINE__, #e), false)
```
En esta definición, __FILE__ y __LINE__ están predefinidas por el preprocesador de macros para producir el nombre del archivo y el número de línea de la aserción que falla. La sintaxis #e transforma la condición de la aserción e en una cadena de texto (string) que se imprime como parte del mensaje de error. En código destinado a un procesador embebido, por supuesto, no hay lugar para imprimir el mensaje de error en sí; en ese caso, la llamada a tst_debugging se convierte en una operación nula (no-op), y la aserción se transforma en una prueba booleana pura que habilita la recuperación de errores ante un comportamiento anómalo.

### VI

<!-- Declare all data objects at the smallest possible level of scope. -->

Declarar todos los objetos de datos (variables) en el nivel de alcance (*scope*) más pequeño posible.

<!-- Rationale: This rule supports a basic principle of data-hiding. Clearly if an object is not in scope, its value cannot be referenced or corrupted. Similarly, if an erroneous value of an object has to be diagnosed, the fewer the number of statements where the value could have been assigned; the easier it is to diagnose the problem. The rule discourages the re-use of variables for multiple, incompatible purposes, which can complicate fault diagnosis -->

**RAZÓN**: Esta regla sostiene el principio básico de la ocultación de datos (*data-hiding*). Claramente, si un objeto/dato no está en el alcance (*scope*), su valor no puede ser referenciado o corrompido. De modo similar, si un valor erróneo en un objeto/dato es identificado, cuanto menor sea el número de sentencias donde el valor pudo haber sido asignado, resulta más fácil diagnosticar el problema. Esta regla desalienta la reutilización de variables para múltiples propósitos incompatibles, lo que puede complicar el diagnóstico de fallos.

### VII

<!-- Each calling function must check the return value of nonvoid functions, and each called function must check the validity of all parameters provided by the caller. -->

Verificar el valor de retorno de las funciones no vacías (`non-void`) que son llamadas desde la función invocadora. Verificar también la validez de todos los parámetros proporcionados por el invocador en cada función invocada.

<!-- Rationale: This is possibly the most frequently violated rule, and therefore somewhat more suspect as a general rule. In its strictest form, this rule means that even the return value of printf statements and file close statements must be checked. One can make a case, though, that if the response to an error would rightfully be no different than the response to success, there is little point in explicitly checking a return value. This is often the case with calls to printf and close. In cases like these, it can be acceptable to explicitly cast the function return value to (void) – thereby indicating that the programmer explicitly and not accidentally decides to ignore a return value. In more dubious cases, a comment should be present to explain why a return value is irrelevant. In most cases, though, the return value of a function should not be ignored, especially if error return values must be propagated up the function call chain. Standard libraries famously violate this rule with potentially grave consequences. See, for instance, what happens if you accidentally execute strlen(0), or strcat(s1, s2, -1) with the standard C string library – it is not pretty. By keeping the general rule, we make sure that exceptions must be justified, with mechanical checkers flagging violations. Often, it will be easier to comply with the rule than to explain why noncompliance might be acceptable.  -->

**RAZÓN**: Probablemente esta sea la regla más violada de todo el decálogo, y por lo tanto es algo más sospechosa como regla general. En su forma más estricta, esta regla pretende que incluso el valor de retorno de sentencias como `printf` y `file close` debe ser verificado. Se puede argumentar, sin embargo, que si la respuesta a un error no fuera, justificadamente, diferente a la respuesta de éxito, tiene poco sentido verificar explícitamente un valor de retorno. Esto es a menudo el caso con las llamadas a `printf` y `close`. En casos como estos, puede ser aceptable hacer un *cast* (conversión de tipo) explícito del valor de retorno de la función a `(void)` – indicando así que el programador, de forma explícita y no accidental, decide ignorar un valor de retorno.

En casos más dudosos, debe haber un comentario en el código para presentar una explicación de por qué el retorno de dicha función es irrelevante. En la mayoría de los casos, sin embargo, el valor de retorno de una función no debe ser ignorado, especialmente si los valores de error deben propagarse hacia arriba en la cadena de llamadas de las funciones.

Las bibliotecas estándar violan esta regla de manera notoria, con consecuencias potencialmente graves. Observe, por ejemplo, lo que pasa si ejecuta accidentalmente `strlen(0)` o `strcat(s1, s2, -1)` con la librería C estándar – no es nada agradable. Al mantener esta regla general, nos aseguramos que tales excepciones deban ser justificadas, con los verificadores mecánicos señalando las violaciones. Comúnmente, será más fácil cumplir con la regla que explicar por qué omitirla puede ser aceptable.

### VIII
<!-- The use of the preprocessor must be limited to the inclusion of header files and simple macros definitions. Token pasting, variable argument lists (ellipses), and recursive macro calls are not allowed. All macros must expand into complete syntactic units. The use of conditional compilation directives must be kept to a minimum. -->

Limitar el uso del preprocesador a la inclusión de archivos de cabecera y a definiciones simples de macros.  No se permite la concatenación de tokens (*token pasting*), listas de argumentos variables (elipses), ni las llamadas a macros recursivas. 

Todos las macros deben expandirse a unidades sintácticas completas. El uso de directivas de compilación condicional debe reducirse al mínimo.

<!-- Rationale: The C preprocessor is a powerful obfuscation tool that can destroy code clarity and befuddle many text based checkers. The effect of constructs in unrestricted preprocessor code can be extremely hard to decipher, even with a formal language  definition in hand. In a new implementation of the C preprocessor, developers often have to resort to using earlier implementations as the referee for interpreting complex defining language in the C standard. The rationale for the caution against conditional compilation is equally important. Note that with just ten conditional compilation directives, there could be up to 210 possible versions of the code, each of which would have to be tested– causing a huge increase in the required test effort. -->

**RAZÓN**: El preprocesador de C es una poderosa herramienta de ofuscación que puede destruir la claridad del código y confundir a muchos verificadores. El efecto de las construcciones en código de preprocesador irrestricto puede ser extremadamente difícil de descifrar, incluso teniendo a mano una definición formal del lenguaje. En una nueva implementación del preprocesador de C, los desarrolladores a menudo tienen que recurrir a utilizar implementaciones anteriores para tener un punto de referencia con el que poder interpretar el complejo lenguaje definitorio del estándar C.

La justificación para la precaución contra la compilación condicional es igualmente importante. Nótese que con solo diez directivas de compilación condicional, podría haber hasta $2^{10}$ posibles versiones distintas del código final, cada una de las cuales tendría que ser probada (*tested*), causando un enorme aumento en el esfuerzo de prueba requerido.

### IX

<!-- The use of pointers must be restricted. Specifically, no more than one level of dereference should be used. Pointer dereference operations may not be hidden in macro definitions or inside typedef declarations. Function pointers are not permitted. -->

Restringir el uso de punteros. Específicamente, no usar más de un nivel de desreferenciación. Las operaciones de desreferencia de punteros no pueden estar ocultas en definiciones de macros ni dentro de declaraciones `typedef`. No se permiten los punteros a funciones.

<!-- Rationale: Pointers are easily misused, even by experienced programmers. They can make it hard to follow or analyze the flow of data in a program, especially by toolbased static analyzers. Function pointers, similarly, can seriously restrict the types of checks that can be performed by static analyzers and should only be used if there is a strong justification for their use, and ideally alternate means are provided to assist tool-based checkers determine flow of control and function call hierarchies. For instance, if function pointers are used, it can become impossible for a tool to prove absence of recursion, so alternate guarantees would have to be provided to make up for this loss in analytical capabilities.  -->

**RAZÓN**: Los punteros pueden ser fácilmente mal utilizados, incluso por programadores expertos. Pueden dificultar el seguimiento y/o análisis del flujo de datos en un programa, especialmente para herramientas basadas en el análisis estático.

Los punteros a una función, de manera similar, pueden restringir seriamente las verificaciones que los analizadores estáticos pueden realizar, y solo deberían usarse si existe una justificación sólida y, de manera ideal, se proveen medios alternos para asistir a los comprobadores basados en herramientas a determinar el flujo de control en las jerarquías de llamadas a funciones.

Por ejemplo, si se utilizan punteros a una función, puede volverse imposible para una herramienta comprobar la ausencia de recursión; por tanto, se deben proporcionar garantías alternas para compensar esta pérdida de capacidades analíticas.

### X

<!-- All code must be compiled, from the first day of development, with all compiler warnings enabled at the most pedantic setting available. All code must compile without warnings. All code must also be checked daily with at least one, but preferably more than one, strong static source code analyzer and should pass all analyses with zero warnings. -->

Compilar todo el código, desde el primer día de desarrollo, con todas las advertencias (*warnings*) del compilador activadas en la configuración más estricta (*pedantic*) disponible. Todo el código debe compilar sin advertencias. 

Asimismo, el código debe ser verificado diariamente con al menos un (pero preferiblemente más de uno) analizador estático de código fuente robusto, y debe superar todos los análisis con cero advertencias.

<!-- Rationale: There are several very effective static source code analyzers on the market today, and quite a few freeware tools as well.2 There simply is no excuse for any software development effort not to make use of this readily available technology. It should be considered routine practice, even for non-critical code development. The rule of zero warnings applies even in cases where the compiler or the static analyzer gives an erroneous warning: if the compiler or the static analyzer gets confused, the code causing the confusion should be rewritten so that it becomes more trivially valid. Many developers have been caught in the assumption that a warning was surely invalid, only to realize much later that the message was in fact valid for less obvious reasons. Static analyzers have somewhat of a bad reputation due to early predecessors, such as lint, that produced mostly invalid messages, but this is no longer the case. The best static analyzers today are fast, and they produce selective and accurate messages. Their use should not be negotiable at any serious software project. -->

 <!-- 2 For an overview see, for instance, http://spinroot.com/static/index.html -->

**RAZÓN**: Existen varios analizadores de código fuente estático muy efectivos en el mercado, y también bastantes herramientas gratuitas [2]. Simplemente no hay excusa para que ningún esfuerzo de desarrollo de software no utilice esta tecnología fácilmente disponible. Debería considerarse práctica rutinaria, incluso para el desarrollo de código no crítico.

La regla de cero advertencias aplica incluso en casos donde el compilador o el analizador lanza advertencias erróneas: si el compilador o el analizador resulta confundido, el código causante de la confusión debería reescribirse para que se vuelva trivialmente válido. Muchos desarrolladores han sido sorprendidos por la presunción de que una advertencia era inválida, solo para darse cuenta mucho después de que el mensaje era, de hecho, válido por razones menos obvias. Los analizadores estáticos tienen algo de mala reputación debido a sus predecesores tempranos, tales como *lint*, que producían mayormente mensajes inválidos, pero este ya no es el caso. Los mejores analizadores estáticos de hoy en día son rápidos y producen mensajes selectivos y precisos. Su uso no debería ser negociable en ningún proyecto de software serio.

## Conclusiones

<!-- The first two rules guarantee the creation of a clear and transparent control flow structure that is easier to build, test, and analyze. The absence of dynamic memory allocation, stipulated by the third rule, eliminates a class of problems related to the allocation and freeing of memory, the use of stray pointers, etc. The next few rules (4 to 7) are fairly broadly accepted as standards for good coding style. Some benefits of other coding styles that have been advanced for safety critical systems, e.g., the discipline of “design by contract” can partly be found in rules 5 to 7. -->

Las dos primeras reglas garantizan la creación de una estructura de flujo de control clara y transparente, que es más fácil de construir, probar y analizar. La ausencia de asignación dinámica de memoria, estipulada por la tercera regla, elimina una clase de problemas relacionados con la asignación y liberación de memoria, el uso de punteros errantes (*stray pointers*), etc. Las siguientes reglas (4 a 7) son aceptadas de forma bastante general como estándares de buen estilo de codificación. Ciertos beneficios de otros estilos de codificación promovidos para sistemas críticos de seguridad, p. ej., la disciplina de *"diseño por contrato"* (*design by contract*), se pueden encontrar parcialmente en las reglas 5 a 7.

<!-- These ten rules are being used experimentally at JPL in the writing of mission critical software, with encouraging results. After overcoming a healthy initial reluctance to live within such strict confines, developers often find that compliance with the rules does tend to benefit code clarity, analyzability, and code safety. The rules lessen the burden on the developer and tester to establish key properties of the code (e.g., termination or boundedness, safe use of memory and stack, etc.) by other means. If the rules seem Draconian at first, bear in mind that they are meant to make it possible to check code where very literally your life may depend on its correctness: code that is used to control the airplane that you fly on, the nuclear power plant a few miles from where you live, or the spacecraft that carries astronauts into orbit. The rules act like the seat-belt in your car: initially they are perhaps a little uncomfortable, but after a while their use becomes second-nature and not using them becomes unimaginable.  -->

Estas diez reglas se están utilizando experimentalmente en el JPL en la escritura de software crítico para misiones, con resultados alentadores. Tras superar una reticencia inicial saludable a operar dentro de confines tan estrictos, los desarrolladores a menudo descubren que el cumplimiento de las reglas tiende a beneficiar la claridad del código, su capacidad de análisis (*analyzability*) y su seguridad.

Las reglas alivian la carga del desarrollador y del probador para establecer propiedades clave del código (p. ej., terminación o acotamiento, uso seguro de memoria y pila (*stack*), etc.) por otros medios. Si las reglas parecen draconianas al principio, tenga en cuenta que están destinadas a permitir la verificación de código donde, literalmente, su vida puede depender de su corrección: código utilizado para controlar el avión en el que vuela, la central nuclear a pocos kilómetros de donde vive, o la nave espacial que transporta astronautas a órbita. Las reglas actúan como el cinturón de seguridad de su coche: inicialmente son quizás un poco incómodas, pero después de un tiempo su uso se vuelve instintivo y no usarlas se vuelve inimaginable.

---
[1] La investigación descrita en este artículo se llevó a cabo en el *Jet Propulsion Laboratory* del *California Institute
of Technology*, bajo un contrato con la Administración Nacional de Aeronáutica y del Espacio (NASA).

[2] Para una visión general, consulte, por ejemplo, http://spinroot.com/static/index.html


Link al articulo original: https://spinroot.com/gerard/pdf/P10.pdf