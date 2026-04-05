# Marco Teórico

## Contexto general de la calidad del software
El hecho de desarrollar un software de mala calidad puede generar incidencias, retrasos, costes y problemas que afectan a la empresa y a los desarrolladores.

La calidad del software no se limita a que una app funcione, sino que se haga de manera correcta y que siga la linea de los requisitos y casos de uso definidos previamente. 
Entonces, el asegurar la calidad, por definición representa el conjunto de procesos, prácticas y mecanismos que garantizan que el software se desarrolla y se valida en base a unos estándares definidos. Este previene defectos, da fiabilidad y evidencia que el sistema cumple con la calidad esperada, es decir, da una capa de control y de mejora continua.

## Papel del testing
El testing constituye una de las actividades más relevantes dentro de QA. Su función principal se basa en diseñar, ejecutar y evaluar pruebas con el fin de comprobar si un sistema se comporta según lo esperado. A través del testing se validan los requisitos, se detectan los defectos y todo ello antes de la puesta en producción y eso otorga confianza sobre el estado del producto.

Se puede decir que el testing tradicional tiene limitaciones, ya que muchas veces el proceso depende de un esfuerzo manual para interpretar la documentación, diseñar casos de prueba, ejecutar validaciones y registrar incidencias, lo que incrementa el tiempo necesario y la carga operativa del equipo. Por lo que la  repetición de tareas similares y depender del conocimiento del ingeniero de QA hace que sea muy dificil poder escalar el proceso y puede limitar tanto la cobertura como la capacidad de adaptación a nuevas necesidades.

## Automatización del testing

La automatización del testing permite ejecutar validaciones de forma repetible, rápida y consistente, haciendo uso de scripts, herramientas y pipelines que permiten reducir parte de la intervención manual y aportar estabilidad. 

No obstante, sigue siendo necesario interpretar requisitos y diseñar los casos de prueba, y el testing tradicional no siempre resuelve el problema previo.

## Aplicación de IA

La aplicación de Inteligencia Artificial al testing supone una evolución. No solo puede ejecutar o asistir tareas repetitivas, sino también participar en procesos de análisis, estructuración y generación de contenido útil para QA, como escenarios de prueba o interpretaciones semánticas de documentación. 

Este cambio de enfoque resulta especialmente interesante cuando la carga del proceso recae sobre tareas cognitivas y documentales. En ese tipo de contexto, la IA puede aportar apoyo no tanto en la ejecución, sino en la fase de preparación del conocimiento para el testing.  
El AI-driven testing hace referencia a enfoques en los que modelos de IA intervienen en la generación, priorización, análisis o mantenimiento de pruebas. A diferencia de las aproximaciones tradicionales, aquí la IA participa en tareas que requieren cierto grado de interpretación y adaptación.  

Una de las aplicaciones más relevantes de la IA en testing es la generación automática o asistida de casos de prueba a partir de requisitos, historias de usuario o documentación funcional. Esta capacidad permite reducir parte del esfuerzo manual y aumentar la velocidad con la que se construyen escenarios iniciales de validación.  

Sin embargo, la calidad del resultado depende de cómo se interprete la entrada, de la consistencia del modelo empleado y del grado de control que se mantenga sobre la transformación. Por ello, la generación automática no debe entenderse como un proceso completamente trivial o garantizado, sino como una línea de apoyo que requiere diseño y validación.  

## Los agentes de IA

Estos representan una evolución respecto al uso de modelos generativos.  
Un agente no solo produce una respuesta de texto, sino que percibe la información, toma decisiones, utiliza herramientas y ejecuta acciones que están orientadas a un objetivo concreto.  
Esto hace que los agentes sean perfectos para procesos con varias etapas conectadas entre sí. En este trabajo, el análisis de documentación, la transformación a Gherkin y la integración con una API son tareas que encajan muy bien en una arquitectura de este tipo.  

## Definición de agente

Un agente puede definirse como una entidad software que percibe información de su entorno, la procesa, toma decisiones y ejecuta acciones en función de un objetivo.  
En términos prácticos, un agente puede combinar un modelo de lenguaje con instrucciones, herramientas, memoria y mecanismos de control, lo que le permite abordar tareas más complejas que una simple generación de texto.  

## Capacidades de percepción, decisión y acción

