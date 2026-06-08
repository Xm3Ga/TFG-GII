# 5. CONCLUSIONES, DISCUSION Y LINEAS FUTURAS

Este capitulo cierra el TFG revisando si la solucion desarrollada cumple los objetivos definidos en el capitulo 1, que decisiones han condicionado el resultado y que extensiones tienen sentido despues del MVP.

## 5.1. Cumplimiento de objetivos

| Objetivo especifico | Evidencia de cumplimiento |
| --- | --- |
| OE-1. Definir el dominio, actores, requisitos y casos de uso de las tres misiones. | El capitulo 2 delimita guardarrailes, minimo privilegio y autonomia; separa requisitos de requerimientos; y actualiza actores, casos de uso y glosario. |
| OE-2. Disenar la arquitectura, clases, paquetes, flujos y modelo de datos que soportan las misiones. | El capitulo 3 vincula cada caso de uso con routers, servicios, conectores, modelos y diagramas actualizados. |
| OE-3. Implementar y presentar un MVP funcional alineado con el codigo real. | El capitulo 4 describe la navegacion real, los flujos de mision y la evidencia visible en `/security`. |
| OE-4. Evaluar resultados, limitaciones y futuras lineas de evolucion. | Este capitulo resume resultados, compromisos tecnicos y mejoras razonables. |

El objetivo general tambien se cumple dentro del alcance fijado: se ha construido una solucion que permite guiar misiones de seguridad sobre agentes de IA en proveedores cloud, con controles para guardarrailes, minimo privilegio y autonomia.

## 5.2. Conclusiones principales

| Conclusion | Justificacion |
| --- | --- |
| El enfoque por misiones facilita la operacion de seguridad. | El usuario no tiene que conocer cada API cloud; avanza por hitos y decisiones concretas. |
| La separacion entre CAIO y endpoints tipados es necesaria. | El asistente guia, pero las acciones sensibles pasan por validaciones backend. |
| Los proveedores cloud no ofrecen capacidades equivalentes. | AWS permite Bedrock Guardrails; GCP usa Security Settings para casos concretos; Azure no tiene guardarrailes implementados en el MVP. |
| Minimo privilegio necesita revision humana. | Cambiar IAM/RBAC sin aprobacion puede romper agentes o servicios dependientes. |
| Autonomia debe presentarse como recomendacion en esta fase. | El codigo genera perfiles y validaciones propuestas, pero no intercepta acciones runtime. |

La conclusion mas relevante es que la arquitectura permite convertir necesidades de gobierno de IA en flujos operativos. El usuario no solo recibe un informe: puede aplicar controles, revisar evidencias y mantener trazabilidad.

## 5.3. Discusion de resultados

### Guardarrailes

La mision de guardarrailes es la mas cercana a una accion directa de seguridad. El sistema crea o actualiza recursos nativos y los asocia a agentes. El resultado es tangible, pero tambien muestra una limitacion habitual en entornos multi-cloud: cada proveedor define sus controles de forma distinta.

La decision de ocultar guardarrailes de Azure en el catalogo es correcta para el MVP. Mostrar una mision no implementada como si estuviera disponible generaria una memoria incoherente y una mala experiencia de usuario.

### Minimo privilegio

La mision de minimo privilegio aporta valor porque traduce el principio general de least privilege en baselines revisables por agente. La solucion no se limita a decir que un agente tiene demasiados permisos: conserva permisos actuales, permisos propuestos, removibles, confianza y rollback.

El compromiso principal es que la calidad de la propuesta depende de la evidencia disponible. Si el sistema no puede resolver la identidad runtime o no tiene suficientes datos de uso, debe bloquear o rebajar la confianza de la baseline. Esta decision es preferible a aplicar cambios inseguros.

### Autonomia

La mision de autonomia permite detectar agentes con potencial de actuar sobre dinero, datos, identidades, sistemas externos o produccion. La implementacion actual usa reglas deterministas sobre metadatos, lo que hace que el resultado sea explicable y trazable.

