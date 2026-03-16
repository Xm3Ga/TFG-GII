# 1. INTRODUCCIÓN

## Motivación

En este Trabajo de Fin de Grado se propone diseñar e implementar un conjunto de flujos de seguridad para agentes de inteligencia artificial, integrados en una plataforma B2B de gobernanza de agentes agnóstica de proveedor, actualmente en desarrollo dentro de una *startup* tecnológica.

La motivación surge de una necesidad real identificada en el entorno empresarial actual. Las organizaciones están desplegando cada vez más agentes de IA en sus operaciones, a menudo distribuidos en múltiples proveedores como Amazon Web Services, Microsoft Azure, Google Cloud u OpenAI. Gestionar la seguridad de todos estos agentes de forma individual, con las herramientas propias de cada proveedor, se ha convertido en un problema creciente que ninguna solución del mercado resuelve de forma integral.

## Problemática identificada

En términos más específicos, las empresas que operan con múltiples agentes de IA se enfrentan a las siguientes limitaciones:

- **Fragmentación de la seguridad**: cada proveedor ofrece sus propias herramientas de seguridad, que no son interoperables entre sí. No existe una forma unificada de aplicar políticas de seguridad transversales.
- **Ausencia de controles de privilegios**: los agentes de IA suelen desplegarse con permisos excesivos, sin un análisis sistemático de los privilegios mínimos que realmente necesitan para operar.
- **Falta de supervisión humana**: las operaciones de alto riesgo ejecutadas por agentes autónomos (como procesamiento de pagos o modificación de datos críticos) carecen, en muchos casos, de mecanismos de aprobación humana.
- **Complejidad técnica**: las soluciones de seguridad existentes requieren conocimientos avanzados para su configuración, lo que dificulta su adopción por equipos no especializados.

## Propuesta de solución

Para abordar este problema, el presente trabajo propone el diseño y desarrollo de misiones de seguridad dentro de la plataforma de gobernanza de agentes de IA. Las misiones son flujos conversacionales guiados mediante los cuales el usuario interactúa con el agente de la plataforma para configurar y aplicar políticas de seguridad sobre sus agentes.

La solución se estructura en torno a tres conjuntos de misiones: creación y aplicación de guardarraíles, cumplimiento del principio de mínimo privilegio, y definición de niveles de autonomía con supervisión humana. Estos componentes se detallarán en secciones posteriores.

# 2. MARCO TEÓRICO

## Estado del arte

### Contexto empresarial: la plataforma de gobernanza de agentes de IA

El presente TFG se desarrolla en el contexto de una *startup* tecnológica que está construyendo una plataforma B2B (*Business-to-Business*) para el manejo y la gobernanza de agentes de inteligencia artificial. La plataforma se concibe como un punto único de control, agnóstico de proveedor, que permite a las empresas gestionar todos sus agentes de IA desde una misma interfaz, independientemente de si están desplegados en Amazon Bedrock, Azure OpenAI Service, Google Cloud Vertex AI u otros proveedores.

La plataforma se articula en torno a cuatro ejes: visibilidad, cumplimiento, seguridad y trazabilidad. Su funcionamiento se basa en un sistema de **misiones**, que son flujos guiados en los que el cliente dialoga con el agente de la plataforma, y este le orienta paso a paso a través del proceso que corresponda.

La primera misión que todo cliente debe completar es la de **conexión**: el agente de la plataforma guía al usuario para crear cuentas de servicio o proveer credenciales, de forma que la plataforma pueda integrarse con su infraestructura y descubrir sus agentes desplegados. A partir de ahí se desbloquean misiones de diversa índole: seguimiento de costes, versionado, trazabilidad y, en particular, las misiones de seguridad que constituyen el objeto de este TFG.

En el momento de inicio de este trabajo, la plataforma se encuentra en fase pre-MVP (*Minimum Viable Product*). Las misiones de seguridad que se proponen formarán parte del primer conjunto funcional del producto.

