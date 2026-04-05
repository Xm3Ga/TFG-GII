# **Trabajo final de grado**

Desarrollo de un sistema analítico desacoplado integrado con Odoo para la optimización en la toma de decisiones


Trabajo fin de grado realizado por
**César García Eguiguren**


SANTANDER – 03/2026


# Resumen
La gestión efectiva de proyectos en consultorías tecnológicas requiere transformar datos operativos en indicadores estratégicos que faciliten la toma de decisiones. Netkia, una consultoría informática, utiliza Odoo como sistema ERP central para gestionar proyectos, tareas y recursos, acumulando información valiosa sobre estimaciones, horas imputadas y desviaciones. Sin embargo, esta información no se traduce automáticamente en conocimiento interpretable para directores y responsables de proyectos, quienes carecen de herramientas específicas para analizar desviaciones presupuestarias, evaluar la productividad de recursos y anticipar riesgos de incumplimiento de plazos.

Las soluciones comerciales de Business Intelligence (Power BI, Tableau) presentan limitaciones críticas para este contexto: costes de licenciamiento elevados, ausencia de conectores nativos certificados para Odoo v16 Enterprise, y falta de KPIs especializados para el modelo operativo de la empresa. Igualmente, las opciones open-source (Metabase, Superset) carecen de capas semánticas para definir métricas consistentes y presentan limitaciones funcionales con esquemas complejos.

Este trabajo propone el diseño e implementación de un módulo analítico externo, desacoplado del ERP pero integrado con su base de datos PostgreSQL, que extrae datos operativos, calcula indicadores clave de rendimiento específicamente diseñados para las necesidades directivas, y los presenta mediante paneles de control interactivos e informes exportables. La solución responde a los principios arquitectónicos OLTP/OLAP, garantizando rendimiento operativo sin interferencias en los 110 empleados que utilizan diariamente el sistema transaccional, mientras optimiza costes eliminando licencias recurrentes mediante el uso de tecnologías open-source (Python, FastAPI, React).

El proyecto sigue la metodología RUP organizado en tres fases: *Ingeniería de Requisitos*, definiendo el modelo del dominio y casos de uso; *Análisis, Diseño e Implementación* de la arquitectura en capas con datos simulados y por último *Validación y Evaluación* frente a los requisitos. La solución sienta las bases para evoluciones futuras hacia análisis predictivo e integración con sistemas conversacionales corporativos.

**Palabras clave:** Analítica de datos, Business Intelligence, Odoo, ERP, KPIs, toma de decisiones, módulo desacoplado, gestión de proyectos, visualización de datos.

# Índice

