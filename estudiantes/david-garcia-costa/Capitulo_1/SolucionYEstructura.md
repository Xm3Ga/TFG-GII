## Metodología - Rational Unified Process (RUP)
Se utilizará RUP como marco metodológico, es un marco de trabajo (framework) para el desarrollo de software, iterativo, incremental y centrado en la arquitectura,

## Estructura del Proyecto 
La metodología que se va a utilizar (RUP) estructura el desarrollo en cuatro fases:

- Inicio: definición del problema y alcance.

- Elaboración: análisis, modelo de dominio y disciplina de requisitos.

- Construcción: diseño e implementación del prototipo controlado.

- Transición : validación y evaluación del prototipo.

---------------------------------------------------------

## Solución

El inicio de este proyecto comienza en este capítulo.
En en el segundo capítulo se realizará el análisis correspondiente a este proyecto con sus respectivo modelo de dominio y disciplina de requisitos.
Debido a todo lo comentado en los otros apartados, la solución propuesta consiste en diseñar una arquitectura basada en agentes de Inteligencia Artificial con ADK Google utilizando el modelo de OpenAI, para que apoye distintas fases del proceso de testing de QA de CIC Consulting Informático.

La idea central es estructurar el flujo de trabajo en tareas especializadas. 

Solución Propuesta:

Un agente puede encargarse de analizar el DRF (Documento Requisitos Funcionales) y el DDS (Documentos Diseño Sistema) y con ellos extraer requisitos o casos de uso relevantes; otro puede coger ese output y transformar esa información a casos de pruebas, como Gherkin (Scenarios (Given,When,And,Then)); y otro puede coger esos casos de prueba en dicho lenguaje para inyectarlos dentro KiwiTCMS a través de su API.

La propuesta se plantea como un prototipo controlado, de manera que sea posible comprobar su viabilidad técnica y valorar su utilidad dentro de un contexto real.