### Solución existente aproximada

En el ámbito de la seguridad para agentes de IA, la solución más próxima al enfoque de este trabajo es **Amazon Bedrock Guardrails** (Amazon Web Services [AWS], 2024). Se trata de un servicio que permite definir políticas de filtrado de contenido, bloqueo de temas sensibles, detección de información de identificación personal (PII) y protección frente a inyecciones de *prompts*.

El interés de esta solución reside en que plantea un modelo de guardarraíles configurables que permite controlar el comportamiento de los agentes. El administrador puede definir qué temas están permitidos, qué tipo de información no debe aparecer en las respuestas y qué patrones de entrada deben ser bloqueados. Esto supone un avance significativo respecto a las aproximaciones anteriores, donde la seguridad dependía exclusivamente de las instrucciones del *prompt* del sistema.

Sin embargo, esta solución presenta limitaciones importantes para el problema que se aborda en este TFG. En primer lugar, funciona exclusivamente dentro del ecosistema de Amazon Bedrock; no es posible aplicar estas políticas sobre agentes desplegados en Azure, Google Cloud u otros proveedores. En segundo lugar, no aborda aspectos como la gestión de privilegios de los agentes ni la definición de niveles de autonomía con supervisión humana. Y en tercer lugar, su configuración se realiza mediante consola técnica, sin un flujo conversacional guiado que facilite la adopción por parte de equipos con distintos niveles de competencia técnica.

Por tanto, Amazon Bedrock Guardrails puede considerarse como una solución relevante, especialmente por sus capacidades de filtrado de contenido y detección de PII. Pero su alcance se limita a un único proveedor y a un único aspecto de la seguridad, mientras que la propuesta de este TFG busca una cobertura multi-proveedor e integral.

## Marco teórico

### Seguridad en sistemas de inteligencia artificial

El despliegue de sistemas de inteligencia artificial en entornos empresariales ha generado un nuevo conjunto de riesgos de seguridad que difiere significativamente de los riesgos tradicionales en ingeniería del software. Mientras que la seguridad convencional se centra en proteger sistemas frente a accesos no autorizados, inyecciones de código o vulnerabilidades en la infraestructura, la seguridad en sistemas de IA debe abordar además amenazas propias de los modelos de lenguaje y los agentes autónomos (OWASP, 2025).

El informe OWASP Top 10 para Aplicaciones de LLM, en su versión 2025, identifica los diez riesgos más críticos para este tipo de sistemas. Entre ellos, tres resultan directamente relevantes para el presente trabajo:

- **LLM01 — Inyección de *prompts***: manipulación de modelos de lenguaje mediante entradas diseñadas para alterar su comportamiento, eludir restricciones o ejecutar acciones no previstas (OWASP, 2025). Es uno de los ataques más documentados en los últimos años. Perez y Ribeiro (2022) demostraron que incluso modelos con instrucciones de seguridad pueden ser manipulados mediante técnicas de inyección directa e indirecta. Greshake et al. (2023) extendieron este análisis a aplicaciones integradas con LLMs, mostrando cómo las inyecciones indirectas a través de fuentes externas pueden comprometer sistemas reales.
- **LLM02 — Divulgación de información sensible**: exposición no autorizada de datos personales, credenciales o información confidencial a través de las respuestas generadas por el modelo (OWASP, 2025). Esto incluye la filtración de información de identificación personal (PII), un aspecto especialmente crítico en el contexto del Reglamento General de Protección de Datos (RGPD) europeo.
- **LLM06 — Agencia excesiva**: concesión de niveles de autonomía desproporcionados a agentes de IA, sin los controles adecuados sobre las acciones que pueden ejecutar (OWASP, 2025). Este riesgo se materializa cuando un agente tiene acceso a herramientas, APIs o recursos que exceden lo necesario para su función.

### Guardarraíles para agentes de IA

