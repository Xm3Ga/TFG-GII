##  Hipótesis

El desarrollo de una PWA con arquitectura local-first, autenticación basada en JWT, notificaciones en tiempo real mediante WebSockets y un modelo de datos ajustado a la operativa del Restaurante La Unión permitirá digitalizar por completo la gestión interna del establecimiento. Con ello se eliminarán los problemas derivados del sistema analógico actual y se garantizará que operaciones críticas —como el registro de alérgenos o el seguimiento de los tickets— queden siempre registradas, incluso cuando se produzcan interrupciones puntuales en la red local.

---

##  Objetivos

### Objetivo general

El objetivo principal del proyecto es desarrollar una Progressive Web App con arquitectura local-first que digitalice la gestión operativa del Restaurante La Unión. La aplicación cubrirá los módulos de comandas, cocina (KDS), reservas, mesas, caja y auditoría, con acceso diferenciado según el rol del usuario y capacidad de funcionamiento ante fallos de conectividad.

###  Objetivos específicos

- **Objetivo específico I** *(Capítulo 2 — Requisitos)*: Recopilar, mediante entrevistas con el cliente, un catálogo de al menos 20 casos de uso validados, incluyendo requisitos funcionales y no funcionales numerados, y una tabla de priorización MoSCoW que delimite el alcance del MVP.

- **Objetivo específico II** *(Capítulo 3 — Análisis y diseño)*: Elaborar los artefactos de diseño del sistema: modelo entidad-relación con al menos 8 entidades, diseño de la API REST con el contrato de cada endpoint, esquema de eventos WebSocket, diagrama de arquitectura y prototipos de interfaz por rol.

- **Objetivo específico III** *(Capítulo 4 — MVP)*: Implementar un MVP que cubra el 100 % de los casos de uso clasificados como Must-have, con la arquitectura local-first en funcionamiento, autenticación JWT activa y notificaciones por WebSocket validadas en la red local del restaurante.

---

##  Metodología

El proyecto adopta una metodología iterativa e incremental con un cliente real. El ciclo de vida se basa en un RUP simplificado combinado con prácticas ágiles, organizado en cuatro fases:

- **Fase de inicio** *(Capítulo 2)*: entrevistas con el cliente, extracción de requisitos, elaboración del catálogo de casos de uso y tabla MoSCoW. La fase concluye cuando el cliente valida los requisitos recogidos.

- **Fase de elaboración** *(Capítulo 3)*: traducción de los requisitos en decisiones de diseño concretas: modelo de datos, API REST, eventos WebSocket, arquitectura del sistema y prototipos de interfaz.

- **Fase de construcción** *(Capítulo 4)*: implementación iterativa del MVP, priorizando los casos de uso Must-have. Cada iteración genera software funcional que puede ser validado de forma incremental.

- **Fase de transición** *(Capítulo 5)*: evaluación de los resultados obtenidos frente a la hipótesis planteada, análisis de las limitaciones encontradas y propuesta de líneas de trabajo futuro.

| Fase | Capítulo | Resultado principal |
|---|---|---|
| Inicio | Capítulo 2 | Catálogo de requisitos + MoSCoW |
| Elaboración | Capítulo 3 | Artefactos de diseño |
| Construcción | Capítulo 4 | MVP funcional validado |
| Transición | Capítulo 5 | Conclusiones y líneas futuras |

