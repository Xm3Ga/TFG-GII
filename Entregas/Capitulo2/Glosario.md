# Glosario

## Organización

Empresa cliente que utiliza la plataforma Horizon para gestionar la seguridad de sus agentes de IA. Una organización puede tener múltiples usuarios, credenciales y agentes desplegados en distintos proveedores cloud.

## Usuario

Persona perteneciente a una organización que interactúa con la plataforma Horizon. Inicia misiones, proporciona credenciales, define configuraciones de seguridad y confirma la aplicación de cambios sobre sus agentes.

## Proveedor Cloud

Servicio de computación en la nube que ofrece infraestructura para el despliegue y ejecución de agentes de IA. Los proveedores soportados son Amazon Web Services (AWS), Microsoft Azure y Google Cloud Platform (GCP). Cada proveedor expone sus propias APIs para la gestión de agentes y políticas de seguridad.

## Credencial

Conjunto de datos de autenticación (claves de acceso, cuentas de servicio o tokens) que permiten a la plataforma Horizon conectarse con un proveedor cloud en nombre de la organización cliente. Las credenciales se almacenan cifradas y se utilizan para ejecutar operaciones sobre la infraestructura del cliente.

## Agente de IA

Entidad software desplegada por la organización en un proveedor cloud que utiliza modelos de lenguaje para realizar tareas de forma autónoma o semi-autónoma. Cada agente tiene instrucciones personalizadas, herramientas asignadas y permisos que determinan su capacidad de acción.

## Misión

Flujo de trabajo guiado por el CAIO mediante una interfaz conversacional. Constituye la unidad funcional de la plataforma: el usuario inicia una misión, interactúa con el CAIO siguiendo los pasos definidos y obtiene un resultado (una política aplicada, un análisis completado o una configuración modificada).

## Misión de Guardarraíl

Tipo de misión de seguridad orientada a la configuración y aplicación de guardarraíles sobre los agentes de IA del cliente. Incluye sub-misiones para seguridad de contenido, protección anti-jailbreak, detección de PII, bloqueo de temas, control de idioma y verificación de coherencia (*grounding*).

## Misión de Mínimo Privilegio

Tipo de misión de seguridad que analiza los permisos asignados a los agentes de IA, identifica privilegios excesivos y propone la reducción al conjunto mínimo necesario para que cada agente pueda realizar sus tareas.

## Misión de Autonomía

Tipo de misión de seguridad que permite definir niveles de autonomía para los agentes de IA según el riesgo de las operaciones que realizan, y configurar mecanismos de supervisión humana (*human-in-the-loop*) para operaciones de alto riesgo.

## Guardarraíl

Política de seguridad configurable que restringe el comportamiento de un agente de IA dentro de unos límites predefinidos. Un guardarraíl puede contener una o varias políticas de distinto tipo (contenido, temas, PII, *grounding*) y se aplica a uno o varios agentes.

## Política de Contenido

Componente de un guardarraíl que define filtros para categorías de contenido dañino: odio, insultos, contenido sexual, violencia y conductas inapropiadas. Cada categoría tiene un nivel de severidad configurable (ninguno, bajo, medio, alto).

## Política de Temas

Componente de un guardarraíl que define los temas que un agente debe rechazar o no abordar en sus respuestas. Permite bloquear áreas temáticas completas que la organización considere fuera del alcance del agente.

## Política de PII

Componente de un guardarraíl que configura la detección y tratamiento de información de identificación personal (*Personally Identifiable Information*) en las entradas y salidas del agente. Permite identificar datos como nombres, direcciones, números de teléfono, correos electrónicos y documentos de identidad, y aplicar acciones de anonimización o bloqueo.

## Política de Grounding

Componente de un guardarraíl que verifica la coherencia y relevancia de las respuestas generadas por el agente respecto a la información de referencia proporcionada. Permite detectar respuestas inventadas o no fundamentadas (*hallucinations*).

## Permiso

Derecho de acceso o acción asignado a un agente de IA dentro de su proveedor cloud. Los permisos determinan qué operaciones puede ejecutar el agente (por ejemplo, lectura de bases de datos, llamadas a APIs externas, procesamiento de pagos). El principio de mínimo privilegio establece que un agente solo debe tener los permisos estrictamente necesarios para su función. Se implementa de forma distinta según el proveedor: políticas IAM en AWS, roles RBAC en Azure e IAM roles en GCP.

## Perfil de Uso