- [Introducción](#introducción)
  - [Contextualización del entorno empresarial](#contextualización-del-entorno-empresarial)
  - [Problema identificado y motivación](#problema-identificado-y-motivación)
  - [Alcance y limitaciones del trabajo](#alcance-y-limitaciones-del-trabajo)
- [Marco teórico](#marco-teórico)
  - [Justificación](#justificación)
    - [Contextualización del escenario](#contextualización-del-escenario)
    - [Exploración de soluciones existentes (Estado del Arte)](#exploración-de-soluciones-existentes)
      - [Plataformas de Business Intelligence Empresarial](#1-plataformas-de-business-intelligence-empresarial)
        - [Microsoft Power BI](#11-microsoft-power-bi)
        - [Tableau](#12-tableau)
      - [Módulos Analíticos Integrados en ERPs](#2-módulos-analíticos-integrados-en-erps)
        - [Odoo Enterprise v16 - Studio y Dashboards Avanzados](#21-odoo-enterprise-v16---studio-y-dashboards-avanzados)
      - [Soluciones de Código Abierto Adicionales](#3-soluciones-de-código-abierto-adicionales)
        - [Apache Superset](#31-apache-superset)
        - [Metabase (Open Source)](#32-metabase-open-source)
    - [Por qué las soluciones existentes no son suficientes](#por-qué-las-soluciones-existentes-no-son-suficientes)
  - [Solución propuesta](#solución-propuesta)
    - [Proyección futura hacia análisis predictivo](#proyección-futura-hacia-análisis-predictivo)
  - [Objetivos General y Específicos](#objetivos-general-y-específicos)
  - [Metodología y Tareas](#metodología-y-tareas)
    - [Fase 1 – Ingeniería de Requisitos](#fase-1--ingeniería-de-requisitos)
    - [Fase 2 – Análisis, Diseño e Implementación](#fase-2--análisis-diseño-e-implementación)
    - [Fase 3 – Validación y Evaluación](#fase-3--validación-y-evaluación)
  - [Estructura del trabajo](#estructura-del-trabajo)
- [Requerimientos y requisitos](#requerimientos-y-requisitos)
- [Análisis y diseño](#análisis-y-diseño)
- [Descripción de la solución propuesta](#descripción-de-la-solución-propuesta)
- [Conclusiones](#conclusiones)
- [Referencias bibliográficas](#referencias-bibliográficas)
- [Anexos](#anexos)

# Introducción
## Contextualización del entorno empresarial
La gestión eficiente de proyectos en el ámbito de una consultoría tecnológica constituye un pilar fundamental para buscar rentabilidad y competitividad en el mercado. Bajo este contexto, Netkia es una consultoría informática que opera con una serie de proyectos simultáneos y atiende a múltiples clientes con diversas necesidades y plazos; para ello es necesario gestionar correctamente los distintos recursos, así como llevar a cabo un control del trabajo realizado para garantizar un correcto funcionamiento a nivel empresarial.

Por ello, Netkia emplea un sistema de planificación de recursos empresariales (ERP) llamado Odoo como plataforma central para la gestión de sus proyectos, lo que les permite asignar tareas, registrar horas imputadas y realizar un seguimiento del avance, ya que cada empleado registra sus partes de horas justificando el trabajo realizado cada día. Este sistema constituye una fuente de datos de gran valor, pues registra información relativa a tareas, proyectos y empleados, así como las estimaciones de tiempo asignadas a cada tarea.

Sin embargo, toda esta información no se traduce en conocimiento si no se utiliza de manera adecuada. La dirección y los responsables de los proyectos no solo necesitan conocer el estado actual de cada proyecto, sino también identificar desviaciones respecto a las estimaciones iniciales, analizar la productividad de los recursos humanos y anticipar posibles riesgos de incumplimiento de plazos o presupuestos.

## Problema identificado y motivación
El principal problema que motiva el presente Trabajo de Fin de Grado surge de una necesidad observable en el entorno empresarial descrito: la ausencia de herramientas de analítica específicas que permitan transformar los datos operativos almacenados en el ERP en indicadores de rendimiento significativos, orientados a la toma de decisiones y al apoyo diario de los responsables de proyecto.

En la situación actual, los responsables de Netkia carecen de un mecanismo que les permita responder de manera ágil a preguntas como: ¿cuántas horas se han imputado a un proyecto en comparación con lo estimado?, ¿qué empleados presentan una mayor carga de trabajo hoy?, o ¿existen proyectos con riesgo de desviación presupuestaria? Esta información existe en el sistema, pero no está consolidada ni presentada de forma interpretable.

Las soluciones comerciales de Business Intelligence disponibles en el mercado como Power BI o Tableau presentan limitaciones importantes para el contexto específico de Netkia, debido a su elevado coste de licenciamiento, la complejidad de integración con la versión de Odoo empleada y la imposibilidad de definir métricas e indicadores a medida que respondan a las particularidades de la gestión de proyectos de la empresa.

Esta situación justifica el desarrollo de una solución a medida: un módulo de analítica externo, desacoplado del ERP pero capaz de integrarse con él, que permita calcular y visualizar indicadores clave de rendimiento adaptados específicamente al modelo operativo de la empresa.

## Alcance y limitaciones del trabajo
El presente trabajo abarca el ciclo completo de ingeniería de software aplicado a la construcción del módulo de analítica descrito, desde la captura de requisitos hasta la implementación de un prototipo funcional. El alcance del proyecto comprende:
- La definición de requisitos del módulo, siguiendo las disciplinas de la ingeniería de requisitos.
- El diseño de la arquitectura del sistema, incluyendo el modelo de dominio, el modelo lógico de datos y los componentes principales.
- La definición, cálculo y validación de un conjunto de KPIs orientados a la gestión de proyectos en el contexto de la empresa.
- La implementación de un prototipo funcional que integre los módulos de extracción, procesamiento y visualización de datos.

En cuanto a las limitaciones, el desarrollo se realizará íntegramente sobre datos simulados que modelan la estructura del ERP real, sin acceder a información confidencial de clientes o proyectos de Netkia. El prototipo resultante para la empresa es un sistema listo para producción. Para el trabajo de fin de grado, queda fuera del alcance actual la integración en tiempo real con el ERP, la gestión avanzada de roles y autenticación, y el desarrollo  de un módulo predictivo basado en aprendizaje automático, que actualmente se identifican como líneas de trabajo futuro.

[⬆️ Volver al índice](#índice)

# Marco teórico
## Justificación
### Contextualización del escenario
La gestión de proyectos en el sector de las consultorías tecnológicas exige coordinar simultáneamente proyectos y recursos de manera eficiente. Como se ha expuesto en la introducción, Netkia opera con Odoo como ERP central, un sistema que acumula de forma continua datos operativos de alto valor: horas estimadas y reales por tarea, estados del flujo de trabajo, asignaciones de empleados y costes asociados. El problema no reside en la ausencia de datos, sino en que esos datos no están transformados en información útil para quienes deben tomar decisiones sobre los proyectos.

### Exploración de soluciones existentes

El mercado ofrece varias categorías de herramientas que, en principio, podrían cubrir la necesidad de análisis de datos para gestión de proyectos en empresas como Netkia. A continuación se presenta un análisis exhaustivo de las principales alternativas disponibles, organizadas en tres categorías: plataformas de Business Intelligence empresarial, módulos analíticos integrados en ERPs, y soluciones de código abierto para analítica.

#### 1. Plataformas de Business Intelligence Empresarial

##### 1.1 Microsoft Power BI

**Descripción general:**
Power BI es una plataforma integral de Business Intelligence desarrollada por Microsoft que permite conectar múltiples fuentes de datos, crear modelos analíticos complejos y generar visualizaciones interactivas (Microsoft, 2024). Según Gartner (2024), Power BI se posiciona como líder en el cuadrante mágico de plataformas de análisis y BI, destacando por su integración con el ecosistema Microsoft.

**Ventajas:**
- **Ecosistema consolidado:** Más de 250,000 organizaciones utilizan Power BI globalmente, garantizando madurez tecnológica y amplia comunidad de soporte (Microsoft, 2023)
- **Flexibilidad analítica avanzada:** El motor DAX (Data Analysis Expressions) proporciona más de 250 funciones para crear métricas personalizadas complejas (Ferrari & Russo, 2019)
- **Integración con Microsoft 365:** Publicación nativa en SharePoint, Teams y Excel, reduciendo fricción en organizaciones que ya utilizan el ecosistema Microsoft (Aspin, 2020)
- **Escalabilidad probada:** Soporta desde pequeños datasets hasta grandes volúmenes mediante modelos DirectQuery
- **Actualizaciones automáticas:** Programación de refrescos hasta 48 veces/día en versión Premium

**Limitaciones para Netkia:**
- **Coste significativo:** Power BI Pro requiere $10 USD/usuario/mes. Para uso directivo y responsables de proyecto en Netkia (~15-20 usuarios): $1,800-2,400 USD/año (Microsoft, 2024)
- **Infraestructura adicional:** Power BI Gateway requiere servidor Windows dedicado (~$1,000-2,000 adicionales)
- **Ausencia de conector para Odoo v16 Enterprise:** No existe conector certificado, obligando a conexión directa a PostgreSQL con más de 1,000 tablas o desarrollo de API intermedia personalizada (Odoo S.A., 2024)
- **Curva de aprendizaje pronunciada:** Tiempo medio de proficiencia en DAX: 3-6 meses para usuarios técnicos (Kimball Group, 2022)
- **Optimización continua necesaria:** Queries complejas sin optimización pueden superar 30 segundos de tiempo de respuesta (Ferrari & Russo, 2019)
- **Vendor lock-in:** Migración a otras plataformas implica reescritura completa de lógica DAX y visualizaciones (Aspin, 2020)

##### 1.2 Tableau

**Descripción general:**
Tableau, adquirida por Salesforce en 2019, es reconocida como líder en visualización de datos por Gartner. Se caracteriza por su capacidad de crear visualizaciones altamente interactivas mediante interfaz drag-and-drop y su motor Hyper para procesamiento in-memory de alto rendimiento (Tableau Software, 2024).

**Ventajas:**
- **Excelencia en visualización:** Más de 24 tipos de gráficos nativos con capacidad de crear visualizaciones personalizadas (Murray, 2016)
- **Rendimiento optimizado:** Motor Hyper procesa consultas hasta 3x más rápido que generación anterior (Tableau Software, 2018)
- **Análisis exploratorio ágil:** Feature "Show Me" sugiere visualizaciones óptimas según tipos de datos
- **Interfaz intuitiva:** Drag-and-drop facilita creación de análisis complejos sin código
- **Comunidad activa:** Tableau Public con más de 3 millones de visualizaciones como referencia

**Limitaciones para Netkia:**
- **Modelo de costes elevado:** Para uso directivo (~5 Creators + 15 Explorers): $4,200/año + $7,560/año = **$11,760 USD/año**, más infraestructura Tableau Server (~$2,500 iniciales + hardware) (Tableau Software, 2024)
- **Sin conector nativo para Odoo:** Requiere conexión ODBC/JDBC directa con mapeo manual del esquema complejo o desarrollo de Web Data Connector personalizado (Murray, 2016)
- **Enfoque en visualización sobre lógica:** Optimizado para exploración visual, no para implementar reglas de negocio complejas; KPIs sofisticados requieren procesamiento previo externo (Sleeper & Pulley, 2018)
- **Gestión de versiones limitada:** Tableau Server no incluye Git nativo
- **Rendimiento con transformaciones complejas:** Consultas con múltiples joins sobre esquema de Odoo pueden superar 10-15 segundos

[⬆️ Volver al índice](#índice)

#### 2. Módulos Analíticos Integrados en ERPs

##### 2.1 Odoo Enterprise v16 - Studio y Dashboards Avanzados

**Descripción general:**
Odoo Enterprise v16 incluye capacidades analíticas nativas mediante Odoo Studio (constructor visual no-code) y Dashboards avanzados (paneles configurables con KPIs y gráficos). Más de 12 millones de usuarios utilizan Odoo en sus diferentes versiones (Odoo S.A., 2024).

**Ventajas:**
- **Integración nativa total:** Acceso directo al ORM de Odoo; actualizaciones en tiempo real automáticas
- **Experiencia unificada:** Misma interfaz que sistema transaccional; curva de aprendizaje mínima
- **Seguridad heredada:** Security rules de Odoo aplican automáticamente a reportes
- **Deployment simplificado:** Sin infraestructura adicional ni sincronización de datos
- **Desarrollo rápido:** Odoo Studio permite crear reportes simples en minutos

**Limitaciones críticas para Netkia:**
- **Coste de licenciamiento elevado:** Odoo Enterprise requiere licencia para **todos los usuarios activos del sistema**, no solo para quienes usen funcionalidades analíticas. Precio típico: €24-€35/usuario/mes en Europa. Para Netkia con 110 empleados que deben acceder al ERP para registrar horas, gestionar tareas y proyectos: 110 usuarios × €28/mes (promedio) = €3,080/mes = **€36,960/año (~$40,000 USD/año)**. Este coste representa el incremento total desde Odoo Community (€0) a Enterprise para toda la operación (Odoo S.A., 2024)
- **Modelo de licenciamiento indivisible:** No es posible contratar Odoo Enterprise solo para los 15-20 directores y responsables que necesitan analítica; todos los 110 empleados operativos requieren licencia Enterprise
- **Ausencia de KPIs específicos de consultoría:** Dashboards incluyen métricas genéricas (total horas, ingresos, tareas completadas); no incluyen indicadores especializados como índice de precisión de estimación por responsable o distribución estratégica de horas
- **Complejidad de personalización:** Implementar KPIs específicos requiere desarrollo de campos computados en Python con mantenimiento en cada upgrade o crear modelos analíticos dedicados con complejidad equivalente a módulo externo
- **Studio con limitaciones funcionales:** No permite lógica de negocio compleja; cálculos limitados a operaciones básicas (sum, avg, count); sin ventanas analíticas SQL
- **Impacto en rendimiento del sistema transaccional:** Odoo diseñado como sistema OLTP (optimizado para escrituras), no OLAP (optimizado para lectura analítica). Ejecutar análisis complejos en misma instancia genera contención de recursos y locks de PostgreSQL (Kimball & Ross, 2013; Kleppmann, 2017)
- **Violación de principio de separación:** Mejores prácticas arquitectónicas recomiendan separación física entre sistemas OLTP y OLAP (Kimball & Ross, 2013)
- **Overhead del ORM:** El ORM añade 2-3x latencia vs. SQL directo; generación de SQL subóptimo (Odoo S.A., 2024)
- **Escalabilidad limitada:** Con 200,000 registros/año, tras 5 años (1,000,000+ registros), dashboards presentan timeouts en sistema transaccional
- **Vendor lock-in total:** Migración de Enterprise a Community tras adopción es técnicamente compleja

[⬆️ Volver al índice](#índice)

#### 3. Soluciones de Código Abierto Adicionales

##### 3.1 Apache Superset

**Descripción general:**
Apache Superset es una plataforma open-source de exploración y visualización de datos desarrollada originalmente por Airbnb. Diseñada para ser intuitiva manteniendo capacidades avanzadas para analistas (Apache Software Foundation, 2024).

**Ventajas:**
- **Licencia Apache 2.0:** Totalmente gratuita sin restricciones comerciales
- **Arquitectura flexible:** Soporta múltiples bases de datos (PostgreSQL, MySQL, BigQuery)
- **Comunidad activa:** Más de 58,000 estrellas en GitHub; contribuciones de empresas como Airbnb, Netflix

**Limitaciones para Netkia:**
- **Conector genérico para PostgreSQL:** Sin integración específica para Odoo; requiere mapeo manual del esquema complejo
- **Reportes de bajo rendimiento:** Queries complejas sobre esquema Odoo pueden tardar 30-60 segundos
- **Conectividad Nativa Limitada a Fuentes de Datos:** Algunos usuarios han informado que Apache Superset no se conecta de manera nativa a todas las fuentes de datos, requiriendo el uso de controladores o conectores de terceros, lo que puede llevar tiempo configurar.
- **Falta de Capacidades Analíticas Avanzadas:** Aunque Apache Superset ofrece muchas características poderosas para la visualización y exploración de datos.  no es la mejor herramienta para usuarios que necesitan realizar análisis estadísticos complejos o modelado predictivo debido a la falta de funciones analíticas avanzadas integradas.
- **Complejidad de instalación:** Requiere Python, Redis, base de datos de metadatos, workers Celery; deployment más complejo que Metabase
- **Curva de aprendizaje técnica:** Interfaz orientada a usuarios con conocimientos SQL
- **Ausencia de capa semántica nativa:** Sin modelado de métricas reutilizable directo
- **Mantenimiento continuo:** Actualizaciones frecuentes; testing de compatibilidad necesario con esquema modificado de Odoo
- **Documentación menos exhaustiva:** Comparada con soluciones comerciales

##### 3.2 Metabase (Open Source)

**Descripción general:**
Metabase es una plataforma open-source de Business Intelligence enfocada en la democratización del acceso a datos. Con más de 42,000 estrellas en GitHub y 50,000+ instalaciones documentadas, representa la alternativa open-source más popular para equipos que buscan soluciones sin coste de licenciamiento (Metabase, 2024).

**Ventajas:**
- **Coste cero de licenciamiento:** Edición Community completamente gratuita bajo licencia AGPL v3, sin límites de usuarios o dashboards
- **Facilidad de deployment:** Instalación mediante Docker en menos de 10 minutos
- **Query Builder visual:** Interfaz no-code para usuarios de negocio con filtros y agregaciones simples
- **Conexión nativa a PostgreSQL:** Conector directo sin capas intermedias, acceso en tiempo real a datos de Odoo
- **Permisos granulares:** Control de acceso por colecciones, tablas y columnas
- **Comunidad activa:** Discourse forum con más de 10,000 miembros

**Limitaciones para Netkia:**
- **Query Builder visual limitado:** No soporta subqueries en interfaz gráfica; joins limitados a 3 tablas; sin ventanas analíticas (OVER, PARTITION BY)
- **Ausencia de capa semántica:** Cada dashboard debe redefinir KPIs complejos, generando riesgo de inconsistencias
- **Limitaciones de visualización:** Solo 15 tipos de gráficos nativos; personalización visual limitada
- **Sin motor in-memory propio:** Todas las queries se ejecutan directamente en PostgreSQL; dashboards con 10 gráficos pueden tardar 15-30 segundos en cargar
- **Sistema de caché básico:** Caché por TTL fijo sin invalidación inteligente
- **Complejidad con esquema Odoo:** Query Builder no abstrae estructura compleja; usuarios deben entender tablas técnicas como `project_task_user_rel`
- **Sin soporte oficial:** Edición Community sin soporte; resolución de bugs la mediante comunidad
- **Gobernanza limitada:** Sin auditoría nativa de cambios en queries/dashboards

[⬆️ Volver al índice](#índice)

### Por qué las soluciones existentes no son suficientes

El análisis de las soluciones existentes revela limitaciones sistemáticas en el contexto específico de Netkia:

**Coste económico desproporcionado para necesidad analítica específica:** Aunque el módulo analítico solo será utilizado por directores y responsables de proyecto (~15-20 usuarios), las plataformas comerciales representan costes significativos: Power BI ($1,800-2,400/año para usuarios analíticos), Tableau ($11,760/año). Odoo Enterprise presenta el coste más elevado: ~$40,000/año ya que requiere licenciar a todos los 110 empleados operativos del sistema, no solo a los usuarios de analítica. Este modelo de licenciamiento hace inviable justificar la migración a Enterprise únicamente por capacidades analíticas, especialmente considerando sus limitaciones funcionales para KPIs específicos de consultoría.

**Complejidad de integración universal:** Ninguna solución ofrece conector certificado para Odoo v16 Enterprise con modificaciones personalizadas. Todas requieren: (1) mapeo manual del esquema PostgreSQL complejo; (2) gestión de tablas relacionales mediante tablas puente; (3) replicación de lógica de campos computados; o (4) desarrollo de capas intermedias.

**Ausencia de KPIs específicos:** Las métricas predefinidas responden a casos genéricos, no a indicadores estratégicos necesarios para directores como: índice de precisión de estimación por responsable, coeficiente de desviación ponderado por criticidad de cliente, distribución estratégica de horas por tipología de trabajo, análisis de carga por empleado, o eficiencia comparativa entre equipos/proyectos.

**Impacto en rendimiento:** Las soluciones integradas en Odoo Enterprise violan el principio de separación entre sistemas OLTP (transaccional) y OLAP (analítico). Según Kimball & Ross (2013) y Kleppmann (2017), ejecutar cargas analíticas intensivas en la misma instancia que procesa operaciones críticas genera contención de recursos y degradación de experiencia para los 110 empleados operativos.

**Limitaciones de las soluciones open-source:** Si bien Metabase y Superset eliminan costes de licenciamiento, presentan limitaciones críticas: (1) ausencia de capa semántica para definir KPIs consistentes; (2) Query Builder limitado que requiere SQL avanzado; (3) sin motor in-memory propio generando queries lentas; (4) complejidad operativa con esquema Odoo no abstraído.

Dado que actualmente existe una brecha entre los datos que los sistemas de gestión de proyectos generan y la información que los directivos de Netkia necesitan para tomar decisiones, cerrar esa brecha con herramientas genéricas implica adaptarse a sus restricciones; hacerlo con una solución a medida permite que sea la herramienta la que se adapte a la organización.

[⬆️ Volver al índice](#índice)

## Solución propuesta

En base al análisis del estado del arte y las limitaciones identificadas, la solución propuesta en este trabajo es un módulo externo a Odoo, desacoplado del ERP, pero conectado a la base de datos del mismo; la cual es PostgreSQL, permitiendo realizar consultas avanzadas sin interferir en el flujo de datos operativo de los 110 empleados. 

El módulo extrae los datos operativos de dicha base de datos, los procesa para calcular un conjunto de KPIs diseñados específicamente para las necesidades de los directores y responsables de proyecto, y los presenta a través de dos mecanismos de salida: un panel de control interactivo orientado a la supervisión operativa diaria, y un sistema de informes exportables orientado al análisis histórico y la toma de decisiones estratégicas.

La solución propuesta resuelve específicamente las limitaciones identificadas:

**Potencial comercial y replicabilidad:** Más allá del uso inmediato en Netkia, el módulo está diseñado con una arquitectura suficientemente modular y generalizable que permite una fácil adaptación y venta a otras empresas con operativas similares. Esta capacidad de replicabilidad convierte el proyecto en un activo empresarial duradero, generando oportunidades de ingresos adicionales mediante la comercialización de variantes del producto a terceros. Este enfoque contrasta directamente con las soluciones propietarias de terceros, donde Netkia no tendría ningún control sobre el producto ni capacidad de derivar ingresos adicionales de su adopción. Al construir una solución propia, la empresa no solo resuelve su necesidad inmediata sino que genera un activo intelectual reutilizable, fomentando la independencia tecnológica frente a proveedores externos y alineándose con el modelo de negocio de una consultoría que busca ofrecer soluciones especializadas a sus clientes.

**Coste optimizado sin licencias recurrentes:** Desarrollo único mediante tecnologías open-source (Python, FastAPI, PostgreSQL, React) elimina costes de licenciamiento perpetuos. Sin coste adicional por usuario, permitiendo acceso ilimitado a directores y responsables sin restricciones económicas. La inversión inicial de desarrollo se amortiza en 1-2 años comparado con soluciones comerciales.

**Integración nativa con esquema real de Odoo modificado:** Acceso directo a PostgreSQL mediante SQLAlchemy permite mapear exactamente las tablas y relaciones utilizadas por Netkia, incluyendo modificaciones personalizadas específicas de la empresa. No requiere abstracciones forzadas por conectores genéricos.

**KPIs diseñados específicamente para supervisión directiva:** Los indicadores se definen mediante lógica Python personalizada respondiendo exactamente a las necesidades de directores y responsables de proyecto: precisión de estimación por responsable, desviación por proyecto, distribución de carga por empleado, eficiencia por tipología de cliente, alertas de sobrecarga, análisis de rentabilidad real vs. estimada.

**Separación arquitectónica garantizando rendimiento operativo:** El módulo opera sobre conexión dedicada de solo lectura, eliminando completamente el impacto en el sistema transaccional de Odoo utilizado por los 110 empleados. Los directores pueden consultar análisis complejos sin afectar el registro de horas, creación de tareas o facturación. Cumple con el patrón arquitectónico recomendado de separación OLTP/OLAP (Kimball & Ross, 2013).

**Control total sobre evolución:** Arquitectura propia permite evolucionar hacia capacidades predictivas sin dependencia de roadmap de proveedores externos. Integración futura con MCP corporativo existente en Netkia factible mediante APIs controladas internamente.

**Escalabilidad sin restricciones de licenciamiento:** El sistema puede añadir nuevos KPIs, optimizar queries, o incorporar más usuarios directivos sin incremento de costes por volumen o usuarios.

**Mantenibilidad con tecnologías estándar:** Python, FastAPI, PostgreSQL y React son tecnologías ampliamente adoptadas con abundante talento disponible en el mercado, reduciendo riesgo de dependencia de perfiles altamente especializados.

Por lo tanto, el proyecto no consiste únicamente en la implementación de un panel de visualización, sino en el diseño de una arquitectura desacoplada, compuesta por una capa de integración con la base de datos PostgreSQL, una capa de lógica de negocio responsable del cálculo de KPIs estratégicos y una capa de presentación orientada a la explotación operativa y directiva de la información.

### Proyección futura hacia análisis predictivo
Este proyecto puede integrarse con el MCP de la empresa como fuente de información estratégica, permitiendo que los indicadores calculados (KPIs, métricas de desviación y análisis históricos) sean consultables mediante lenguaje natural a través del sistema conversacional existente. De esta manera, el sistema no solo ofrecerá un panel de control para la empresa, sino que podrá combinarse junto a un modelo de contexto permitiendo el acceso a la información analítica mediante un chat, ampliando los canales de explotación de los datos sin necesidad de desarrollar una nueva interfaz de chat independiente.

Adicionalmente, esta arquitectura sienta las bases para una posible evolución del sistema hacia modelos de análisis predictivo basados en aprendizaje automático. La acumulación histórica de datos de proyectos incluyendo estimaciones iniciales, desviaciones reales, tipologías de cliente y perfiles de responsables permitiría entrenar modelos capaces de identificar patrones recurrentes asociados a retrasos o sobrecostes.

En una fase posterior, estos modelos podrían integrarse no solo con el MCP, sino también en la capa de lógica de negocio para estimar probabilidades de riesgo en proyectos en curso, evolucionando desde un enfoque descriptivo (qué ha ocurrido) hacia un enfoque predictivo (qué es probable que ocurra). Esta ampliación reforzaría el carácter estratégico del sistema, convirtiéndolo en una herramienta no solo de supervisión, sino también de anticipación y apoyo a la planificación.

[⬆️ Volver al índice](#índice)

## Objetivos General y Específicos
La hipótesis de partida del presente trabajo sostiene que la implementación de un módulo analítico externo, desacoplado del sistema transaccional de Odoo, permitirá mejorar significativamente la capacidad de interpretación de los datos operativos de Netkia mediante la generación de indicadores específicos adaptados a su modelo de gestión, reduciendo la incertidumbre en el seguimiento de proyectos y facilitando la detección temprana de desviaciones en coste y planificación.

El objetivo general del presente TFG es desarrollar un módulo software externo orientado al análisis de datos de un sistema de gestión de proyectos, que permita generar métricas e indicadores clave de rendimiento para apoyar la toma de decisiones y mejorar la gestión de proyectos en una empresa de consultoría tecnológica.

Los objetivos específicos se corresponden con las disciplinas del Proceso Unificado aplicadas al proyecto y se materializan en los capítulos indicados:
  - OE1 — Definir y analizar los requisitos funcionales y no funcionales del módulo de analítica, a partir del estudio del contexto organizativo de Netkia y las necesidades de información de sus responsables de proyecto y dirección.
  - OE2 — Diseñar la arquitectura del sistema y el modelo de datos, definiendo componentes, flujos de información y mecanismos de integración con Odoo, garantizando escalabilidad y mantenibilidad. 
  - OE3 — Implementar un prototipo funcional que permita el procesamiento, almacenamiento y visualización de la información mediante paneles de control e informes exportables, y validar su funcionamiento frente a los requisitos definidos. 

[⬆️ Volver al índice](#índice)

## Metodología y Tareas
Para realizar el desarrollo del proyecto se adoptará la metodología RUP; un marco iterativo e incremental que organiza el trabajo en disciplinas: requisitos, análisis, diseño, implementación y pruebas, debido a la complejidad del sistema y a la necesidad de gestionar de forma estructurada los riesgos asociados a su arquitectura, garantizar la integridad de los datos y la evolución futura.

Uno de los factores determinantes en la elección de RUP es su orientación explícita a la gestión de riesgos. En el presente proyecto existen riesgos técnicos relevantes, tales como:
- El impacto en el rendimiento derivado del acceso a la base de datos.
- La correcta definición y validación de los indicadores estratégicos.
- La consistencia, integridad y trazabilidad de los datos extraídos.
- La mantenibilidad y escalabilidad futura del sistema.

RUP permite abordar estos riesgos desde las primeras iteraciones, especialmente durante las fases de Inicio y Elaboración, donde se define y valida la arquitectura base antes de avanzar hacia la construcción completa del sistema. 

Además, podemos diferenciar tres fases que se corresponden directamente con los objetivos específicos. Aunque las fases se presentan de forma secuencial, el proceso es iterativo: los artefactos de cada fase pueden revisarse a medida que avanza el trabajo.

### Fase 1  Ingeniería de Requisitos
Comprende el análisis del contexto organizativo de Netkia; la construcción del modelo del dominio mediante identificación de entidades, relaciones y glosario de términos; la identificación, priorización y especificación detallada de casos de uso con flujos principales y alternativos; y la definición de requisitos funcionales y no funcionales con su matriz de trazabilidad. El criterio de transición a la siguiente fase es la validación de los casos de uso con el contexto real de la empresa.
### Fase 2  Análisis, Diseño e Implementación
En la subfase de análisis y diseño se elabora la arquitectura del sistema mediante diagramas UML  y se especifican formalmente los KPIs a implementar, aplicando los principios de bajo acoplamiento, alta cohesión y separación de responsabilidades. El criterio de transición a la implementación es la aprobación del diseño arquitectónico. 

En la subfase de implementación el prototipo se construye de forma incremental: primero el modelo de datos con datos simulados, después los algoritmos de cálculo de KPIs, y finalmente la capa de visualización con paneles e informes. Se utilizará control de versiones (Git) para la gestión del código fuente. 
### Fase 3  Validación y Evaluación
Evaluación del sistema frente a los requisitos de la Fase 1. Comprende la ejecución de pruebas funcionales con escenarios simulados representativos, el análisis de los resultados obtenidos por los KPIs implementados y la documentación de conclusiones, limitaciones y recomendaciones. El criterio de finalización es que el prototipo responde satisfactoriamente a los casos de uso definidos. 

[⬆️ Volver al índice](#índice)

## Estructura del trabajo
El documento se organiza en los siguientes capítulos: 

El capítulo 1, Introducción, contextualiza el entorno empresarial de Netkia, presenta el problema identificado y define el alcance y las limitaciones del trabajo. 

El capítulo 2, Marco teórico, se justifica el desarrollo mediante una narrativa de cuatro etapas, revisa el estado del arte, formula la hipótesis, los objetivos, la metodología y la estructura del documento. 

El capítulo 3, Requerimientos y requisitos, materializa el OE1, incluyendo el modelo del dominio, la especificación de casos de uso y los requisitos funcionales y no funcionales. 

El capítulo 4, Análisis y diseño, materializa el OE2 con la arquitectura en capas, el modelo de datos y los diagramas UML. 

El capítulo 5, Conclusiones, recoge los resultados, las limitaciones y las futuras líneas de actuación. Finalmente, se incluye la lista de referencias bibliográficas en formato APA y los anexos con material complementario.

[⬆️ Volver al índice](#índice)

# Requerimientos y requisitos
Este capítulo debe plantear el levantamiento de los requisitos de usuario que gobierna las siguientes etapas de la ejecución de la solución presentada en el TFG.
En el contexto del grado de Ingeniería Informática aquí se recogen los requisitos de usuario, especificación de requerimientos, requerimientos funcionales y no funcionales, entre otros, adecuadamente expresados de acuerdo a la metodología aprendida a lo largo del grado.

[⬆️ Volver al índice](#índice)

# Análisis y diseño
El capítulo Análisis y Diseño toma los requisitos descritos en el capítulo anterior y define los elementos que componen la solución presentada en el TFG. En este capítulo se presentan, entre otros, diagramas de comportamiento (actividades, casos de uso, secuencia y comunicación), diagramas de arquitectura, despliegue, paquetes, objetos, clases de diseño, DER y modelo físico.


[⬆️ Volver al índice](#índice)

# descripción de la solución propuesta

Este capítulo presenta de la manera más detallada posible la solución desarrollada a partir de los elementos expuestos en los capítulos anteriores. 

[⬆️ Volver al índice](#índice)

# Conclusiones
El capítulo Conclusiones expone una breve reflexión sobre la solución presentada, los resultados obtenidos en su aplicación, así como una alusión a las limitaciones que ha sufrido el estudio por diversos motivos (restricciones de espacio, poca accesibilidad a ciertos tipos textuales, etc.), recomendaciones y posibles líneas de investigación que podrían mejorar o incrementar el alcance del estudio llevado a cabo en el TFG.

[⬆️ Volver al índice](#índice)

# Referencias bibliográficas

Apache Software Foundation. (2024). *Apache Superset documentation.* https://superset.apache.org/docs/intro

Aspin, A. (2020). *Pro Power BI architecture: Sharing, security, and deployment options for Microsoft Power BI solutions.* Apress.

Ferrari, A., & Russo, M. (2019). *The definitive guide to DAX: Business intelligence with Microsoft Excel, SQL Server Analysis Services, and Power BI* (2nd ed.). Microsoft Press.

Gartner. (2024). *Magic Quadrant for Analytics and Business Intelligence Platforms.* Gartner Research.

Kimball, R., & Ross, M. (2013). *The data warehouse toolkit: The definitive guide to dimensional modeling* (3rd ed.). Wiley.

Kimball Group. (2022). *Business intelligence adoption patterns and learning curves.* Kimball Group Research Papers.

Kleppmann, M. (2017). *Designing data-intensive applications: The big ideas behind reliable, scalable, and maintainable systems.* O'Reilly Media.

Metabase. (2024). *Metabase open source analytics documentation.* https://www.metabase.com/docs/latest/

Microsoft. (2023). *Microsoft Power BI adoption statistics.* Microsoft Corporation Annual Report.

Microsoft. (2024). *Power BI pricing and licensing.* https://powerbi.microsoft.com/en-us/pricing/

Murray, A. (2016). *Tableau your data! Fast and easy visual analysis with Tableau Software.* Wiley.

Odoo S.A. (2024). *Odoo 16 Enterprise documentation and technical reference.* https://www.odoo.com/documentation/16.0/

Sleeper, R., & Pulley, S. (2018). *Practical Tableau: 100 tips, tutorials, and strategies from a Tableau Zen Master.* O'Reilly Media.

Tableau Software. (2018). *Hyper: A high-performance database engine for Tableau.* Tableau Technical Whitepaper.

Tableau Software. (2024). *Tableau platform documentation and pricing.* https://www.tableau.com/


# Anexos
En este capítulo se deberán incluir los materiales complementarios que se consideran inadecuados incluir en el cuerpo del trabajo por razones de espacio o de continuidad, pero se consideran relevantes para el lector potencial del TFG y su evaluación.
Los anexos que se incluyan en este apartado aparecerán por orden de aparición en el texto principal, y deberán estar nombrados de manera adecuada para una identificación rápida y eficaz.
Este apartado no se contabilizará en el cómputo de páginas.