El concepto de *guardarraíles* (*guardrails*) en inteligencia artificial hace referencia a los mecanismos de control que restringen el comportamiento de un modelo o agente dentro de unos límites predefinidos. Pueden operar a distintos niveles:

- **A nivel de entrada**: filtrando *prompts* maliciosos, detectando intentos de inyección o bloqueando solicitudes sobre temas restringidos.
- **A nivel de salida**: verificando que las respuestas generadas no contengan información sensible, contenido inapropiado o datos que vulneren las políticas establecidas.
- **A nivel de acción**: limitando las operaciones que un agente puede ejecutar de forma autónoma, como llamadas a APIs externas, escritura en bases de datos o procesamiento de transacciones.

Los principales proveedores de servicios de IA en la nube han desarrollado sus propias implementaciones. Además de Amazon Bedrock Guardrails, ya mencionado, **Azure AI Content Safety** (Microsoft, 2024) ofrece moderación de contenido mediante modelos especializados en la detección de contenido dañino y discurso de odio, limitado al ecosistema de Azure. **Google Cloud Vertex AI** (Google Cloud, 2024) incorpora filtros de seguridad configurables para sus modelos generativos, restringidos a Google Cloud Platform.

En el ámbito del código abierto, **NVIDIA NeMo Guardrails** (NVIDIA, 2024) permite añadir guardarraíles programáticos a aplicaciones basadas en LLM mediante un lenguaje de modelado de diálogo llamado Colang. Su enfoque es flexible pero requiere conocimientos técnicos avanzados. **Guardrails AI** (Guardrails AI, 2024) permite definir validadores para entradas y salidas de modelos, centrándose en la validación a nivel de aplicación individual.

### Principio de mínimo privilegio en sistemas de IA

El principio de mínimo privilegio es un concepto fundamental en seguridad informática que establece que una entidad (usuario, proceso o, en este caso, agente de IA) solo debe disponer de los permisos estrictamente necesarios para realizar su función (Saltzer & Schroeder, 1975). En el contexto de los agentes de IA, este principio cobra especial relevancia porque los agentes suelen desplegarse con acceso a herramientas, APIs y recursos que exceden sus necesidades operativas reales.

Un agente diseñado para responder consultas de clientes no necesita, por ejemplo, acceso de escritura a bases de datos de producción ni permisos para ejecutar transacciones financieras. Sin embargo, la práctica habitual en muchas organizaciones es conceder permisos amplios por conveniencia, lo que incrementa la superficie de ataque y el impacto potencial de una vulnerabilidad.

La aplicación sistemática de este principio a agentes de IA requiere un análisis de las tareas que cada agente ejecuta, una identificación de los permisos mínimos necesarios y la eliminación del exceso de privilegios. Este proceso, cuando se realiza manualmente sobre decenas o cientos de agentes, resulta inviable en la práctica.

### Autonomía y supervisión humana

La autonomía de un agente de IA define el grado en que este puede tomar decisiones y ejecutar acciones sin intervención humana. El concepto de *human-in-the-loop* (HITL) plantea la necesidad de incluir puntos de supervisión humana en los procesos automatizados, especialmente cuando las acciones del agente conllevan riesgos significativos (Shneiderman, 2022).

No todas las tareas tienen el mismo nivel de riesgo. Una consulta informativa tiene un impacto bajo si el agente comete un error, mientras que un procesamiento de pago, una modificación de datos personales o una decisión que afecta a la seguridad de un sistema requieren un nivel de supervisión mayor. La definición de niveles de autonomía por agente permite establecer umbrales diferenciados: tareas de bajo riesgo pueden ejecutarse de forma autónoma, mientras que las de alto riesgo requieren aprobación humana antes de su ejecución.

El Reglamento de Inteligencia Artificial de la Unión Europea (*EU AI Act*) refuerza esta necesidad al exigir supervisión humana efectiva para los sistemas de IA clasificados como de alto riesgo (Parlamento Europeo, 2024).

### Agentes de IA y sistemas agénticos

