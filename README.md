# Hoja de ruta de la defensa del TFG

## 1. Título del TFG

**Diseño e implementación de misiones de seguridad para agentes de inteligencia artificial en una plataforma B2B de gobernanza multi-proveedor**

Idea central para abrir la defensa:

> Este TFG no plantea solo un chatbot. Plantea una capa de gobierno de seguridad para agentes de IA: el CAIO guía al usuario, pero las acciones sensibles se ejecutan mediante servicios controlados, trazables y revisables.

## 2. Modelo del dominio

Primero presento el espacio del problema: organización, usuario, credenciales cloud, proveedores, agentes IA, misiones, guardarraíles, baselines de mínimo privilegio, perfiles de autonomía y auditoría.

![Modelo del dominio](Entregas/Capitulo2/MdD/ModeloDominio/MdD.svg)

Mensaje oral:

- Una organización tiene usuarios, credenciales y agentes desplegados en proveedores cloud.
- El elemento central es la misión: un flujo guiado por el CAIO para aplicar seguridad sobre agentes.
- Las tres misiones del alcance son guardarraíles, mínimo privilegio y autonomía.
- La auditoría cierra el ciclo: las acciones no se quedan en una conversación, quedan trazadas.

## 3. Actores y casos de uso

### Actores encontrados

![Actores del sistema](Entregas/Capitulo2/CdU/ActoresIndividuales/Actores.svg)

Los actores que explico en la defensa son:

- **Usuario**: inicia misiones, proporciona credenciales, selecciona agentes y confirma cambios.
- **CAIO**: guía la misión, solicita datos, coordina validaciones y presenta resultados.
- **Proveedor cloud**: sistema externo que expone APIs para validar credenciales, descubrir agentes, crear guardarraíles y consultar permisos.

### Casos de uso por actor

#### Usuario

![Casos de uso del usuario](Entregas/Capitulo2/CdU/CdUPorActor/Usuario/Usuario.svg)

Casos clave para la defensa:

| Caso de uso | Papel en el TFG |
| --- | --- |
| CdU-01 Conectar proveedor cloud | Prerrequisito para cualquier misión. |
| CdU-02 Descubrir agentes | Permite tener inventario antes de proteger. |
| CdU-03 a CdU-07 Guardarraíles | Núcleo de protección conversacional. |
| CdU-08 y CdU-09 Mínimo privilegio | Análisis, revisión y aplicación controlada de permisos. |
| CdU-10 y CdU-11 Autonomía | Clasificación de riesgo y validaciones humanas propuestas. |

#### Proveedor cloud

![Casos de uso del proveedor cloud](Entregas/Capitulo2/CdU/CdUPorActor/ProveedorCloud/ProveedorCloud.svg)

Mensaje oral:

- El proveedor no inicia nada; responde a peticiones del sistema.
- Esta separación justifica la arquitectura por conectores: AWS, Azure y GCP no se tratan igual internamente, pero el sistema los abstrae.

## 4. Diagrama de contexto

![Diagrama de contexto](Entregas/Capitulo2/CdU/DiagramaContexto/DiagramaContexto.svg)

Lectura del diagrama:

1. El usuario conecta un proveedor cloud.
2. El sistema descubre agentes desplegados.
3. A partir de agentes descubiertos se ejecutan misiones de seguridad.
4. Las misiones de guardarraíles siguen los hitos de credencial, permisos, configuración y aplicación.
5. Mínimo privilegio añade preflight, baseline, revisión y apply/rollback.
6. Autonomía genera perfiles y propuestas de validación humana, sin enforcement runtime en el MVP.

## 5. Casos de uso representativos

La guía de defensa pide enseñar la cascada completa: detalle del caso de uso, interfaz de usuario propuesta, análisis MVC y diseño. Yo no lo tengo escrito exactamente con esos cuatro nombres, así que lo abordaré como una trazabilidad entre documentos y código:

| Lo que pide la defensa | Dónde está en mi trabajo | Cómo lo explico |
| --- | --- | --- |
| Detalle del caso de uso | `Entregas/Capitulo2/CdU/DetalleCasosUso/DetalleCdU.md` | Actores, precondiciones, postcondiciones, flujo básico y alternativos. |
| Interfaz de usuario propuesta | `Entregas/Capitulo2.md`, apartado 4.6, y `Entregas/Capitulo4.md` | Pantalla real o propuesta desde la que el usuario ejecuta el caso. |
| Análisis MVC | `Entregas/Capitulo3.md`, apartados 4.3 a 4.5 | Vista, controlador y entidades, aunque en la memoria estén descritos como clases frontera, control y entidad. |
| Diseño | `Entregas/Capitulo3.md` y `Entregas/Capitulo4.md` | Routers, servicios, repositorios, conectores, persistencia y diagramas técnicos. |