Las capacidades fundamentales de un agente pueden resumirse en percibir, decidir y actuar. Percibir implica recoger información relevante del entorno o de los datos de entrada. Decidir supone interpretarla y seleccionar una estrategia adecuada. Actuar significa ejecutar una operación útil sobre una herramienta, un sistema o un flujo de trabajo.  
Estas capacidades son especialmente valiosas cuando se desea automatizar procesos encadenados. En este caso, por ejemplo, permiten pasar de un documento de entrada a una salida estructurada y utilizable dentro de una herramienta de gestión de pruebas.  

## Diferencia entre IA generativa e IA agentica

La IA generativa tradicional responde a una entrada mediante la producción de una salida, normalmente textual.  
La IA agentica dgamos que incorpora objetivos, uso de herramientas, pasos intermedios y la capacidad de verificación o control del flujo. Por ello, esta última resulta más adecuada para procesos donde se necesita algo más que una respuesta aislada.  

Esta distinción es relevante porque la propuesta del TFG no se limita a pedir al modelo que por ejemplo “redacte” escenarios, sino que requiere una arquitectura capaz de procesar los documentos, estructurar esa información, transformarla y registrarla en una herramienta externa.  
Una característica clave de los agentes es su capacidad para utilizar herramientas externas, como APIs, sistemas de almacenamiento, navegadores o conectores. 

En la propuesta de este TFG, esta capacidad es clave, ya que la integración con Kiwi TCMS mediante API constituye una parte central del flujo. Sin uso de herramientas, la solución quedaría limitada a una simple generación documental sin conexión operativa. 

Por otra parte, para operar de forma coherente, un agente necesita gestionar el contexto, el estado del proceso y, en ciertos casos, mecanismos de memoria. Esto le permite mantener consistencia entre etapas, recordar información relevante y actuar de forma alineada con el objetivo definido.  

En sistemas donde varias tareas se enlazan entre sí, esta gestión resulta especialmente importante. Analizar un documento, estructurar requisitos y convertirlos en escenarios exige conservar relación entre la información original y la salida generada.  

## Sistemas multiagente

Cuando un único agente no es suficiente para abordar todas las tareas necesarias, puede recurrirse a un sistema multiagente. 

Es decir, varios agentes que están programados para realizar tareas concretas colaboran entre sí para resolver un problema común, repartiéndose funciones y manteniendo una coordinación explícita.  

Un sistema multiagente puede definirse como una arquitectura compuesta por varios agentes que colaboran, se coordinan y comparten información para resolver un problema.  

## Coordinación entre agentes

En cuanto a la coordinación no basta con tener varios componentes especializados; se necesita establecer cómo se comunican, en qué orden actúan, qué información comparten y cómo se asegura la coherencia del proceso.  
En la práctica, esta coordinación puede realizarse con un componente orquestador o con reglas de flujo definidas.  

## Especialización de roles

Es una gran ventaja, ya que puede existir un agente centrado en analizar documentos, otro en estructurar requisitos, otro en transformar la información a Gherkin y otro en integrarla con una API externa a través de herramientas.  

## Soluciones tradicionales de automatización y testing

### 1. Selenium

Selenium se ha consolidado como una de las referencias históricas en automatización de navegadores. Su principal fortaleza es la flexibilidad y el amplio ecosistema que lo rodea, lo que lo convierte en una herramienta muy extendida para pruebas web.  
Este requiere de un esfuerzo notable de configuración y mantenimiento.  
Pero eso hace que, no resuelva por sí mismo los problemas de análisis documental y estructuración de escenarios.  

### 2. Cypress

Cypress es una herramienta moderna orientada a pruebas end-to-end y de componentes en aplicaciones web. Destaca por su integración con el entorno de desarrollo, su facilidad de uso y su capacidad para observar la ejecución de pruebas de forma visual. 

En el contexto de CIC, Cypress forma parte del ecosistema real de trabajo. En el presente TFG no se llegará a implementar, pero si que se quiere implementar a nivel de empresa.  

### 3. JUnit y TestNG

Son marcos de prueba ampliamente utilizados en el ecosistema Java para la definición y ejecución de pruebas unitarias e integradas. Aportan estructura, anotaciones y capacidad de organización, siendo herramientas muy habituales en procesos de desarrollo con fuerte componente backend.  

### 4. Playwright

