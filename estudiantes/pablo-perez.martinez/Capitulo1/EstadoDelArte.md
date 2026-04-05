# Estado del arte

## MES (Manufacturing Execution Systems) e indicador OEE (Overall Equipment Effectiveness)

La medición de la eficiencia productiva es clave para evaluar el rendimiento real de los 
sistemas de producción. Además, permite detectar pérdidas y establecer acciones de mejora 
basadas en datos a partir de la información generada durante la ejecución. Los indicadores de 
rendimiento ayudan a detectar ineficiencias y a orientar acciones de optimización del proceso 
(Ng Corrales et al., 2020). 

Entre los indicadores más utilizados en la industria, el OEE se ha consolidado como una 
referencia estándar para medir la eficiencia global de los procesos productivos. Su utilidad está 
en combinar en un único valor porcentual tres dimensiones: disponibilidad, rendimiento y 
calidad. Esta visión integrada permite no solo cuantificar el nivel de eficiencia alcanzado, sino 
también identificar las principales causas de pérdida asociadas a paradas, reducción de 
velocidad o defectos en la producción (Tumbajoy et al., 2022). 

Los sistemas MES gestionan información de ejecución (órdenes, actividades, tiempos y 
resultados) y conectan la planificación empresarial (ERP) con el nivel operativo de planta, 
facilitando la trazabilidad y el control en tiempo real (Lindegren et al., 2022). 

Sin embargo, disponer de un sistema MES no garantiza que los datos se conviertan en 
indicadores de rendimiento útiles. En muchos casos, los sistemas MES carecen de mecanismos 
específicos para integrar información de planificación y registros de ejecución, calcular de 
forma automática indicadores como el OEE y almacenar los resultados históricos que permitan 
realizar análisis temporales y comparativos.

## Soluciones existentes para el cálculo del OEE

Aunque el OEE es un indicador ampliamente utilizado, su aplicación práctica presenta 
limitaciones en muchos entornos industriales. Actualmente se utilizan diferentes 
aproximaciones para abordar el cálculo de este indicador: 

- Hojas de cálculo y herramientas manuales: Son una solución básica y frecuente, 
especialmente en pequeñas y medianas empresas. Si bien ofrecen flexibilidad, presentan 
limitaciones importantes en términos de automatización, trazabilidad y escalabilidad. La 
dependencia de la introducción manual de datos incrementa la probabilidad de errores y 
dificulta la obtención de resultados en tiempo real.

- Módulos especializados en sistemas MES comerciales: Algunos sistemas MES 
del mercado incorporan funcionalidades específicas para el cálculo del OEE. Sin embargo, estas 
soluciones suelen ser costosas y requieren adaptaciones significativas para ajustarse a los 
procesos particulares de cada organización. Además, la integración con arquitecturas de datos 
específicas resulta frecuentemente compleja.

- Aplicaciones verticales independientes: Algunas soluciones están orientadas 
exclusivamente al análisis del OEE. Aunque estas herramientas ofrecen capacidades avanzadas 
de visualización y análisis, su principal inconveniente reside en la necesidad de integración con 
otros sistemas empresariales, lo que puede generar problemas de sincronización de datos y 
redundancia de información.

- Desarrollos a medida: Algunas organizaciones optan por desarrollar soluciones 
propias integradas en sus sistemas existentes. Esta aproximación permite una mayor 
personalización y control, aunque requiere una inversión significativa en recursos técnicos y 
conocimiento especializado.

## Siemens Opcenter Foundation OEE

Siemens Opcenter Foundation OEE: forma parte del ecosistema Opcenter y está 
orientado a la gestión de la producción. Permite realizar el cálculo del OEE, monitorizar el 
rendimiento de las máquinas y registrar tiempos de inactividad. Su integración con otras 
herramientas de Siemens permite una visión global de los procesos de producción.

## Machine Metrics

Machine Metrics: plataforma que se centra en la monitorización de máquinas y en la 
recopilación de datos en tiempo real. Proporciona análisis del OEE, tiempo de inactividad y 
rendimiento, y permite la integración con otros sistemas ERP y MES. MachineMetrics es 
conocida por su facilidad de uso y su enfoque en la conectividad con la maquinaria. 

## Factbird

Es una herramienta enfocada en el análisis del OEE y visualización de datos históricos. 
Se integra con máquinas, ERP y otros sistemas, y permite la visualización de datos en tiempo 
real. Su valor radica en la analítica avanzada que proporciona, junto con sus capacidades de 
integración mediante APIs.

