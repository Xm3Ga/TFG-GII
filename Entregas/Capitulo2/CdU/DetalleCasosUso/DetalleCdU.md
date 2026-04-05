# Detalle de Casos de Uso

## CdU-01: Conectar proveedor cloud

**Actores**: Usuario (principal), Proveedor Cloud (secundario).

**Precondiciones**: el usuario tiene una cuenta activa en la plataforma Horizon y dispone de credenciales válidas para un proveedor cloud.

**Postcondiciones**: la plataforma ha verificado la conectividad con el proveedor cloud y ha almacenado las credenciales de forma segura.

**Flujo básico**:

1. El usuario inicia la misión de conexión desde el catálogo de misiones.
2. El CAIO solicita al usuario que seleccione el proveedor cloud que desea conectar (AWS, Azure o GCP).
3. El usuario selecciona el proveedor.
4. El CAIO solicita las credenciales de acceso según el proveedor seleccionado (clave de acceso y clave secreta para AWS, credenciales de servicio para GCP, credenciales de aplicación para Azure).
5. El usuario introduce las credenciales.
6. El CAIO verifica la conectividad con el proveedor cloud utilizando las credenciales proporcionadas.
7. El CAIO confirma la conexión exitosa y almacena las credenciales cifradas.

**Flujo alternativo A — Credenciales inválidas (paso 6)**:

6a. El proveedor cloud rechaza las credenciales.
6b. El CAIO informa al usuario del error y solicita nuevas credenciales.
6c. Se retoma el flujo desde el paso 4.

---

## CdU-02: Descubrir agentes

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: el usuario ha conectado al menos un proveedor cloud (CdU-01).

**Postcondiciones**: la plataforma ha identificado y registrado todos los agentes de IA desplegados en el proveedor cloud del usuario.

**Flujo básico**:

1. El usuario inicia la misión de descubrimiento de agentes.
2. El CAIO solicita la selección del proveedor cloud conectado.
3. El usuario selecciona el proveedor.
4. El CAIO consulta las APIs del proveedor para listar todos los agentes de IA desplegados, recorriendo las regiones disponibles.
5. El CAIO presenta al usuario la lista de agentes descubiertos con su información básica (nombre, identificador, región, estado).
6. El CAIO sincroniza la información de los agentes con el inventario interno de la plataforma.

**Flujo alternativo A — Sin agentes (paso 5)**:

5a. No se encuentran agentes desplegados en el proveedor.
5b. El CAIO informa al usuario y sugiere verificar la región o desplegar agentes primero.

---

## CdU-03: Configurar guardarraíl de seguridad de contenido

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: el usuario ha conectado un proveedor cloud y descubierto sus agentes. La credencial tiene permisos IAM para crear y gestionar guardarraíles.

**Postcondiciones**: se ha creado un guardarraíl de seguridad de contenido en el proveedor cloud con las políticas de filtrado definidas por el usuario.

**Flujo básico**:

1. El usuario inicia la misión de guardarraíl de seguridad de contenido.
2. El CAIO solicita la selección de credencial del proveedor cloud.
3. El usuario selecciona la credencial.
4. El CAIO verifica los permisos de la credencial contra el proveedor cloud.
5. El CAIO presenta las opciones de configuración: categorías de filtrado (odio, insultos, contenido sexual, violencia, conductas inapropiadas) y nivel de severidad para cada una (ninguno, bajo, medio, alto).
6. El usuario configura las categorías y niveles deseados.
7. El CAIO muestra un resumen de la configuración y solicita confirmación.
8. El usuario confirma.
9. El CAIO crea el guardarraíl en el proveedor cloud.
10. El CAIO muestra el resultado con el identificador del guardarraíl creado.

**Flujo alternativo A — Permisos insuficientes (paso 4)**:

4a. La credencial no tiene los permisos necesarios.
4b. El CAIO informa de los permisos faltantes con instrucciones para corregirlos.
4c. El usuario corrige los permisos y solicita nueva verificación.
4d. Se retoma desde el paso 4.

**Flujo alternativo B — Error en la creación (paso 9)**:

9a. El proveedor cloud devuelve un error.
9b. El CAIO informa del error y propone acciones correctivas.