Traducción directa a MVC para explicarlo oralmente:

| MVC | En mi documentación | En el código |
| --- | --- | --- |
| Vista | Clases frontera: `VistaCredenciales`, `VistaMisionConversacional`, `VistaCatalogoMisiones` | Frontend Next.js: páginas de credenciales, misiones, agentes y seguridad. |
| Controlador | Clases control: `ControladorCredenciales`, `ControladorGuardarrailes` | Routers FastAPI y servicios: `credentials.py`, `missions.py`, `SyncService`. |
| Modelo | Clases entidad: `Credencial`, `AgenteIA`, `Guardarrail`, `RegistroDeAuditoria` | Modelos ORM, repositorios, `CredentialRepository`, `AgentRepository`, `AuditLog`, conectores cloud. |

### CdU-01: Conectar proveedor cloud

#### 1. Detalle del caso de uso

Fuente: [`DetalleCdU.md`](Entregas/Capitulo2/CdU/DetalleCasosUso/DetalleCdU.md).

| Elemento | Resumen para defensa |
| --- | --- |
| Actor principal | Usuario. |
| Actor secundario | Proveedor cloud. |
| Precondición | El usuario tiene cuenta en Theia Officer y credenciales válidas del proveedor. |
| Postcondición | La plataforma verifica conectividad y almacena la credencial cifrada. |
| Flujo principal | Seleccionar proveedor, introducir credenciales, validar contra proveedor, cifrar y guardar. |
| Alternativo | Si el proveedor rechaza credenciales, se informa del error y se solicitan nuevas credenciales. |

Mensaje oral:

> Este caso no es vistoso, pero es el prerrequisito del resto. Sin una credencial válida no puedo descubrir agentes, crear guardarraíles ni analizar permisos.

#### 2. Interfaz de usuario propuesta

Pantalla que debo enseñar:

- Página **Credenciales**.
- Alta de credencial por proveedor: AWS, Azure o GCP.
- Región y datos específicos del proveedor.
- Validación antes de persistir.
- Resultado visible: credencial disponible para misiones y sincronización de agentes.

Evidencia en código:

- `theia-officer-caio-virtual/frontend/src/app/(dashboard)/credentials/page.tsx`
- `theia-officer-caio-virtual/backend/app/api/v1/credentials.py`

#### 3. Análisis MVC

| MVC | Elemento | Responsabilidad |
| --- | --- | --- |
| Vista | `VistaCredenciales` | Recoge proveedor, región y datos de credencial. |
| Controlador | `ControladorCredenciales` | Coordina validación, cifrado, persistencia y auditoría. |
| Modelo | `Credencial`, `ProveedorCloud`, `RegistroDeAuditoria` | Representan la credencial cifrada, el proveedor conectado y la trazabilidad. |

Aunque el documento no tenga un diagrama de secuencia específico solo para CdU-01, la secuencia que explicaré es:

```text
Usuario -> VistaCredenciales -> credentials.py -> ConnectorFactory
ConnectorFactory -> AWS/Azure/GCPConnector.test_connection()
credentials.py -> CredentialEncryptor -> CredentialRepository
credentials.py -> AuditLog
```

#### 4. Diseño

| Pieza de diseño | Papel |
| --- | --- |
| `credentials.py` | Expone `POST /credentials`, `GET /credentials`, `POST /credentials/{id}/test` y sincronización. |
| `ConnectorFactory` | Instancia el conector del proveedor seleccionado. |
| `AWSConnector`, `AzureConnector`, `GCPConnector` | Validan conectividad con APIs cloud concretas. |
| `CredentialEncryptor` | Cifra secretos antes de guardarlos. |
| `CredentialRepository` | Persiste y consulta credenciales sin devolver secretos al frontend. |
| `AuditLog` | Registra creación, borrado o acciones relevantes sobre credenciales. |

Decisión de diseño a destacar:

> La credencial no se guarda primero y se prueba después. Se valida contra el proveedor antes de persistir, y si es válida se cifra. Esto responde directamente al requisito de seguridad de credenciales.

### CdU-04: Configurar protección anti-jailbreak

#### 1. Detalle del caso de uso

Fuente: [`DetalleCdU.md`](Entregas/Capitulo2/CdU/DetalleCasosUso/DetalleCdU.md).