Un agente de IA puede definirse como una entidad software que percibe información de su entorno, la procesa, toma decisiones y ejecuta acciones orientadas a un objetivo (Russell & Norvig, 2021). A diferencia de un modelo generativo convencional, que produce una respuesta de texto ante una entrada, un agente combina un modelo de lenguaje con instrucciones, herramientas, memoria y mecanismos de control que le permiten abordar tareas más complejas.

La diferencia entre IA generativa e IA agéntica resulta relevante para este trabajo. La IA generativa tradicional responde a una entrada mediante la producción de una salida, normalmente textual (Wang et al., 2024). La IA agéntica incorpora el uso de herramientas, pasos intermedios y capacidad de verificación o control del flujo. Esta distinción es importante porque las misiones de seguridad que se proponen no se limitan a generar recomendaciones de texto: requieren que el agente de la plataforma analice configuraciones reales de los agentes del cliente, interactúe con APIs de distintos proveedores y aplique cambios efectivos sobre la infraestructura.

Una capacidad fundamental de los agentes es el uso de herramientas externas, como APIs, sistemas de almacenamiento o conectores con plataformas de terceros. En la propuesta de este TFG, esta capacidad es esencial, ya que las misiones de seguridad deben interactuar con las APIs de los distintos proveedores de IA para analizar y modificar la configuración de los agentes del cliente.

### Plataformas de seguridad y observabilidad de IA

Más allá de los guardarraíles de los proveedores *cloud*, han surgido plataformas especializadas en la seguridad de sistemas de IA:

**Lakera Guard** (Lakera, 2024) se especializa en la detección de inyecciones de *prompts* y la protección en tiempo real de aplicaciones basadas en LLM, actuando como un *firewall* para modelos de lenguaje. Ofrece protección robusta frente a ataques de inyección, pero no aborda la gestión de privilegios ni la definición de niveles de autonomía.

**Protect AI** (Protect AI, 2024) ofrece seguridad para el ciclo de vida completo de la IA, incluyendo escaneo de vulnerabilidades y análisis de la cadena de suministro de modelos. Su enfoque se orienta más hacia la seguridad del *pipeline* de *machine learning* que hacia la gobernanza operativa de agentes en producción.

**Arthur AI** (Arthur AI, 2024) proporciona monitorización y observabilidad para modelos de IA en producción. Su propuesta se centra en la monitorización pasiva y la generación de alertas, sin mecanismos activos de aplicación de políticas.

**Calypso AI** (Calypso AI, 2024) ofrece pruebas y evaluación de seguridad para modelos de IA, incluyendo pruebas adversariales y evaluación del cumplimiento normativo. Se posiciona como herramienta de *testing* y auditoría, complementaria pero no sustitutiva de los mecanismos de protección en tiempo de ejecución.

### Marcos regulatorios y estándares

El panorama regulatorio en torno a la inteligencia artificial ha evolucionado significativamente, generando un marco normativo que las organizaciones deben cumplir:

El **Reglamento de Inteligencia Artificial de la UE** (*EU AI Act*) (Parlamento Europeo, 2024) clasifica los sistemas de IA en categorías de riesgo y establece requisitos específicos para cada una. Los sistemas de alto riesgo deben implementar gestión de riesgos, supervisión humana, documentación técnica y estándares de transparencia.

La norma **ISO/IEC 42001:2023** (ISO, 2023) establece los requisitos para un Sistema de Gestión de Inteligencia Artificial, proporcionando un marco estructurado que incluye controles de seguridad, privacidad y gestión de riesgos.

El **NIST AI Risk Management Framework** (NIST, 2023) proporciona directrices voluntarias organizadas en cuatro funciones: gobernar, mapear, medir y gestionar. Enfatiza la supervisión humana y la transparencia en sistemas de IA autónomos.

Estas regulaciones refuerzan la necesidad de mecanismos sistemáticos de seguridad para agentes de IA, especialmente en entornos empresariales con múltiples proveedores.

## 2.1. Justificación