---

## CdU-04: Configurar protección anti-jailbreak

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: mismas que CdU-03.

**Postcondiciones**: se ha creado o actualizado un guardarraíl con la protección anti-jailbreak activada.

**Flujo básico**:

1. El usuario inicia la misión de protección anti-jailbreak.
2. El CAIO solicita y verifica la credencial (pasos equivalentes a CdU-03, pasos 2-4).
3. El CAIO presenta las opciones de protección contra inyección de prompts: nivel de filtrado para ataques de entrada y de salida.
4. El usuario configura los niveles de protección.
5. El CAIO muestra el resumen y solicita confirmación.
6. El usuario confirma.
7. El CAIO crea o actualiza el guardarraíl en el proveedor cloud.
8. El CAIO muestra el resultado.

---

## CdU-05: Configurar detección de PII

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: mismas que CdU-03.

**Postcondiciones**: se ha creado o actualizado un guardarraíl con la detección de PII configurada.

**Flujo básico**:

1. El usuario inicia la misión de detección de PII.
2. El CAIO solicita y verifica la credencial.
3. El CAIO presenta los tipos de PII detectables: nombre, dirección, teléfono, correo electrónico, número de documento de identidad, tarjeta de crédito, entre otros.
4. El usuario selecciona los tipos de PII que desea detectar y la acción a aplicar (anonimizar o bloquear) para cada tipo.
5. El CAIO muestra el resumen y solicita confirmación.
6. El usuario confirma.
7. El CAIO crea o actualiza el guardarraíl.
8. El CAIO muestra el resultado.

---

## CdU-06: Configurar bloqueo de temas

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: mismas que CdU-03.

**Postcondiciones**: se ha creado o actualizado un guardarraíl con los temas bloqueados definidos por el usuario.

**Flujo básico**:

1. El usuario inicia la misión de bloqueo de temas.
2. El CAIO solicita y verifica la credencial.
3. El CAIO solicita al usuario que defina los temas que sus agentes deben rechazar, proporcionando una descripción de cada tema y ejemplos de prompts que deberían ser bloqueados.
4. El usuario define los temas prohibidos.
5. El CAIO muestra el resumen y solicita confirmación.
6. El usuario confirma.
7. El CAIO crea o actualiza el guardarraíl.
8. El CAIO muestra el resultado.

---

## CdU-07: Aplicar guardarraíl a agentes

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: existe al menos un guardarraíl creado y al menos un agente descubierto.

**Postcondiciones**: el guardarraíl ha sido asignado a los agentes seleccionados.

**Flujo básico**:

1. El usuario solicita aplicar un guardarraíl a sus agentes.
2. El CAIO lista los guardarraíles disponibles en el proveedor cloud.
3. El usuario selecciona el guardarraíl.
4. El CAIO lista los agentes descubiertos con su estado de protección actual.
5. El usuario selecciona los agentes.
6. El CAIO solicita confirmación.
7. El usuario confirma.
8. El CAIO aplica el guardarraíl a cada agente, actualizando su configuración y preparándolo para que los cambios surtan efecto.
9. El CAIO muestra el resultado por agente.

**Flujo alternativo A — Agente en otra región (paso 8)**:

8a. El agente no está en la región por defecto de la credencial.
8b. El CAIO busca el agente en las regiones disponibles.
8c. Se aplica el guardarraíl en la región correcta.

---

## CdU-08: Analizar privilegios de agentes

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: el usuario ha conectado un proveedor cloud (CdU-01) y descubierto sus agentes (CdU-02). La credencial tiene permisos de lectura sobre las políticas IAM de los agentes y sobre los logs de actividad del proveedor (CloudTrail en AWS, Activity Log en Azure, Cloud Audit Logs en GCP).

**Postcondiciones**: se ha generado un análisis de privilegios para cada agente seleccionado, con un informe detallado que identifica los permisos excesivos y produce recomendaciones de reducción concretas.

**Flujo básico**:

1. El usuario inicia la misión de mínimo privilegio desde el catálogo de misiones.
2. El CAIO solicita al usuario que seleccione la credencial del proveedor cloud.
3. El usuario selecciona la credencial.
4. El CAIO verifica los permisos de la credencial. Comprueba que dispone de acceso para leer las políticas IAM de los agentes, consultar los logs de actividad y, opcionalmente, modificar políticas de permisos.
5. El CAIO lista los agentes descubiertos, mostrando para cada uno: nombre, identificador, rol IAM asociado y número de permisos asignados.
6. El usuario selecciona los agentes que desea analizar.
7. El CAIO solicita al usuario que defina el período de análisis: el rango temporal sobre el que se consultará el historial de uso (por ejemplo, últimos 30, 60 o 90 días).
8. El CAIO inicia la recopilación del perfil de uso de cada agente seleccionado, consultando los logs de actividad del proveedor cloud para identificar qué permisos ha utilizado cada agente durante el período indicado.
9. El CAIO compara los permisos asignados a cada agente con su perfil de uso, identificando tres categorías:
   - **Permisos utilizados**: acciones que el agente ha ejecutado durante el período.
   - **Permisos no utilizados**: acciones asignadas pero nunca ejecutadas.
   - **Permisos de alto riesgo no utilizados**: permisos con impacto potencial significativo (escritura en bases de datos, procesamiento de pagos, eliminación de recursos) que el agente no ha necesitado.
10. El CAIO genera un informe de análisis para cada agente con la lista de recomendaciones de reducción. Cada recomendación incluye: el permiso que se recomienda eliminar, la justificación (no utilizado en X días), el nivel de riesgo del permiso y el impacto estimado de su eliminación.
11. El CAIO presenta el informe completo al usuario, organizado por agente y ordenado por nivel de riesgo de los permisos excesivos.

**Flujo alternativo A — Permisos insuficientes (paso 4)**:

4a. La credencial no tiene acceso de lectura a las políticas IAM o a los logs de actividad.
4b. El CAIO identifica los permisos faltantes según el proveedor:
   - AWS: `iam:GetPolicy`, `iam:ListAttachedRolePolicies`, `cloudtrail:LookupEvents`.
   - Azure: `Microsoft.Authorization/roleAssignments/read`, `Microsoft.Insights/ActivityLogAlerts/read`.
   - GCP: `iam.roles.get`, `logging.logEntries.list`.
4c. El CAIO proporciona instrucciones específicas para configurar estos permisos.
4d. El usuario corrige los permisos y solicita nueva verificación.

**Flujo alternativo B — Datos de uso insuficientes (paso 8)**:

8a. Los logs de actividad no contienen suficiente información para el período solicitado (por ejemplo, el *logging* no estaba activado o el período es demasiado corto).
8b. El CAIO informa al usuario del problema y presenta dos opciones:
   - Ampliar el período de análisis si hay datos disponibles en un rango mayor.
   - Activar un período de observación: el CAIO configura la monitorización del agente para que se recopilen datos durante un período determinado, tras el cual se podrá reiniciar el análisis.
8c. El usuario elige la opción deseada.

**Flujo alternativo C — Agente sin rol IAM explícito (paso 5)**:

5a. El agente no tiene un rol IAM asignado de forma explícita (hereda permisos del servicio).
5b. El CAIO informa al usuario y recomienda crear un rol IAM dedicado para el agente antes de proceder con el análisis.

---

## CdU-09: Aplicar reducción de privilegios

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: se ha completado un análisis de privilegios (CdU-08) que ha producido al menos una recomendación de reducción.

**Postcondiciones**: los permisos aceptados como excesivos han sido eliminados de los agentes correspondientes. Los agentes siguen operando correctamente con el conjunto reducido de permisos. Se ha generado un registro de auditoría con los cambios realizados.

**Flujo básico**:

1. El CAIO presenta al usuario el resumen de las recomendaciones de reducción del análisis previo, organizado por agente. Para cada recomendación muestra: el permiso a eliminar, la justificación, el nivel de riesgo y el impacto estimado.
2. El usuario revisa cada recomendación y selecciona cuáles acepta aplicar. Para cada recomendación, puede:
   - **Aceptar**: el permiso será eliminado.
   - **Rechazar**: el permiso se mantiene (el CAIO solicita un motivo para el registro de auditoría).
   - **Aplazar**: la recomendación se marca como pendiente para una revisión futura.