| Elemento | Resumen para defensa |
| --- | --- |
| Actor principal | Usuario. |
| Actores secundarios | CAIO y proveedor cloud. |
| Precondición | Existe proveedor conectado, agentes descubiertos y permisos para gestionar guardarraíles. |
| Postcondición | Se crea o actualiza un guardarraíl con protección anti-jailbreak. |
| Flujo principal | Seleccionar credencial, verificar permisos, configurar niveles de protección, confirmar y crear/actualizar guardarraíl. |

Mensaje oral:

> Este caso representa la protección frente a prompt injection y jailbreak. En la demo lo uniré con la aplicación a agentes, porque crear el guardarraíl no basta: debe asociarse al agente para que tenga efecto.

#### 2. Interfaz de usuario propuesta

Pantalla que debo enseñar:

- Catálogo de misiones.
- Misión **guardrail-jailbreak-aws** o equivalente visible.
- Selector de credencial y región.
- Verificación de permisos.
- Configuración de protección anti-jailbreak.
- Selección de agentes.
- Resumen final con agentes protegidos, actualizados o fallidos.

Evidencia en código:

- `theia-officer-caio-virtual/frontend/src/app/(dashboard)/missions/missions-content.tsx`
- `theia-officer-caio-virtual/frontend/src/lib/missions.ts`
- `theia-officer-caio-virtual/backend/app/api/v1/missions.py`
- `theia-officer-caio-virtual/backend/app/connectors/aws/connector.py`

#### 3. Análisis MVC

| MVC | Elemento | Responsabilidad |
| --- | --- | --- |
| Vista | `VistaMisionConversacional` | Guía al usuario por hitos: credencial, permisos, configuración y agentes. |
| Controlador | `ControladorGuardarrailes` | Verifica permisos, crea/actualiza guardarraíl y aplica a agentes. |
| Modelo | `Credencial`, `AgenteIA`, `Guardarrail`, `RegistroDeAuditoria` | Mantienen estado, configuración, recursos cloud y evidencias. |

Diagrama de secuencia que usaré como apoyo:

![Secuencia de guardarraíles](Entregas/Capitulo3/Analisis/CasosUso/GuardarrailesSecuencia.svg)

La lectura del diagrama para CdU-04 es:

1. La vista inicia la misión de guardarraíl.
2. `MissionsRouter` obtiene y descifra credencial.
3. `AWSConnector` verifica permisos Bedrock.
4. El usuario configura la protección anti-jailbreak.
5. El backend crea o actualiza el guardarraíl.
6. El usuario selecciona agentes.
7. El backend asocia el guardarraíl y prepara el agente.
8. Se registra auditoría y se muestran evidencias.

#### 4. Diseño

| Pieza de diseño | Papel |
| --- | --- |
| `missions-content.tsx` | Implementa la experiencia guiada de misión en el frontend. |
| `missions.ts` | Define plantillas de misiones, incluido `guardrail-jailbreak-{provider}`. |
| `missions.py` | Gestiona introspección, acciones de misión y ejecución cloud. |
| `check_guardrail_permissions` | Comprueba permisos antes de crear o aplicar guardarraíles. |
| `create_guardrail` | Crea el recurso de protección en el proveedor. |
| `apply_guardrail_to_agents` | Asocia la protección a agentes seleccionados. |
| `AWSConnector.create_guardrail()` | Traduce la configuración al API de Bedrock Guardrails. |
| `AWSConnector.update_agent_guardrail()` | Actualiza el agente con la configuración del guardarraíl. |
| `prepare_agent()` | Prepara el agente para que el cambio tenga efecto real. |

Decisión de diseño a destacar:

> El CAIO guía la conversación, pero no ejecuta cambios por texto libre. Las acciones reales pasan por endpoints tipados, verificación de permisos, conectores cloud y auditoría.

Limitación que debo decir explícitamente:

> El soporte completo de guardarraíles está centrado en AWS Bedrock. GCP se cubre en controles concretos mediante Security Settings y Azure no se presenta como guardarraíl implementado en el MVP.

## 6. Entrada a la demostración

Ruta breve para enlazar esta hoja con la demo:

1. Mostrar credenciales y agentes como prerrequisitos.
2. Entrar al catálogo de misiones.
3. Ejecutar o enseñar el flujo de protección anti-jailbreak.
4. Mostrar que el resultado se asocia a agentes.
5. Revisar evidencias en seguridad o auditoría.

Frase de transición:

> Hasta aquí he enseñado el modelo, los requisitos y la trazabilidad. Ahora voy a mostrar cómo esa estructura se materializa en la aplicación.

## 7. Conclusiones