A partir del análisis realizado, se puede afirmar que ninguna de las soluciones existentes cubre de forma completa el problema planteado.

Las soluciones de guardarraíles de los proveedores *cloud* (Amazon Bedrock Guardrails, Azure AI Content Safety, Google Vertex AI) son las más maduras en cuanto a funcionalidad, pero presentan una limitación fundamental: solo operan dentro de sus respectivos ecosistemas. Una empresa que utilice agentes en múltiples proveedores debe configurar y gestionar la seguridad de forma independiente en cada uno, sin posibilidad de aplicar políticas unificadas. Esto genera fragmentación y dificulta la auditoría y el cumplimiento normativo.

Las plataformas de seguridad especializadas (Lakera Guard, Protect AI, Arthur AI, Calypso AI) aportan capacidades avanzadas, pero cada una se centra en un aspecto concreto de la seguridad. Ninguna aborda de forma integrada los tres ejes que este trabajo propone: guardarraíles, gestión de privilegios y control de autonomía.

Las herramientas de código abierto (NeMo Guardrails, Guardrails AI) ofrecen flexibilidad, pero requieren conocimientos técnicos avanzados para su configuración y no proporcionan una experiencia guiada que facilite la adopción.


| Característica                    | Bedrock Guardrails | Azure Content Safety | NeMo Guardrails | Lakera Guard | Protect AI |
| --------------------------------- | ------------------ | -------------------- | --------------- | ------------ | ---------- |
| Agnóstico de proveedor            | No                 | No                   | Parcial         | Parcial      | Parcial    |
| Guardarraíles de contenido        | Sí                 | Sí                   | Sí              | Parcial      | No         |
| Detección de PII                  | Sí                 | No                   | No              | No           | No         |
| Bloqueo de inyección de *prompts* | Sí                 | No                   | Parcial         | Sí           | No         |
| Gestión de privilegios            | No                 | No                   | No              | No           | No         |
| Definición de autonomía           | No                 | No                   | No              | No           | No         |
| Interfaz conversacional guiada    | No                 | No                   | No              | No           | No         |


*Tabla 1. Análisis comparativo de soluciones existentes. Elaboración propia.*

En la tabla se evidencia que ninguna solución ofrece gestión de privilegios, definición de autonomía ni una interfaz conversacional guiada. Además, la característica de ser agnóstico de proveedor, que resulta fundamental para el contexto empresarial descrito, solo se consigue de forma parcial con las herramientas de código abierto o las plataformas de seguridad independientes.

Por todo ello, se justifica la necesidad de desarrollar una solución específica que, integrada en una plataforma de gobernanza agnóstica de proveedor, permita abordar la seguridad de los agentes de IA de forma centralizada, integral y guiada.

## 2.2. Solución propuesta

Debido a todo lo expuesto en los apartados anteriores, la solución propuesta consiste en diseñar y desarrollar un conjunto de misiones de seguridad dentro de la plataforma de gobernanza de agentes de IA.

La solución se estructura en torno a tres conjuntos funcionales de misiones:

**Misiones de guardarraíles**: el cliente inicia la misión y dialoga con el agente de la plataforma para configurar mecanismos de protección sobre sus agentes de IA. Incluyen sub-misiones para definir el tono y estilo de respuesta, bloquear temas prohibidos, activar la protección frente a inyecciones de *prompts* y activar la detección y anonimización de PII.

**Misión de cumplimiento del mínimo privilegio**: una misión que analiza los permisos asignados a cada agente de IA del cliente, calcula el conjunto mínimo de privilegios necesarios para sus tareas y propone la eliminación del exceso de privilegios.

**Misión de definición de autonomía**: una misión que analiza el tipo de tarea que realiza cada agente, evalúa su nivel de riesgo y asigna un nivel de autonomía. Las operaciones de alto riesgo requerirán aprobación humana (*human-in-the-loop*) antes de su ejecución.

