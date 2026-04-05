# Priorización de Casos de Uso

| Nº | Caso de uso | Actor | Prioridad |
|----|------------|-------|-----------|
| CdU-01 | Conectar proveedor cloud | Usuario | Alta |
| CdU-02 | Descubrir agentes | Usuario | Alta |
| CdU-03 | Configurar guardarraíl de contenido | Usuario | Alta |
| CdU-04 | Configurar protección anti-jailbreak | Usuario | Alta |
| CdU-05 | Configurar detección de PII | Usuario | Alta |
| CdU-06 | Configurar bloqueo de temas | Usuario | Media |
| CdU-07 | Aplicar guardarraíl a agentes | Usuario | Alta |
| CdU-08 | Analizar privilegios de agentes | Usuario | Media |
| CdU-09 | Aplicar reducción de privilegios | Usuario | Media |
| CdU-10 | Definir niveles de autonomía | Usuario | Media |
| CdU-11 | Configurar supervisión humana | Usuario | Media |

## Justificación de la priorización

### CdU-01: Conectar proveedor cloud — Alta

Es un prerrequisito funcional imprescindible. Sin la conexión a un proveedor cloud, ninguna misión de seguridad puede ejecutarse. Debe desarrollarse en la primera iteración.

### CdU-02: Descubrir agentes — Alta

Es el segundo prerrequisito funcional. La plataforma necesita conocer los agentes desplegados por la organización antes de poder aplicar cualquier política de seguridad sobre ellos.

### CdU-03: Configurar guardarraíl de contenido — Alta

Los filtros de contenido son la funcionalidad de seguridad más demandada y la que mayor impacto tiene en la protección de los agentes. Es el caso de uso de referencia para validar la arquitectura de misiones de guardarraíles.

### CdU-04: Configurar protección anti-jailbreak — Alta

La inyección de prompts es la vulnerabilidad más crítica en sistemas basados en LLM (OWASP, 2025). Ofrecer protección contra este ataque es prioritario para la propuesta de valor de la plataforma.

### CdU-05: Configurar detección de PII — Alta

La divulgación de información personal sensible tiene implicaciones legales directas (RGPD). La detección y anonimización de PII es una funcionalidad esencial para organizaciones que operan en la Unión Europea.

### CdU-06: Configurar bloqueo de temas — Media

El bloqueo de temas es una funcionalidad relevante pero menos crítica que los filtros de contenido y la protección anti-jailbreak. Su implementación puede aplazarse sin comprometer la seguridad básica de los agentes.

### CdU-07: Aplicar guardarraíl a agentes — Alta

Completa el flujo de la misión de guardarraíles. Sin la capacidad de aplicar los guardarraíles configurados a los agentes, las misiones CdU-03 a CdU-06 carecerían de efecto práctico.

### CdU-08: Analizar privilegios de agentes — Media

Es la funcionalidad central de la misión de mínimo privilegio. Su prioridad es media porque requiere un período de observación del comportamiento de los agentes para generar recomendaciones precisas, lo que la hace más adecuada para iteraciones posteriores.

### CdU-09: Aplicar reducción de privilegios — Media

Depende del análisis previo (CdU-08). Se desarrollará en la misma iteración que el análisis de privilegios.

### CdU-10: Definir niveles de autonomía — Media

Es la funcionalidad central de la misión de autonomía. Su prioridad es media porque, aunque es importante, las misiones de guardarraíles ofrecen una protección más inmediata.

### CdU-11: Configurar supervisión humana — Media

Complementa la definición de niveles de autonomía (CdU-10). Se desarrollará conjuntamente con la misión de autonomía en una iteración posterior.