El limite es claro: no existe todavia un enforcement runtime. Por tanto, la solucion no garantiza que una accion sensible quede bloqueada hasta recibir aprobacion humana. Lo que si hace es identificar donde deberia existir esa aprobacion y dejar una propuesta revisable.

## 5.4. Limitaciones

| Limitacion | Consecuencia | Posible mitigacion |
| --- | --- | --- |
| Cobertura desigual de proveedores | No todas las misiones tienen las mismas capacidades en AWS, GCP y Azure. | Documentar soporte por proveedor y ampliar conectores de forma incremental. |
| Autonomia sin enforcement | Las validaciones humanas son propuestas, no bloqueos efectivos. | Integrar un punto de decision runtime o gateway de herramientas. |
| Evidencia dependiente de metadatos | Algunos agentes pueden clasificarse con baja confianza. | Enriquecer inventario con logs, permisos usados y descripciones de herramientas. |
| Persistencia parcial de algunos resultados de guardarrail | Parte de la evidencia de aplicacion depende del estado local de mision y metadatos. | Persistir ejecuciones de guardarrail con entidad propia. |
| MVP orientado a validacion | Requiere endurecimiento antes de produccion. | PostgreSQL, gestion avanzada de secretos, observabilidad, backups y pruebas end-to-end. |

Estas limitaciones no cambian el valor del TFG. Delimitan correctamente hasta donde llega el MVP y evitan atribuirle capacidades que el codigo no tiene.

## 5.5. Recomendaciones

| Recomendacion | Motivo |
| --- | --- |
| Mantener tablas de soporte por proveedor en la documentacion y en la UI. | El usuario debe saber que capacidades existen antes de iniciar una mision. |
| Persistir ejecuciones de guardarrail como entidad propia. | Facilitaria auditoria historica y consultas independientes del estado local. |
| Exigir aprobacion explicita en cualquier accion que modifique IAM/RBAC. | Es una barrera necesaria ante cambios de alto impacto. |
| Separar recomendaciones de autonomia y enforcement runtime. | Ayuda a evolucionar sin prometer control que aun no existe. |
| Automatizar pruebas de los flujos criticos. | Las misiones combinan frontend, backend y proveedores externos; necesitan regresion controlada. |

## 5.6. Futuras lineas de actuacion

| Linea futura | Descripcion |
| --- | --- |
| Guardarrailes Azure | Investigar equivalentes reales y exponerlos solo cuando exista aplicacion segura. |
| Persistencia completa de misiones | Guardar ejecuciones, configuraciones, resultados y fallos de guardarrailes en tablas propias. |
| Gateway de validacion humana | Interceptar herramientas o acciones runtime para aprobar operaciones sensibles antes de ejecutarlas. |
| Evidencia avanzada de minimo privilegio | Incorporar logs de uso mas completos, Access Analyzer, Cloud Audit Logs y Activity Logs segun proveedor. |
| Politicas reutilizables | Permitir plantillas de guardarrail y baselines reutilizables por tipo de agente o entorno. |
| Pruebas end-to-end | Validar flujos de credencial, misiones y evidencias con datos demo y mocks de proveedores. |
| Endurecimiento productivo | Migrar a PostgreSQL, mejorar gestion de secretos, observabilidad, colas y aislamiento multi-tenant. |

La ventaja de haber seguido un proceso metodologico es que estas lineas futuras no son ideas aisladas. Cada una se conecta con una limitacion concreta del analisis, del diseno o de la implementacion.

## 5.7. Cierre

El TFG demuestra que es posible construir un CAIO virtual orientado a misiones de seguridad sin convertirlo en un simple chatbot. Theia Officer combina una interfaz guiada con servicios backend, conectores cloud, modelos persistentes y evidencias por agente.

El resultado final mantiene el alcance definido: tres misiones de seguridad para agentes de IA. Guardarrailes aporta controles directos de conversacion; minimo privilegio aporta reduccion controlada de permisos; y autonomia aporta clasificacion y propuestas de supervision humana. La solucion queda preparada para crecer, pero la memoria distingue con claridad entre lo implementado, lo parcial y lo futuro.