La propuesta se plantea como un MVP funcional, de forma que sea posible comprobar su viabilidad técnica y valorar su utilidad dentro de un contexto real. En el segundo capítulo se realizará el análisis de requisitos correspondiente, con su modelo de dominio y disciplina de requisitos.

## 2.3. Objetivos general y específicos

### Objetivo general

El objetivo general de este Trabajo de Fin de Grado es diseñar, desarrollar e implementar un conjunto de misiones de seguridad para una plataforma B2B de gobernanza de agentes de IA que permita a las organizaciones configurar guardarraíles, gestionar privilegios y definir niveles de autonomía sobre sus agentes, de forma centralizada y agnóstica de proveedor.

### Objetivos específicos

Para alcanzar este objetivo general, se plantean los siguientes objetivos específicos:

1. Elaborar el modelo de dominio y definir la disciplina de requisitos de las misiones de seguridad.
2. Diseñar la arquitectura y los componentes necesarios para que las misiones funcionen de manera coherente con la plataforma.
3. Desarrollar un prototipo funcional (MVP) que implemente las misiones de seguridad diseñadas.
4. Evaluar las aportaciones y limitaciones del prototipo desarrollado.

### Alcance del trabajo

El alcance de este TFG se concreta en los siguientes puntos:

1. Analizar y documentar (modelo de dominio y disciplina de requisitos) las misiones de seguridad.
2. Diseñar la arquitectura de las misiones de seguridad y su integración con la plataforma de gobernanza.
3. Implementar un MVP funcional que cubra los tres conjuntos de misiones descritos: guardarraíles, mínimo privilegio y definición de autonomía.
4. Validar el prototipo en un entorno controlado.
5. Evaluar los resultados obtenidos frente a los objetivos planteados.

En este trabajo se busca demostrar que un sistema de misiones conversacionales puede ofrecer una forma efectiva de securizar agentes de IA en entornos multi-proveedor. No se pretende cubrir la totalidad de las misiones de la plataforma (que incluyen también seguimiento de costes, versionado o trazabilidad), sino centrarse exclusivamente en el eje de seguridad.

La arquitectura propuesta se planteará de forma que pueda servir como base para futuros desarrollos dentro de la plataforma.

## 2.4. Estructura del trabajo

### Metodología — Proceso Unificado (RUP)

Se utilizará el Proceso Unificado (Jacobson et al., 1999) como marco metodológico. Es un *framework* de desarrollo de software iterativo, incremental y centrado en la arquitectura.

La elección de esta metodología se fundamenta en su adecuación al contexto del proyecto: el desarrollo se realiza dentro de una *startup* en fase temprana, donde los requisitos pueden evolucionar, y el enfoque iterativo permite incorporar cambios de forma controlada. Además, las disciplinas del Proceso Unificado (requisitos, análisis, diseño, implementación) se alinean de forma natural con los capítulos de este TFG.

### Estructura del proyecto

La metodología estructura el desarrollo en cuatro fases:

- **Inicio**: definición del problema, alcance, estado del arte y objetivos (Capítulo 1).
- **Elaboración**: modelo de dominio, disciplina de requisitos, actores, casos de uso y diagrama de contexto (Capítulo 2).
- **Construcción**: análisis y diseño, clases de análisis y diseño, arquitectura, diagramas de despliegue y paquetes (Capítulo 3).
- **Transición**: implementación del MVP, descripción de la solución, validación, conclusiones y líneas futuras (Capítulos 4 y 5).

# Referencias bibliográficas

