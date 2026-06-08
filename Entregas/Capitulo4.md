# 4. SOLUCION DESARROLLADA

Este capitulo presenta la solucion implementada. A diferencia de los capitulos de analisis y diseno, aqui se describe como se usa el MVP desde la interfaz y que partes del codigo materializan cada mision.

Theia Officer es una aplicacion web para gobernar agentes de IA en entornos cloud. Dentro de este TFG solo se presentan las tres misiones de seguridad definidas desde el inicio: guardarrailes, minimo privilegio y autonomia. Las pantallas de credenciales, agentes, CAIO o actividad se explican como soporte para esas misiones, no como ampliacion del alcance.

## 4.1. Mapa de la solucion

La navegacion funcional parte de una idea sencilla: antes de proteger un agente, el sistema necesita una credencial cloud valida y un inventario sincronizado. A partir de ahi, el usuario puede ejecutar misiones y revisar evidencias.

| Orden logico | Pantalla | Funcion dentro del TFG |
| --- | --- | --- |
| 1 | Credenciales | Registrar credenciales AWS, GCP o Azure y validarlas contra el proveedor. |
| 2 | Agentes | Consultar los agentes descubiertos y normalizados. |
| 3 | Misiones | Ejecutar guardarrailes, minimo privilegio o autonomia con ayuda del CAIO. |
| 4 | Seguridad | Revisar evidencias por agente despues de ejecutar misiones. |
| 5 | Actividad/Auditoria | Consultar trazabilidad de acciones relevantes. |

| Pantalla | Archivo principal | Elementos de interfaz relevantes |
| --- | --- | --- |
| Misiones | `frontend/src/app/(dashboard)/missions/missions-content.tsx` | Catalogo, panel de hito actual, selector de credencial, formularios de configuracion, seleccion de agentes y resumen de resultado. |
| Seguridad | `frontend/src/app/(dashboard)/security/page.tsx` | Tabla de agentes, tarjetas de evidencia, estado de guardarrail, baseline de privilegios y perfil de autonomia. |
| Agentes | `frontend/src/app/(dashboard)/agents/page.tsx` | Inventario normalizado por proveedor, estado, region y metadatos. |
| Credenciales | `frontend/src/app/(dashboard)/credentials/page.tsx` | Alta, validacion y sincronizacion de credenciales cloud. |

La pantalla de misiones es la pieza principal del TFG. En ella, el CAIO acompana al usuario, pero la ejecucion real no depende de texto libre: cada avance llama a endpoints y acciones tipadas del backend.

## 4.2. Navegabilidad por casos de uso

| Caso de uso | Inicio en interfaz | Navegacion esperada | Resultado visible |
| --- | --- | --- | --- |
| CdU-01 Conectar proveedor | Credenciales | Crear credencial, validar y sincronizar | Credencial disponible para misiones. |
| CdU-02 Descubrir agentes | Credenciales / Agentes | Ejecutar sincronizacion y abrir inventario | Agentes normalizados por proveedor. |
| CdU-03 a CdU-07 Guardarrailes | Misiones | Elegir mision, credencial, permisos, configuracion y agentes | Agentes protegidos, actualizados o fallidos. |
| CdU-08 Analizar privilegios | Misiones | Elegir mision de least privilege y ejecutar scan | Baselines generadas por agente. |
| CdU-09 Aplicar reduccion | Misiones / Seguridad | Revisar baseline y aplicar cambios aprobados | Apply run y snapshot de rollback. |
| CdU-10 Clasificar autonomia | Misiones | Ejecutar `classify-by-autonomy` | Perfiles de autonomia por agente. |
| CdU-11 Revisar validaciones | Seguridad | Abrir evidencia de autonomia | Propuestas de validacion humana. |

Esta navegabilidad mantiene la relacion con el diagrama de contexto: el usuario no entra directamente en una operacion tecnica, sino que avanza por casos de uso guiados.

## 4.3. Catalogo de misiones

El catalogo de misiones se define en `frontend/src/lib/missions.ts`. La solucion no muestra todas las ideas posibles del producto; filtra el catalogo para que el usuario vea misiones que tienen sentido en el MVP.

| Grupo de mision | Identificador / plantilla | Estado |
| --- | --- | --- |
| Guardarrailes AWS | Misiones por tipo: content safety, jailbreak, PII, topics, language, grounding | Implementadas sobre Bedrock Guardrails. |
| Guardarrailes GCP | PII y retention policy | Implementadas sobre Dialogflow CX Security Settings. |
| Guardarrailes Azure | Misiones de guardarrail | Ocultas en el catalogo porque no hay aplicacion real. |
| Minimo privilegio | `least-privilege-{provider}-agents` | Implementado para AWS, GCP y Azure mediante adaptadores. |
| Autonomia | `classify-by-autonomy` | Implementado como analisis y recomendacion. |

