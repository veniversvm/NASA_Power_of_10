# NASA: El Poder de los 10
## Reglas para el desarrollo de código crítico y seguro

### Introducción
Este conjunto de reglas, desarrollado por el **JPL (Jet Propulsion Laboratory)** de la NASA, establece un estándar estricto para asegurar la fiabilidad y seguridad del código en sistemas embebidos críticos.

---

### Las 10 Reglas

#### 1. Flujo de Control Simple

<!-- Restrict all code to very simple control flow constructs—do not use goto statements, setjmp or longjmp constructs, or direct or indirect recursion. -->

Restringir todo el código a estructuras de control de flujo muy simples. No utilizar sentencias `goto`, construcciones `setjmp` o `longjmp`, ni recursión directa o indirecta.

#### 2. Límites Fijos en Bucles

<!-- Give all loops a fixed upper bound. It must be trivially possible for a checking tool to prove statically that the loop cannot exceed a preset upper bound on the number of iterations. If a tool cannot prove the loop bound statically, the rule is considered violated. -->

Asignar a todos los bucles un límite superior fijo. Debe ser `trivialmente` posible para una herramienta de comprobación demostrar estáticamente que el bucle no puede exceder un límite preestablecido de iteraciones. Si una herramienta no puede demostrar el límite del bucle de forma estática, se considerará que la regla ha sido vulnerada.

#### 3. Gestión de Memoria Estática

<!-- Do not use dynamic memory allocation after initialization. -->

No utilizar asignación dinámica de memoria después de la fase de inicialización.

#### 4. Claridad y Longitud de Funciones

<!-- No function should be longer than what can be printed on a single sheet of paper in a standard format with one line per statement and one line per declaration. Typically, this means no more than about 60 lines of code per function. -->

No extender la longitud de ninguna función más allá de lo que se puede imprimir en una sola hoja de papel en un formato estándar, con una línea por sentencia y una línea por declaración. Típicamente, esto significa un máximo de 60 líneas de código por función.

> **Nota:** El estándar de referencia es la hoja tamaño carta (Letter) comúnmente usada en EE.UU.

#### 5. Densidad de Aserciones

<!-- The code's assertions density should average to minimally two assertions per function. Assertions must be used to check for anomalous conditions that should never happen in real-life executions. Assertions must be side-effect free and should be defined as Boolean tests. When an assertion fails, an explicit recovery action must be taken such as returning an error condition to the caller of the function that executes the failing assertion. Any assertion for which a static checking tool can prove that it can never fail or never hold violates this rule. -->

Asegurar un mínimo de dos aserciones por función. Deben usarse para verificar condiciones anómalas que nunca deberían ocurrir en ejecuciones reales. 

Deben carecer de efectos secundarios y deben definirse como expresiones booleanas. Cuando una aserción falla, se debe ejecutar una acción de recuperación explícita, como devolver un código  de error al invocador de la función que falló. 

Cualquier aserción para la cual una herramienta de comprobación estática pueda demostrar que nunca fallará o que nunca se cumplirá, viola esta regla.

#### 6. Alcance de Datos Mínimo

<!-- Declare all data objects at the smallest possible level of scope. -->

Declarar todos los objetos de datos (variables) en el nivel de alcance (*scope*) más pequeño posible.

#### 7. Validación de Retornos y Parámetros

<!-- Each calling function must check the return value of nonvoid functions, and each called function must check the validity of all parameters provided by the caller. -->

Verificar el valor de retorno de las funciones no vacías (`non-void`) que son llamadas desde la función invocadora. Verificar también la validez de todos los parámetros proporcionados por el invocador en cada función invocada.

#### 8. Uso Restringido del Preprocesador

<!-- The use of the preprocessor must be limited to the inclusion of header files and simple macros definitions. Token pasting, variable argument lists (ellipses), and recursive macro calls are not allowed. All macros must expand into complete syntactic units. The use of conditional compilation directives must be kept to a minimum. -->

Limitar el uso del preprocesador a la inclusión de archivos de cabecera y a definiciones simples de macros.  No se permite la concatenación de tokens (*token pasting*), listas de argumentos variables (elipses), ni las llamadas a macros recursivas. 

Todos las macros deben expandirse a unidades sintácticas completas. El uso de directivas de compilación condicional debe reducirse al mínimo.

#### 9. Restricción de Punteros

<!-- The use of pointers must be restricted. Specifically, no more than one level of dereference should be used. Pointer dereference operations may not be hidden in macro definitions or inside typedef declarations. Function pointers are not permitted. -->

Restringir el uso de punteros. Específicamente, no usar más de un nivel de desreferenciación. Las operaciones de desreferencia de punteros no pueden estar ocultas en definiciones de macros ni dentro de declaraciones `typedef`. No se permiten los punteros a funciones.

#### 10. Compilación y Análisis Estático

<!-- All code must be compiled, from the first day of development, with all compiler warnings enabled at the most pedantic setting available. All code must compile without warnings. All code must also be checked daily with at least one, but preferably more than one, strong static source code analyzer and should pass all analyses with zero warnings. -->

Compilar todo el código, desde el primer día de desarrollo, con todas las advertencias (*warnings*) del compilador activadas en la configuración más estricta (*pedantic*) disponible. Todo el código debe compilar sin advertencias. 

Asimismo, el código debe ser verificado diariamente con al menos un (pero preferiblemente más de uno) analizador estático de código fuente robusto, y debe superar todos los análisis con cero advertencias.