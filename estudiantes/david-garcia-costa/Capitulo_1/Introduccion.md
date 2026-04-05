# Introducción

## Motivación

En este Trabajo de Fin de Grado se propone realizar un diseño e implementar un sistema de agentes de Inteligencia Artificial orientado a mejorar el proceso de testing de QA de CIC Consulting Informático. 

La motivación que lleva a realizar este trabajo sale de una necesidad por parte de CIC.

 Actualmente, el esfuerzo de los ingenieros de QA esta en hacer tareas de elevada carga; como puede ser la interpretación de documentación (Documentos de Requisitos Funcionales y Documentos de Diseño del Sistema), el análisis de los requisitos funcionales y casos de uso que contienen dichos documentos, la redacción manual de planes de prueba dentro de un excel, en KiwiTCMS ahora mismo solo hay unos cuantos ejemplos de los scenarios en lenguaje Gherkin y estan dentro de una carpeta de VSCode donde trabaja el ingeniero, no estan en ninguna API. Estas actividades requieren considerable inversión de tiempo y conocimiento especializado, lo que limita la escalabilidad del proceso y genera dependencia del expertise individual del equipo.

## Problemática Identificada

En términos más especificos, se puede decir que el flujo de trabajo actual en CIC presenta las siguientes limitaciones:

- **Dependencia del análisis manual**
- **Falta de automatización en fase preparatoria**
- **Escalabilidad limitada**

## Propuesta de Solución

Para resolver este problema, el presente trabajo propone el diseño de una arquitectura basada en agentes de Inteligencia Artificial, utilizando Google ADK con un modelo de OpenAI, que permita automatizar la generación de casos de prueba a partir de la documentación existente. 

La solución se estructura en torno a un sistema multiagente especializado que se detallarán durante la solución, en el cual cada agente asume un rol específico dentro del flujo de trabajo.

Esta arquitectura aprovecha las capacidades de los agentes de IA no solo para ejecutar tareas repetitivas, sino también para interpretar y analizar la documentación por parte de los Project Managers y Clientes, estructurar la información y tomar decisiones en un proceso colaborativo orientado a un objetivo común.