Playwright ha ganado relevancia por su enfoque moderno de automatización web, su soporte para múltiples navegadores y sus capacidades de depuración y espera automática. 

Estas características lo convierten en una opción muy atractiva para pruebas sobre interfaces dinámicas. 

Aun así, como ocurre con otras herramientas de automatización, su foco principal está en la ejecución de pruebas, no en la fase previa de interpretación documental y construcción estructurada de escenarios.  

### 5. Kiwi TCMS

Kiwi TCMS es una herramienta de gestión de pruebas de código abierto que permite definir casos de prueba, organizarlos en planes de prueba, registrar sus ejecuciones y mantener la trazabilidad de todo el proceso de validación. Dentro de este contexto, un scenario es una manera de describir un caso de prueba como una situación concreta que se quiere comprobar en el sistema. 

Para que esa descripción sea más clara, natural y fácil de seguir, suele utilizarse la estructura Given / When / Then / And, propia del enfoque BDD (Behavior Driven Development). Given define el contexto inicial o las condiciones previas, When indica la acción que realiza el usuario o el sistema, y Then expresa el resultado esperado. La palabra And permite añadir información o pasos adicionales sin romper la estructura. Esta forma de redactar los casos de prueba hace que sean mucho más comprensibles, tanto para perfiles de testing como para desarrollo, y facilita una comunicación más clara entre todos los implicados.


Su interés en este trabajo es especialmente alto porque puede actuar como punto de destino de los scenarios generados por el sistema multiagente. Además, el hecho de contar con API facilita su integración con arquitecturas basadas en herramientas y automatización, lo que la convierte en un elemento especialmente adecuado para la propuesta concreta de este TFG.  

Digamos para detallar mas, uno de los agentes se encarga de conectarse con la API de Kiwi TCMS para insertar automáticamente la información generada en la herramienta (A partir de los escenarios, este agente crea los casos de prueba dentro de la plataforma)

Así la solución no se limita a generar texto de forma aislada, sino que transforma ese output en elementos útiles dentro del proceso de QA. Esto permite mantener la información organizada para que el equipo de QA lo utilice.

## Herramientas de testing basadas en IA

### 1. Testim

Testim es una plataforma orientada a la automatización de pruebas con capacidades de IA que ayudan a mejorar la estabilidad y reducir el mantenimiento. 

Su propuesta se centra en absorber parte del impacto de cambios frecuentes en la interfaz.  

### 2. Applitools

Applitools se orienta a la detección de regresiones visuales mediante Visual AI, permitiendo identificar diferencias perceptibles para el usuario en distintos navegadores y dispositivos. 

Este enfoque resulta especialmente potente en validaciones visuales complejas.  

### 3. Mabl

Mabl busca unificar creación, ejecución y análisis de pruebas web y API dentro de entornos guiados, incorporando capacidades de IA para hacer el proceso más asistido. 

Su enfoque facilita determinadas tareas operativas y de mantenimiento.  

### 4. Functionize

Functionize plantea una automatización empresarial apoyada en IA y capacidades cercanas a enfoques agénticos para acelerar la creación, mantenimiento y ejecución de flujos end-to-end.  

## Frameworks para el desarrollo de agentes

### 1. Google ADK

Google ADK es un framework orientado al desarrollo de agentes de IA, con capacidad para construir soluciones modulares, estructuradas y orientadas a procesos. Su interés está en la facilidad para crear arquitecturas en las que varios componentes colaboran dentro de un flujo definido. En su documentación se explica todo muy detalladamente y es fácil de seguir.  

### 2. LangGraph

LangGraph se orienta a construir aplicaciones stateful y multi-actor basadas en grafos, con especial atención a la persistencia, la orquestación y la trazabilidad de flujos largos. Su diseño resulta interesante para procesos en los que el estado y la secuencia de los pasos son críticos.  

### 3. AutoGen

AutoGen ha sido una referencia importante en el desarrollo de aplicaciones basadas en conversaciones entre agentes y en la experimentación con arquitecturas colaborativas.  

Sin embargo, este framework en concreto dependiendo del tipo de solución, puede resultar menos alineado que otros enfoques más orientados a procesos.  

### 4. CrewAI

CrewAI se centra en la orquestación de agentes colaborativos y flujos complejos, ofreciendo abstracciones de alto nivel para modelar equipos de agentes con roles definidos. Esta orientación resulta útil cuando se desea construir sistemas con separación clara de responsabilidades.  