Este comportamiento evita que el TFG aparente cubrir todo el producto. La solucion se centra en las tres misiones acordadas y distingue claramente los proveedores disponibles.

## 4.4. Solucion de guardarrailes

La mision de guardarrailes permite aplicar protecciones conversacionales a agentes ya descubiertos. Su flujo se divide en hitos:

| Hito | Accion del usuario | Accion del sistema |
| --- | --- | --- |
| Seleccionar credencial | Escoge proveedor, credencial y region. | Carga credenciales compatibles. |
| Verificar permisos | Solicita comprobacion. | Llama a `check_guardrail_permissions` y muestra permisos faltantes si existen. |
| Configurar politica | Define filtros segun el tipo de guardarrail. | Convierte la configuracion al formato del proveedor. |
| Seleccionar agentes | Elige agentes objetivo. | Lista agentes compatibles y detecta si ya tienen proteccion. |
| Aplicar | Confirma la aplicacion. | Crea o actualiza el recurso y lo asocia a agentes seleccionados. |

En backend, el flujo se concentra en `backend/app/api/v1/missions.py`. Las acciones principales son:

| Accion backend | Uso |
| --- | --- |
| `list_guardrails` | Consultar recursos existentes. |
| `check_guardrail_permissions` | Verificar permisos necesarios antes de operar. |
| `create_guardrail` | Crear un guardarrail o security setting. |
| `update_guardrail` | Actualizar un recurso existente cuando corresponde. |
| `apply_guardrail_to_agents` | Asociar la proteccion a los agentes seleccionados. |
| `get_agent_guardrail_status` | Consultar la proteccion ya aplicada al agente. |

La solucion contempla resultados parciales. Si algunos agentes se protegen y otros fallan, la interfaz mantiene listas separadas de `guardrailAppliedAgents`, `guardrailUpdatedAgents` y errores. Esto es importante porque las operaciones cloud pueden fallar por region, permisos o estado individual del agente.

### Cobertura por proveedor

| Control | AWS Bedrock | GCP Dialogflow CX | Azure |
| --- | --- | --- | --- |
| Seguridad de contenido | Si | No | No visible |
| Anti-jailbreak | Si | No | No visible |
| PII | Si | Si | No visible |
| Temas | Si | No | No visible |
| Lenguaje | Si | No | No visible |
| Grounding | Si | No | No visible |
| Retencion | No | Si | No visible |

En AWS se usan Bedrock Guardrails. En GCP, para el alcance implementado, se usan Security Settings. Esta diferencia aparece tanto en la configuracion enviada al backend como en el resumen que se muestra al usuario.

## 4.5. Solucion de minimo privilegio

La mision de minimo privilegio se implementa con un modulo propio. Su objetivo es generar una propuesta concreta de permisos minimos por agente y permitir su aplicacion controlada.

| Hito de interfaz | Endpoint / servicio | Resultado |
| --- | --- | --- |
| Select credential | `/least-privilege/preflight` | Comprueba credencial y capacidad de analizar agentes. |
| Resolve principals | `LeastPrivilegeService.preflight` | Resuelve identidad runtime de cada agente. |
| Generate baseline | `/least-privilege/scans` | Genera `LeastPrivilegeBaseline`. |
| Review baseline | `/least-privilege/baselines` | Muestra permisos actuales, propuestos y removibles. |
| Apply approved changes | `/least-privilege/baselines/{id}/apply` | Aplica cambios aprobados y guarda rollback. |

La baseline contiene la informacion que el usuario necesita para decidir:

| Dato | Significado |
| --- | --- |
| Permisos actuales | Lo que el agente o su identidad runtime tiene asignado. |
| Permisos observados | Evidencia de permisos usados o declarados. |
| Permisos propuestos | Conjunto minimo que el sistema recomienda conservar. |
| Permisos removibles | Privilegios que pueden retirarse segun la evidencia. |
| Confianza | Nivel de fiabilidad de la propuesta. |
| Estado | Indica si esta lista, bloqueada, aplicada o requiere revision. |

La aplicacion de cambios no es automatica. El usuario debe revisar y aprobar. El backend crea un `LeastPrivilegeApplyRun`, guarda un snapshot de rollback y usa locks para evitar que dos operaciones editen el mismo principal al mismo tiempo.