3. El CAIO muestra un resumen de los cambios que se van a aplicar y solicita confirmación final.
4. El usuario confirma la aplicación.
5. El CAIO genera una política IAM actualizada para cada agente afectado, eliminando únicamente los permisos aceptados para reducción.
6. El CAIO aplica la nueva política sobre el proveedor cloud. Para cada agente:
   a. Crea una copia de seguridad de la política actual (*snapshot*).
   b. Aplica la política reducida.
   c. Registra la operación en el sistema de auditoría.
7. El CAIO inicia una verificación de funcionamiento: comprueba que los agentes afectados siguen respondiendo correctamente ejecutando una prueba de conectividad básica.
8. El CAIO presenta el resultado final al usuario: agentes actualizados, permisos eliminados, resultado de la verificación.

**Flujo alternativo A — Rechazo total (paso 2)**:

2a. El usuario rechaza todas las recomendaciones.
2b. El CAIO registra los motivos, genera el registro de auditoría y marca la misión como completada sin cambios.

**Flujo alternativo B — Error durante la aplicación (paso 6)**:

6a. El proveedor cloud devuelve un error al aplicar la nueva política en un agente.
6b. El CAIO restaura la política original desde la copia de seguridad.
6c. El CAIO informa al usuario del error específico y del agente afectado.
6d. Se continúa con los agentes restantes.

**Flujo alternativo C — Fallo en la verificación (paso 7)**:

7a. Un agente no responde correctamente tras la reducción de permisos.
7b. El CAIO ejecuta un *rollback* automático, restaurando la política original del agente desde la copia de seguridad.
7c. El CAIO informa al usuario de que el agente requería el permiso eliminado y lo marca como falso positivo en el análisis.
7d. El CAIO recomienda un período de observación más largo para ese agente.

---

## CdU-10: Definir niveles de autonomía

**Actores**: Usuario (principal), CAIO (sistema), Proveedor Cloud (secundario).

**Precondiciones**: el usuario ha conectado un proveedor cloud (CdU-01) y descubierto sus agentes (CdU-02). La credencial tiene permisos para consultar la configuración de los agentes (herramientas asignadas, APIs accesibles).

**Postcondiciones**: se ha creado una política de autonomía para cada agente seleccionado, con su nivel de autonomía asignado basado en el análisis de riesgo de sus operaciones.

**Flujo básico**:

1. El usuario inicia la misión de definición de autonomía desde el catálogo de misiones.
2. El CAIO solicita al usuario que seleccione la credencial del proveedor cloud.
3. El usuario selecciona la credencial.
4. El CAIO verifica los permisos de la credencial para consultar la configuración de los agentes.
5. El CAIO lista los agentes descubiertos, mostrando para cada uno: nombre, identificador, descripción y herramientas o APIs asignadas.
6. El CAIO analiza las capacidades de cada agente. Para cada herramienta o API que el agente tiene acceso, identifica las operaciones que puede ejecutar.
7. El CAIO clasifica cada operación en una categoría de riesgo:
   - **Financiero**: operaciones que implican transacciones, facturación o gestión de costes.
   - **Datos personales**: operaciones que acceden, modifican o eliminan PII u otros datos regulados.
   - **Infraestructura**: operaciones que modifican configuraciones de sistemas, despliegan recursos o alteran la infraestructura.
   - **Comunicaciones**: operaciones que envían mensajes, notificaciones o comunicaciones externas.
8. El CAIO asigna un nivel de riesgo a cada operación (bajo, medio, alto, crítico) y presenta al usuario un informe de riesgo por agente. El informe muestra para cada agente: sus operaciones, la categoría y nivel de riesgo de cada una, y el nivel de autonomía recomendado.
9. El CAIO propone un nivel de autonomía para cada agente basado en el análisis:
   - **Total**: si todas las operaciones del agente son de riesgo bajo.
   - **Supervisado**: si alguna operación es de riesgo medio o alto.
   - **Restringido**: si alguna operación es de riesgo crítico o si el agente está en fase de pruebas.