## Limitaciones identificadas en la literatura

A partir de la revisión de la literatura y el análisis de soluciones existentes, se han 
identificado las siguientes limitaciones en la aplicación práctica del indicador OEE en entornos 
industriales: 

En primer lugar, el OEE se calcula a menudo con herramientas no integradas, lo que 
dificulta obtener resultados fiables y comparables. Además, la dispersión de los datos necesarios 
para el cálculo agrava este problema. Los valores definidos en la planificación y los datos reales 
obtenidos durante la ejecución suelen almacenarse en sistemas distintos o en registros no 
estructurados (Bianchini et al., 2024). Esto dificulta integrar la información y obtener una visión 
homogénea del rendimiento productivo. 

En segundo lugar, la ausencia de automatización en el cálculo del OEE genera 
dependencia de procesos manuales que incrementan la probabilidad de error y reducen la 
trazabilidad de los datos. Esta limitación se ve acentuada en entornos con múltiples líneas 
productivas o productos, donde el volumen de datos a procesar es considerable. 
Finalmente, la falta de herramientas de análisis histórico limita las posibilidades de 
detectar tendencias, realizar comparativas temporales y establecer estrategias de 
perfeccionamiento basadas en la evolución del rendimiento productivo. Muchas soluciones 
actuales proporcionan valores puntuales del OEE, pero no incorporan mecanismos sólidos de 
análisis temporal. 

En cuanto a las soluciones analizadas mencionadas anteriormente, aunque son eficaces 
para el cálculo del OEE, presentan ciertas limitaciones que pueden restringir su implementación 
en entornos específicos. Muchas de estas soluciones no ofrecen una integración nativa con 
sistemas ya existentes en las empresas, lo que puede generar gastos adicionales de adaptación e 
integración. Además, el cálculo del OEE en estas plataformas no siempre llega a nivel de 
actividad dentro de las órdenes de trabajo, lo que limita su capacidad para proporcionar 
información detallada sobre el rendimiento de las actividades individuales. 

## Diferenciación del trabajo propuesto

La propuesta desarrollada se diferencia de las soluciones revisadas en los siguientes 
aspectos: 

- Integración nativa en Visual Tracking: A diferencia de las aplicaciones verticales 
independientes, la solución se desarrolla como parte integral del módulo VT de EMI 
Suite 4.0, evitando problemas de sincronización y redundancia de datos característicos 
de sistemas externos.

- Arquitectura orientada a digitalización y trazabilidad: El diseño prioriza la coherencia 
de datos y la posibilidad de análisis posterior, superando las limitaciones de soluciones 
manuales.

- Cálculo automático y sistemático: Se implementan mecanismos de cálculo precalculado 
en backend que garantizan la coherencia y disponibilidad inmediata de los indicadores, 
abordando la problemática de automatización identificada en la literatura.

- Capacidades de análisis temporal: La solución incorpora almacenamiento histórico y 
herramientas de visualización que facilitan el análisis evolutivo y la detección de 
desviaciones, cubriendo la falta de análisis histórico presente en muchas soluciones 
comerciales.

Esta aproximación permite utilizar el conocimiento existente sobre el indicador OEE y 
los sistemas MES para orientar el desarrollo de una aplicación informática específicamente 
adaptada a las necesidades del entorno productivo de EMI Suite 4.0, proporcionando una 
solución que integra las mejores prácticas identificadas en el estado del arte. 

A continuación, se presenta una matriz comparativa que resume las principales 
características y diferencias entre las soluciones existentes en el mercado y la propuesta 
desarrollada.

|Funcionalidad | Sistema integrado en VT - EMI Suite 4.0 | Siemens Opcenter OEE | MachineMetrics | Factbird |
|:---:|:---:|:---:|:---:|:---:|
|Integración nativa dentro del sistema de la empresa | ✅ | ❌ | ❌ | ❌ |
|Cálculo automático del OEE | ✅ | ✅ | ✅ | ✅ |
|Cruce entre planificación y ejecución real | ✅ | ✅ | ✅ | ✅ |
|Cálculo a nivel de actividad en la orden de trabajo | ✅ | ❌ | ❌ | ❌ |
|Histórico y análisis de tendencias | ✅ | ✅ | ✅ | ✅ |
|Visualización gráfica integrada | ✅ | ✅ | ✅ | ✅ |
|Necesidad de integración/adaptación externa al sistema local | ❌ | ✅ | ✅ | ✅ |