| Criterio | Google ADK | LangGraph | AutoGen | CrewAI |
|---|---|---|---|---|
| Soporte multiagente | SI | SI | SI | SI |
| Flujos estructurados / orientados a proceso | SI | SI | NO | SI |
| Gestión fuerte de estado / persistencia | NO | SI | NO | SI |
| Enfoque principalmente conversacional | NO | NO | SI | NO |
| Encaje directo con este TFG | SI | SI | NO | SI |

## Justificación
En primer lugar, se puede decir que las soluciones tradicionales de automatización de testing no resuelven de forma completa el problema planteado.

El problema de este TFG está orientado al hecho de como construir las pruebas. El análisis de la documentación que te dan los Project Managers, la extracción de requisitos y casos de uso relevantes y su transformación a otro lenguaje son tareas con una elevada carga. 

Por eso el enfoque se centra en asistir en la comprensión, estructuración y preparación de la información necesaria para el testing. 

El hecho de aplicar Inteligencia Artificial aporta un gran valor en este punto, ya que permite intervenir en procesos como la interpretación de documentación, la estructuración de la información de esos documentos y la generación de contenido que es muy buena para la parte de QA. En particular, la posibilidad de generar casos de prueba a partir de requisitos o documentación funcional supone una gran ventaja a la hora de reducir parte del esfuerzo manual y acelerar la construcción de los casos de prueba. 

Por otra parte, hay que tener en cuenta que la calidad del resultado depende de la forma en que el agente interprete la entrada, de la consistencia del modelo utilizado y del grado de control que se mantenga.

Por ello, la propuesta no puede apoyarse únicamente en un uso aislado de IA generativa. Ya que el objetivo del trabajo no consiste simplemente en solicitar a un modelo que redacte escenarios, sino en construir esa arquitectura capaz de procesar documentos, estructurar la información, transformarla a un formato como Gherkin,etc. Esto exige que exista la capacidad de decisión, y  el uso de herramientas por parte de los agentes.

En este trabajo, la conexión que hacen los agentes con Kiwi TCMS mediante su API constituye una parte clave dentro del flujo, ya que la utilidad práctica esta en transformar esa salida en un recurso operativo dentro del proceso real de pruebas. 

Sin esta integración, la solución queda reducida a un ejercicio de generación textual al que ya esta acostumbrado prácticamente todo el mundo.

Otro aspecto que justifica la propuesta es la necesidad de mantener coherencia y trazabilidad entre las distintas fases del proceso. El hecho de analizar la documentación, estructurar requisitos, transformarlos en escenarios y registrarlos posteriormente en una herramienta exige mantener la relación entre la información original y la salida generada. Por tanto esta necesidad de gestionar el contexto, el estado y la consistencia hace que una arquitectura multiagente resulte muy adecuada para el problema que se aborda.

En cuanto al análisis de las herramientas existentes se puede afirmar que ninguna de ellas cubre de forma completa todas las necesidades del trabajo. 
Herramientas como Selenium, Cypress, JUnit, TestNG o Playwright son muy útiles en la automatización y ejecución de pruebas, pero no resuelven la fase previa de análisis documental y construcción estructurada de escenarios. 

Por su parte, plataformas de testing basadas en IA, como Testim, Applitools, Mabl o Functionize, aportan capacidades avanzadas de asistencia y automatización, pero no están necesariamente orientadas a lo que se plantea en este trabajo, ni tampoco garantizan una adaptación directa al contexto del proyecto. 

Finalmente, la elección de Google ADK se debe a que en un principio dentro de la empresa se realizaron pruebas con este framework y dio buenos resultados, y puedo destacar que Google no llega a ser partnership de CIC pero le ofrece soporte de primer nivel en caso de necesitar ayuda, por tanto, el uso de esta herramienta encaja perfectamente. 

Si que es cierto que se podría usar otros frameworks como LangGraph pero debido a estas causas se utiliza Google ADK. A partir de esta decisión, se puede decir que Google ADK ofrece unas capacidades muy apropiadas que permiten construir la arquitectura multiagente y también flujos estructurados, por lo que toda esta suma de ventajas lo hace el framework mas óptimo para usar en este trabajo.