Amazon Web Services. (2024). *Amazon Bedrock Guardrails*. [https://aws.amazon.com/bedrock/guardrails/](https://aws.amazon.com/bedrock/guardrails/)

Arthur AI. (2024). *Arthur AI: AI Performance Monitoring*. [https://www.arthur.ai/](https://www.arthur.ai/)

Calypso AI. (2024). *Calypso AI: AI Security and Testing Platform*. [https://calypsoai.com/](https://calypsoai.com/)

Google Cloud. (2024). *Vertex AI: Generative AI on Google Cloud*. [https://cloud.google.com/vertex-ai](https://cloud.google.com/vertex-ai)

Greshake, K., Abdelnabi, S., Mishra, S., Endres, C., Holz, T., & Fritz, M. (2023). Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection. *Proceedings of the 16th ACM Workshop on Artificial Intelligence and Security*, 79-90. [https://doi.org/10.1145/3605764.3623985](https://doi.org/10.1145/3605764.3623985)

Guardrails AI. (2024). *Guardrails AI: Input/Output Guards for LLMs*. [https://www.guardrailsai.com/](https://www.guardrailsai.com/)

ISO. (2023). *ISO/IEC 42001:2023 — Information technology — Artificial intelligence — Management system*. International Organization for Standardization. [https://www.iso.org/standard/81230.html](https://www.iso.org/standard/81230.html)

Jacobson, I., Booch, G., & Rumbaugh, J. (1999). *The Unified Software Development Process*. Addison-Wesley.

Lakera. (2024). *Lakera Guard: AI Security for LLM Applications*. [https://www.lakera.ai/](https://www.lakera.ai/)

McKinsey & Company. (2024). *The state of AI in early 2024: Gen AI adoption spikes and starts to generate value*. [https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai)

Microsoft. (2024). *Azure AI Content Safety*. [https://azure.microsoft.com/en-us/products/ai-services/ai-content-safety](https://azure.microsoft.com/en-us/products/ai-services/ai-content-safety)

NIST. (2023). *Artificial Intelligence Risk Management Framework (AI RMF 1.0)* (NIST AI 100-1). National Institute of Standards and Technology. [https://doi.org/10.6028/NIST.AI.100-1](https://doi.org/10.6028/NIST.AI.100-1)

NVIDIA. (2024). *NeMo Guardrails: Open-Source Toolkit for LLM Safety*. [https://github.com/NVIDIA/NeMo-Guardrails](https://github.com/NVIDIA/NeMo-Guardrails)

OWASP. (2025). *OWASP Top 10 for Large Language Model Applications 2025*. OWASP Foundation. [https://genai.owasp.org/llm-top-10/](https://genai.owasp.org/llm-top-10/)

Parlamento Europeo. (2024). *Reglamento (UE) 2024/1689 del Parlamento Europeo y del Consejo por el que se establecen normas armonizadas en materia de inteligencia artificial (Reglamento de Inteligencia Artificial)*. Diario Oficial de la Unión Europea. [https://eur-lex.europa.eu/eli/reg/2024/1689/oj](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)

Perez, F., & Ribeiro, I. (2022). Ignore This Title and HackAPrompt: Evaluating and Eliciting Prompt Injection Attacks in LLMs. *arXiv preprint arXiv:2211.09527*. [https://arxiv.org/abs/2211.09527](https://arxiv.org/abs/2211.09527)

Protect AI. (2024). *Protect AI: AI/ML Security Platform*. [https://protectai.com/](https://protectai.com/)

Russell, S., & Norvig, P. (2021). *Artificial intelligence: A modern approach* (4th ed.). Pearson.

Saltzer, J. H., & Schroeder, M. D. (1975). The protection of information in computer systems. *Proceedings of the IEEE, 63*(9), 1278-1308. [https://doi.org/10.1109/PROC.1975.9939](https://doi.org/10.1109/PROC.1975.9939)

Shneiderman, B. (2022). *Human-Centered AI*. Oxford University Press.

Wang, L., Ma, C., Feng, X., Zhang, Z., Yang, H., Zhang, J., Chen, Z., Tang, J., Chen, X., Lin, Y., Zhao, W. X., Wei, Z., & Wen, J. (2024). A Survey on Large Language Model based Autonomous Agents. *Frontiers of Computer Science, 18*(6), 186345. [https://doi.org/10.1007/s11704-024-40231-1](https://doi.org/10.1007/s11704-024-40231-1)