10. El usuario revisa las propuestas y puede:
    - Aceptar la recomendación del CAIO.
    - Modificar el nivel de autonomía propuesto (subir o bajar).
    - Reclasificar el nivel de riesgo de operaciones específicas si no está de acuerdo con la clasificación automática.
11. El CAIO registra las decisiones del usuario. Si el usuario ha reclasificado operaciones, el CAIO recalcula las recomendaciones de nivel de autonomía.
12. El CAIO genera una política de autonomía para cada agente con el nivel asignado confirmado.
13. El CAIO muestra un resumen con todos los agentes y sus niveles de autonomía asignados.

**Flujo alternativo A — Permisos insuficientes (paso 4)**:

4a. La credencial no tiene acceso para consultar la configuración de los agentes.
4b. El CAIO informa de los permisos necesarios según el proveedor.
4c. El usuario corrige y reintenta.

**Flujo alternativo B — Agente sin herramientas asignadas (paso 6)**:

6a. Un agente no tiene herramientas ni APIs asignadas (solo responde con texto).
6b. El CAIO clasifica automáticamente al agente como de riesgo bajo y recomienda autonomía total.
6c. El usuario puede aceptar o asignar un nivel más restrictivo.

---

## CdU-11: Configurar supervisión humana

**Actores**: Usuario (principal), CAIO (sistema).

**Precondiciones**: se ha asignado el nivel de autonomía "supervisado" o "restringido" a al menos un agente (CdU-10).

**Postcondiciones**: se han definido las reglas de supervisión humana para cada agente con nivel supervisado o restringido, incluyendo las operaciones que requieren aprobación, los usuarios aprobadores designados y el comportamiento en caso de *timeout*.

**Flujo básico**:

1. El CAIO presenta al usuario los agentes que tienen nivel de autonomía supervisado o restringido, con sus operaciones de riesgo identificadas.
2. El usuario selecciona un agente para configurar su supervisión.
3. El CAIO muestra las operaciones de riesgo del agente seleccionado, organizadas por categoría y nivel de riesgo. Para cada operación indica si requiere supervisión (basado en el nivel de autonomía) y el tipo de supervisión recomendada.
4. Para agentes con nivel supervisado, el usuario define qué operaciones concretas requieren aprobación humana. El CAIO preselecciona las operaciones de riesgo alto y crítico, pero el usuario puede añadir o quitar operaciones de la lista.
5. Para agentes con nivel restringido, todas las operaciones requieren aprobación (el CAIO lo configura automáticamente).
6. Para cada operación que requiere aprobación, el CAIO solicita:
   a. **Aprobador**: el usuario o rol de la organización que recibirá las solicitudes de aprobación. Puede ser un usuario específico, un equipo o un rol (por ejemplo, "administrador de seguridad").
   b. **Timeout**: el tiempo máximo de espera para recibir la aprobación (por ejemplo, 1 hora, 4 horas, 24 horas).
   c. **Comportamiento por defecto**: qué ocurre si el aprobador no responde dentro del *timeout*: denegar la operación (opción conservadora) o encolar la operación para revisión posterior.
7. El usuario configura estos parámetros para cada regla de supervisión.
8. El CAIO muestra un resumen de todas las reglas de supervisión configuradas para el agente: operación, aprobador, timeout y comportamiento por defecto.
9. El usuario confirma la configuración.
10. Se repiten los pasos 2-9 para cada agente con supervisión pendiente de configurar.
11. El CAIO activa las políticas de autonomía con las reglas de supervisión definidas y muestra un resumen global.

**Flujo alternativo A — Sin usuarios aprobadores disponibles (paso 6a)**:

6a-i. La organización solo tiene un usuario registrado en la plataforma.
6a-ii. El CAIO advierte de que el aprobador sería el mismo usuario que opera los agentes, lo que reduce la efectividad de la supervisión.
6a-iii. El CAIO recomienda registrar usuarios adicionales antes de activar la supervisión.

**Flujo alternativo B — Modificación posterior (tras paso 11)**:

11a. El usuario desea modificar una regla de supervisión existente.
11b. El CAIO permite seleccionar el agente y la regla a modificar.
11c. El usuario realiza los cambios y confirma.
11d. El CAIO actualiza la política de autonomía del agente.
