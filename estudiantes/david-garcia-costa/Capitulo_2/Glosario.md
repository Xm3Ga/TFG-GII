# Glosario

### Proyecto

Entidad que representa un contexto organizativo o funcional al que pertenece una documentación.
Permite agrupar documentación, casos de uso, requisitos funcionales y escenarios de prueba.

### Documentación

Conjunto de información que describe el sistema a desarrollar o analizar.
Es el punto de entrada del sistema y contiene los elementos necesarios para el procesamiento.

### DRF (Documento de Requisitos Funcionales)

Tipo de documentación que describe las funcionalidades que debe cumplir el sistema desde el punto de vista del usuario.

### DDS (Documento de Diseño del Sistema)

Tipo de documentación que describe la arquitectura, componentes y diseño técnico del sistema.

### Caso de Uso

Descripción de una interacción entre un actor y el sistema para lograr un objetivo concreto.
Define cómo se utiliza el sistema desde la perspectiva del usuario.

### Requisito Funcional

Especificación de una funcionalidad que el sistema debe cumplir.
Describe qué debe hacer el sistema.

### Escenario de Prueba

Descripción concreta de una situación que permite verificar el comportamiento del sistema.
Se deriva de uno o varios casos de uso y requisitos funcionales.

### Escenario Gherkin

Representación de un escenario de prueba mediante el formato: Given / And / When / Then

Es una forma de expresar escenarios de prueba.
### Agente de IA

Componente del sistema encargado de realizar tareas automáticas como:

análisis de documentación
extracción de información
estructurar casos de uso y requisitos funcionales
generación de escenarios
publicar escenarios

No es un actor, sino parte interna del sistema.

- #### Análisis de documentación

Proceso mediante el cual el sistema interpreta la documentación para extraer información relevante.

- #### Estructuración

Proceso mediante el cual el sistema organiza los casos de uso y requisitos funcionales extraídos.

- #### Generación de escenarios

Proceso automático que realiza un agente mediante el cual se crean escenarios de prueba a partir de los casos de uso y requisitos funcionales.

- #### Revisión de escenarios

Proceso en el que el Ingeniero QA analiza los escenarios generados antes de su uso o publicación.

### Project Manager

Responsable de generar o gestionar la documentación, definir casos de uso y definir requisitos funcionales y que el proyecto vaya bien.

### Ingeniero QA

Analiza la documentación, también valida los resultados generados por los agentes de Inteligencia Artificial.

Es el usuario principal del sistema en la fase de testing ayudado por los agentes de Inteligencia Artificial.

### Kiwi TCMS

Sistema externo de gestión de pruebas.
Recibe y almacena los escenarios de prueba generados por el sistema.

- #### API de Kiwi TCMS

Interfaz utilizada para enviar escenarios de prueba desde el sistema hacia Kiwi TCMS.

- #### Publicación en Kiwi TCMS

Proceso mediante el cual los escenarios generados son enviados al sistema externo para su almacenamiento y gestión.