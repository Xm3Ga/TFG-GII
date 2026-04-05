# Priorizar Casos Uso Por Actor



| Caso de uso | Actor | Prioridad |
|------------|--------|-----------|
| Gestionar documentación | Project Manager | Alta |
| Gestionar casos de uso | Project Manager | Alta |
| Gestionar requisitos funcionales | Project Manager | Alta |
| Analizar documentación | Ingeniero QA | Alta |
| Estructurar casos de uso y requisitos funcionales | Ingeniero QA | Alta |
| Generar escenarios de prueba | Ingeniero QA | Alta |
| Revisar escenarios generados | Ingeniero QA | Alta |
| Publicar escenarios en API Kiwi TCMS | Ingeniero QA | Alta |
| Recibir escenarios de prueba | Kiwi TCMS | Alta |
| Almacenar escenarios de prueba | Kiwi TCMS | Alta |


### Gestionar documentación
Este caso de uso es crítico porque la documentación constituye el punto de entrada del sistema. Toda la funcionalidad posterior depende de que exista una documentación válida sobre la que trabajar, ya que de ella se extraen los casos de uso y los requisitos funcionales. Sin esta gestión, el sistema no podría iniciar su flujo principal.

### Gestionar casos de uso
Los casos de uso describen cómo interactúa el sistema y sirven como base para la generación de escenarios de prueba. Su correcta gestión es fundamental para asegurar que los escenarios generados reflejan el comportamiento esperado del sistema, por lo que este caso de uso es esencial. 

Reflejados en la documentación.

### Gestionar requisitos funcionales
Los requisitos funcionales definen las funcionalidades que el sistema debe cumplir. Son necesarios para garantizar que los escenarios de prueba cubren correctamente las necesidades del sistema, por lo que su gestión es imprescindible para la validez del proceso.

Reflejados en la documentación.
### Analizar documentación
Este caso de uso representa el primer paso del procesamiento automatizado mediante agentes de IA. A partir del análisis se obtiene la información necesaria (Casos de Uso y Requisitos Funcionales) para el resto del flujo, por lo que sin esta fase no sería posible continuar con la extracción ni la generación de escenarios.

### Estructurar casos de uso y requisitos funcionales
Una vez analizada la documentación, es necesario organizar la información extraída, esto lo hará un agente de Inteligencia Artificial. Este proceso permite convertir datos no estructurados en elementos utilizables por el sistema, siendo clave para garantizar la coherencia y calidad de los escenarios generados.

### Generar escenarios de prueba
Este caso de uso constituye el objetivo principal del sistema, también lo hará un agente de Inteligencia Artificial. A partir de los casos de uso y requisitos funcionales, se generan escenarios de prueba que permiten validar el comportamiento del sistema, por lo que es una funcionalidad central e indispensable.

### Revisar escenarios generados
La revisión permite validar la calidad y corrección de los escenarios antes de su uso o publicación. Aunque el sistema automatiza la generación, la intervención del Ingeniero QA garantiza que los resultados sean adecuados, lo que hace este caso de uso esencial en entornos reales (es un checkpoint).

### Publicar escenarios en API Kiwi TCMS
Esto permite integrar el sistema con una herramienta externa de gestión de pruebas, herramienta utilizada por un agente de Inteligencia Artificial.

### Recibir escenarios de prueba
Es clave que Kiwi TCMS reciba los escenarios, eso quiere decir que la publicacion y la herramienta utilizad por el agente funciona correctamente.

### Almacenar escenarios de prueba
Una vez recibidos, los escenarios deben almacenarse en Kiwi TCMS. Este caso de uso garantiza la persistencia de la información y permite que los escenarios sean utilizados en procesos de testing, siendo esencial para cerrar el flujo completo.