## 4.6. Solucion de autonomia

La mision de autonomia clasifica agentes segun senales de riesgo en sus metadatos. No es un sistema de bloqueo runtime; es una herramienta de analisis y recomendacion.

| Paso | Funcion |
| --- | --- |
| Seleccion de alcance | El usuario inicia la mision `classify-by-autonomy`. |
| Escaneo | `AutonomyService` revisa agentes descubiertos y aplica reglas deterministas. |
| Perfil | Se crea o actualiza un `AgentAutonomyProfile` por agente. |
| Revision | La pagina `/security` muestra nivel, confianza, evidencias y validaciones sugeridas. |

Las reglas buscan patrones concretos:

| Riesgo detectado | Validacion humana propuesta |
| --- | --- |
| Pagos o transferencias | Revision antes de ejecutar operaciones monetarias. |
| Escritura o borrado de datos | Aprobacion antes de cambios persistentes. |
| IAM/RBAC o credenciales | Revision de identidad y permisos antes de modificar acceso. |
| Datos sensibles | Validacion antes de leer, exportar o enviar informacion regulada. |
| Comunicaciones externas | Confirmacion antes de llamar a sistemas externos. |
| Cambios en produccion | Aprobacion antes de desplegar o modificar infraestructura. |
| Ejecucion de codigo | Revision antes de ejecutar comandos o scripts. |

El resultado se representa con niveles `low`, `medium`, `high` o `unknown`. Cuando la evidencia es insuficiente, el sistema no fuerza una clasificacion artificial: marca el perfil como desconocido o con menor confianza.

## 4.7. Pagina de seguridad como evidencia

La pagina `/security` une los resultados de las misiones y permite revisar la postura por agente. No sustituye a cada mision; funciona como panel de evidencia despues de ejecutarlas.

| Bloque de evidencia | Origen |
| --- | --- |
| Guardarrailes | Resultados locales de misiones y metadatos de agente. |
| Runtime access | Baselines y apply runs de `/least-privilege`. |
| Human validation | Perfiles de `/autonomy/profiles`. |

Esta pantalla es relevante para el TFG porque demuestra que las misiones no terminan en un mensaje conversacional. Sus resultados quedan consultables y asociados al agente.

## 4.8. Trazabilidad tecnica de la solucion

| Mision | Frontend | Backend | Persistencia |
| --- | --- | --- | --- |
| Guardarrailes | `missions-content.tsx`, `missions.ts` | `missions.py`, conectores AWS/GCP | Estado de mision, metadatos de agente y auditoria. |
| Minimo privilegio | `missions-content.tsx`, `api-client.ts`, `/security` | `least_privilege.py`, `LeastPrivilegeService` | `least_privilege_scans`, `least_privilege_baselines`, `least_privilege_apply_runs`, `least_privilege_events`. |
| Autonomia | `missions-content.tsx`, `/security` | `autonomy.py`, `AutonomyService` | `autonomy_scan_runs`, `agent_autonomy_profiles`. |

## 4.9. Validacion funcional

La validacion funcional se ha basado en revisar que cada requisito del capitulo 2 tenga un flujo visible y un componente tecnico asociado.

| Objetivo validado | Evidencia |
| --- | --- |
| Ejecutar guardarrailes | Flujo de credencial, permisos, configuracion y aplicacion. |
| Generar minimo privilegio | Preflight, scan y baseline por agente. |
| Aplicar cambios de minimo privilegio | Aprobacion, apply run, lock y rollback. |
| Clasificar autonomia | Scan metadata-only y perfiles persistidos. |
| Revisar resultados | Pagina `/security` con evidencias por agente. |

Tambien se han revisado los limites de la solucion: Azure no se presenta como proveedor de guardarrailes implementado; GCP usa Security Settings; y autonomia se mantiene como recomendacion, no como enforcement runtime.

## 4.10. Conclusion del capitulo

La solucion desarrollada materializa las tres misiones de seguridad del TFG sin ampliar el alcance al resto de funcionalidades de Theia Officer. El usuario puede conectar proveedores, descubrir agentes, aplicar guardarrailes, generar y aplicar baselines de minimo privilegio, clasificar autonomia y revisar evidencias desde una pagina de seguridad.

El punto mas importante de la implementacion es que combina una experiencia guiada por CAIO con ejecucion controlada por backend. Esto permite que el usuario tenga una interfaz clara sin perder validaciones, trazabilidad ni separacion de responsabilidades.