Registro histórico de las acciones realmente ejecutadas por un agente de IA durante un período determinado. Se construye a partir de los logs de actividad del proveedor cloud (CloudTrail en AWS, Activity Log en Azure, Cloud Audit Logs en GCP). Cada entrada del perfil asocia un permiso con su frecuencia de uso, permitiendo identificar qué permisos están siendo utilizados y cuáles no. Es el insumo principal para el análisis de privilegios.

## Análisis de Privilegios

Resultado del proceso de comparación entre los permisos asignados a un agente de IA y los permisos efectivamente utilizados, según su perfil de uso. El análisis produce un conjunto de recomendaciones de reducción. Puede tener distintos estados: pendiente (aún no iniciado), en recopilación (obteniendo datos del proveedor), completado (recomendaciones generadas) o sin datos suficientes (historial de uso insuficiente para generar recomendaciones fiables).

## Recomendación de Reducción

Propuesta concreta de eliminación de un permiso específico de un agente de IA, generada como resultado de un análisis de privilegios. Incluye la identificación del permiso excesivo, la justificación de por qué se considera innecesario (basada en el perfil de uso) y el impacto estimado de su eliminación. Cada recomendación puede ser aceptada, rechazada o aplazada por el usuario.

## Nivel de Autonomía

Clasificación que define el grado de independencia con el que un agente de IA puede operar. Se definen tres niveles:

- **Total**: el agente opera sin supervisión humana. Adecuado para agentes que solo realizan operaciones de bajo riesgo (búsquedas, consultas informativas, respuestas de soporte básico).
- **Supervisado**: las operaciones de alto riesgo requieren aprobación humana antes de ejecutarse, mientras que las de bajo riesgo se ejecutan de forma autónoma. Es el nivel recomendado para la mayoría de agentes empresariales.
- **Restringido**: todas las operaciones requieren aprobación humana. Adecuado para agentes en fase de pruebas o que operan con datos especialmente sensibles.

## Política de Autonomía

Configuración completa de autonomía para un agente de IA. Agrupa el nivel de autonomía asignado, las operaciones de riesgo identificadas, las reglas de supervisión definidas y los aprobadores asignados. Tiene un ciclo de vida propio: se crea como borrador, se activa cuando la configuración está completa, y puede suspenderse o revisarse periódicamente.

## Operación de Riesgo

Acción que un agente de IA es capaz de ejecutar y que ha sido clasificada según su nivel de impacto potencial. Las operaciones se asocian a una categoría de riesgo y a un nivel de severidad (bajo, medio, alto, crítico). Ejemplos: procesamiento de pagos (financiero, alto), lectura de datos de cliente (datos personales, medio), envío de correos electrónicos (comunicaciones, bajo), modificación de infraestructura (infraestructura, crítico).

## Categoría de Riesgo

Clasificación temática del tipo de impacto que una operación puede tener. Se definen cuatro categorías:

- **Financiero**: operaciones que implican transacciones económicas, facturación o gestión de costes.
- **Datos personales**: operaciones que acceden, modifican o eliminan información de identificación personal (PII) u otros datos sujetos a regulación.
- **Infraestructura**: operaciones que modifican la configuración de sistemas, despliegan recursos o alteran la infraestructura tecnológica.
- **Comunicaciones**: operaciones que envían mensajes, notificaciones o comunicaciones en nombre de la organización.

## Regla de Supervisión

Norma que define bajo qué condiciones una operación ejecutada por un agente de IA requiere aprobación humana antes de completarse. Cada regla especifica la operación de riesgo afectada, el usuario o rol que debe aprobar la solicitud, el tiempo máximo de espera (*timeout*) y el comportamiento por defecto en caso de que no se reciba respuesta dentro del plazo (denegar o encolar).

## Aprobación Humana

Solicitud de autorización generada cuando un agente de IA con nivel de autonomía supervisado o restringido intenta ejecutar una operación sujeta a una regla de supervisión. La solicitud se envía al usuario aprobador designado, quien puede aceptar, rechazar o delegar la operación. Incluye la descripción de la acción solicitada, el agente que la solicita, la categoría de riesgo y la marca temporal.

## CAIO

*Chief AI Officer* virtual de la plataforma Horizon. Es un agente de IA que guía al usuario a través de las misiones mediante una interfaz conversacional. Analiza la infraestructura del cliente, propone configuraciones, ejecuta acciones sobre los proveedores cloud y registra todas las operaciones.

## Registro de Auditoría

Entrada en el sistema de trazabilidad que documenta cada acción ejecutada durante una misión. Incluye la identidad del actor (usuario o CAIO), el tipo de acción, el recurso afectado, el resultado y la marca temporal. Garantiza la auditabilidad del proceso de securización.
