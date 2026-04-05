# Solución propuesta

La solución propuesta para mi Trabajo de Fin de Grado consiste en el diseño y 
desarrollo de un sistema software integrado en el módulo VT de EMI Suite 4.0, orientado al 
cálculo, análisis y visualización del indicador OEE aplicado a órdenes de trabajo industriales y a 
las actividades que las componen.

## Fundamentos de la solución

El sistema se fundamenta en la integración de la información de planificación, definida 
previamente en el entorno de la oficina técnica, con registros reales de ejecución, capturados 
durante la producción en planta. Con esta información, se calcularán de manera automática y 
homogénea los valores de Disponibilidad, Rendimiento y Calidad, así como el valor global del 
OEE asociado a cada actividad y puesto de trabajo. 

La solución se orienta a resolver los problemas identificados previamente, 
principalmente la dispersión de la información y la dependencia de cálculos manuales. Para ello 
se propone una arquitectura que centraliza los datos relevantes dentro del módulo VT, 
garantizando la coherencia entre la planificación y la ejecución real del proceso productivo.

## Arquitectura técnica

El cálculo del OEE y de sus componentes se realizará de forma precalculada en el 
backend, de acuerdo con el funcionamiento actual de VT, con el objetivo de asegurar la 
consistencia de los resultados y optimizar el rendimiento del sistema en las consultas posteriores. Para ello se emplearán métricas como tiempos de ciclo, tiempos de parada y 
microparada, producción real, producción teórica y rechazos registrados durante la ejecución de 
las actividades. 

La arquitectura propuesta contempla los siguientes componentes principales: 
- Capa de integración de datos: Responsable de relacionar los datos teóricos de 
planificación con los datos reales de ejecución, estableciendo las correspondencias necesarias 
entre las entidades del sistema y garantizando la coherencia de la información.

- Motor de cálculo: Aquí se implementan los algoritmos necesarios para calcular 
los componentes del OEE (disponibilidad, rendimiento y calidad) y el valor global del 
indicador, a partir de los datos integrados en la capa anterior.

- Almacenamiento histórico: Gestiona el registro persistente de los valores 
calculados del OEE, permitiendo su recuperación posterior para análisis temporales y 
comparativas.

- Capa de presentación: Proporciona interfaces gráficas de visualización y 
análisis que facilitan la interpretación de los resultados por parte de los usuarios del sistema.

## Funcionalidades principales

Además del cálculo automatizado del OEE, la solución incluirá las siguientes 
funcionalidades: 

- Análisis temporal: Mecanismos de almacenamiento histórico de los valores 
calculados que permitirán realizar análisis de tendencias, comparativas entre períodos y 
detección de desviaciones en el rendimiento productivo.

- Comparación teórico-real: Capacidad de contrastar los valores teóricos 
definidos en la planificación con los resultados reales obtenidos durante la ejecución, 
identificando desviaciones y oportunidades de mejora.

- Visualización gráfica: Herramientas integradas en el frontend de VT para 
facilitar la interpretación visual de los resultados mediante gráficos.

- Trazabilidad completa: Registro detallado de todos los datos utilizados en el 
cálculo del OEE, permitiendo auditar y verificar los resultados obtenidos. 


