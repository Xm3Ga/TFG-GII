# Título
DIGITALIZACIÓN DE LA GESTIÓN INTERNA DE UN RESTAURANTE MEDIANTE UNA PWA CON SOPORTE OFFLINE.

Trabajo fin de grado realizado por
**Daniel Puente Sarañana**


SANTANDER – 03/2026

--------
## Resumen
El presente (TFG) Trabajo de Fin de Grado aborda el desarrollo de una Progressive Web App (PWA) para la gestión interna de un restaurante familiar ubicado en Puente Viesgo, Cantabria. Actualmente operan empleando métodos analógicos como: comandas a papel, reservas en un libro físico y una comunicación con cocina a través de un telefonillo. Esto genera errores en comandas, duplicidad de reservas y pérdidas de tiempo.


La solución propuesta  es una PWA con arquitectura local-first que garantiza la continuidad del servicio ante fallos de red, sincronizando los datos en cuanto vuelve la conexión al servidor. Además digitalizará los módulos de comandas, cocina, reservas, caja y auditoria, controlado por un acceso por  roles (JWT) y notificaciones en tiempo real (WebSocket).

El proyecto sigue una metodología basada en RUP y simplificada con prácticas ágiles, comenzando por entrevistas con el cliente y terminando con un MVP que cubra los casos de uso esenciales del negocio sobre la infraestructura ya existente.

Palabras clave: **PWA**;**Arquitectura local-first**;**Digitalización hostelería**;**Gestión restaurantes**;**JWT**;**WebSocket**.

------

## Escenario e introducción

El restaurante La Unión es un establecimiento familiar ubicado en Puente Viesgo, Cantabria. 
Este establecimiento ofrece servicio de comidas con menú del dia y carta, mientras en el servicio de cenas solo ofrece carta. En él trabajan entre doce y catorce personas incluyendo cocineros, camareros y personal directivo. Debido al limitado espacio del local encontramos diversas zonas (comedor, bar, terraza, lateral, plaza, mirador, balcón y escaleras). Un gran volumen de servicio y una alta demanda de reservas, exige una alta coordinación entre salas y cocina.

Actualmente, la gestión del restaurante se basa en métodos analógicos. Las comandas se apuntan en libretas de camarero, las reservas se registran en un libro físico y la comunicación entre sala y cocina se realiza mediante un telefonillo anunciando verbalmente "sube nota" o "segundos de la mesa 4 del bar". Estas herramientas fueron funcionales durante décadas, pero hoy día presentan limitaciones que pueden afectar a la experiencia del cliente, la eficiencia del equipo y a la calidad del servicio.

------

# Justificacion de la propuesta
----------------------------------------------------
La experiencia directa del autor en el sector hostelero permitió identificar de primera mano los problemas derivados a dicho modelo de gestión. Entre los más frecuentes podemos encontrar la duplicidad o pérdida de reservas en papel, la dificultad para coordinar el doblado de mesas cuando existe a continuación otra reserva, los errores o dificultad de lectura en las comandas escritas a mano y la ineficiencia del telefonillo como canal de comunicación con cocina, ya que, el camarero interrumpe su servicio para transmitir información que podría comunicarse digitalmente.

Frente a las soluciones existentes, resultarían costosas y limitadas debido a la dificultad de adaptación al negocio, por ello se plantea una aplicación a medida y precisa respetando los requisitos establecidos. Esta decision se basa en tren claves principales: adaptar la lógica de la aplicación a la operativa real del negocio(zonas, mesas, menú variable, carta, turnos de comida y cena), la importancia de garantizar un funcionamiento del sistema comtemplando fallos de conectividad, y la oportunidad de integrar funcionalidades como el log sobre tickets y el registro de alérgenos, donde otras soluciones no ofrecen tal control

----------------------------------------------------
# Propuesta de solución
La solución propuesta consiste en desarrollar una PWA orientada a la gestión integral del Restaurante La Union. La aplicación contempla cuatro tipos de perfil diferenciados (administrador (para jefe y encargado), cocinero, camarero y un sistema de notificaciones automáticas. 


Dentro de cada usuario, los camareros podrán gestionar mesas por zonas, enviar tickets a caja, tomar comandas digitales, registrar observaciones, registrar alérgenos y solicitar los segundos platos a cocina.Los cocineros dispondran de una pantalla KDS que muestra en tiempo real tanto las comandas pendientes como las comandas en preparación, destacando las observaciones y alérgenos. Además, podran marcar los platos como listo y el camarero correspondiente recibirá una notificación automática. Los administradores tendrán acceso a la gestión de reservas, configuracion del sistema y resgistros de auditorías. En cambio, el sistema automático de notificaciones solamente avisará de los platos ya listos y de la proximidad de una reserva sobre una mesa ocupada (doblaje de mesas debido al limitado espacio).


Desde un enfoque técnico, la arquitectura seguirá un enfoque local-first, cada dispositivo guardara localmente las comandas al momento de crearse y las sincronizará con el servidor a través de la red WiFi del establecimiento, de esta forma se garantiza la continuidad del servicio ante posibles interrupciones de conectividad.


----------------------------------------------------

## Estado del arte
-------------------------------------
### Sistemas POS para la hosteleria

Los Point of Sale conocidos como TPV funcionan la mayoria con una arquitectura cliente-servidor, donde los dispositivos de los camareros dependen de un servidor central para cualquier operación, de esta forma se centraliza la gestión del restaurante.

Las soluciones mas conocidas en el mercado son:

  **Agora POS** - Funciona tanto en la nube como en servidor local, requiere intervención del proveedor para todos los requisitos, lo que supondra un elevado coste. Además del alto coste de la licencia y el hardware. Tiene modulos de mesas, comandas y caja.

  **Revo XEF** -  Orientada exclusivamente para iPad y basada en la nube. Es muy intuitiva y tiene buena interfaz pero esta atada al enterno Apple y depende completamente de internet para funcionar.

  **Last.app** - Integra sala, reservas y stock pero si se cae Internet, el sistema deja de funcionar por completo, ya que, es una plataforma cloud con API REST. 

  **Square** - Tiene el modulo de cocina integrado y cierto soporte offline, pero no permite configurar varias zonas de servicio con mesas numeradas independientemente.

  **Oracle Simphony** - Orientada a grandes cadenas de negocios, pero su coste es realmente alto para una pequeña empresa, a pesar de que, permite configuraciones complejas y es una plataforma muy completa

  **Glop/Camarero10** - Orientada a pequeños negocios, funciona en un entorno local. Tienen un precio asequible pero su personalización es limitada y no se contemplan los menús variables.

  Todas estas soluciones están pensadas para un restaurante estándar, en cambio, adaptarlas a un escenario específico como el del restaurante La Unión,  puede requerir un coste adicional y la intervención del proveedor

### Tabla comparativa

| Criterio | Agora POS | Revo XEF | Last.app | Oracle Simphony | Glop / Camarero10 | **La Unión App** |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| Funciona sin internet (local-first) | ≈ | ❌ | ❌ | ≈ | ✅ | ✅ PWA + IndexedDB |
| Notificaciones en tiempo real | ❌ | ≈ | ✅ | ✅ | ❌ | ✅ socket.io |
| Varias zonas con numeración propia | ≈ | ≈ | ≈ | ✅ | ≈ | ✅ entidad Zona |
| Lógica variable del menú del día | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ una a medida |
| Aviso de doblado por reserva próxima | ❌ | ❌ | ≈ | ≈ | ❌ | ✅ WebSocket |
| Alérgenos obligatorios e inmutables | ≈ | ≈ | ≈ | ≈ | ❌ | ✅ validación backend |
| Solicitud de segundos platos | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ flujo específico |
| Log auditado de tickets | ≈ | ≈ | ≈ | ✅ | ❌ | ✅ usuario + timestamp |
| Control de acceso por rol | ≈ | ≈ | ✅ | ✅ | ≈ | ✅ JWT + middleware |
| Requiere hardware específico | ✅ sí | ✅ iPad | ❌ | ✅ sí | ≈ | ❌ cualquier tablet |
| Coste de implantación | ≈ alto | ≈ alto | ≈ medio | ≈ muy alto | ≈ bajo | ✅ sin licencia |
| Se adapta a la operativa del negocio | ≈ | ≈ | ≈ | ≈ | ≈ | ✅ desarrollo a medida |

> **Leyenda:** ✅ Sí / ≈ Parcial · ❌ No

---

## Fundamentos tecnologicos

### Progressive Web Apps (PWA)

Una PWA es una aplicación web que puede comportarse como una app nativa instalada en el móvil o tablet. Para que funcione como tal necesita tres cosas:

- **Web App Manifest**: un archivo que define el nombre, icono y colores de la app y permite instalarla en la pantalla de inicio del dispositivo sin necesidad de una tienda de apps.
- **Service Worker**: un script que se ejecuta en segundo plano y actúa como intermediario entre la app y la red. Permite guardar recursos en caché y encolar peticiones cuando no hay conexión para enviarlas cuando vuelva.
- **HTTPS**: necesario para que el Service Worker funcione, garantiza que la comunicación entre cliente y servidor es segura.


### IndexedDB

Es una base de datos integrada en el navegador que permite guardar datos complejos de forma local. A diferencia del localStorage, que solo guarda texto simple y tiene poca capacidad, IndexedDB puede almacenar objetos JavaScript completos y hacer consultas. En este proyecto se usa para guardar las comandas pendientes de sincronización y el estado de las mesas cuando no hay conexión.

###  Arquitectura local-first

En este proyecto cuando un camarero registra una comanda, esta se guarda inmediatamente en su dispositivo. Si en ese momento hay conexión WiFi, se sincroniza al instante con el servidor y aparece en cocina. Si no la hay, se queda guardada localmente y se envía en cuanto vuelve la conexión, sin que el camarero tenga que hacer nada.Es lo contrario al modelo habitual, donde el cliente necesita el servidor para cualquier acción.


### Background Sync API

Permite al Service Worker detectar cuando el dispositivo recupera la conexión y ejecuta automáticamente las tareas que estaban pendientes aunque el usuario haya cerrado el navegador. Así, las comandas guardadas offline se envían solas al servidor en cuanto vuelve la conexion.

### WebSocket

Para las notificaciones en tiempo real, como avisar al camarero  que sus platos están listos o que una mesa tiene que salir pronto, no basta con un HTTP normal, ya que el servidor no puede enviar mensajes al cliente sin que este los pida primero.

WebSocket soluciona esto creando una conexión permanente entre cliente y servidor. Una vez establecida el servidor puede enviar mensajes al cliente en cualquier momento. En este proyecto se usa para emitir eventos como `PLATO_LISTO` o `AVISO_SALIDA_MESA` al camarero en tiempo real.

###  Autenticación con JWT y control de acceso por rol

El sistema tiene tres tipos de usuario con permisos distintos: Camarero, Cocinero y Administrador.

Un JWT es un token firmado que contiene la identidad y el rol del usuario. El servidor lo genera al hacer login y el cliente lo incluye en cada petición a la API. Así el servidor sabe quién es el usuario y que puede realizar sin tener que consultar la base de datos.

Se usan dos tipos de token:

- **Access token**: de corta duración (15-60 minutos), se incluye en cada petición. Cuando caduca, el cliente pide uno nuevo.
- **Refresh token**: de larga duración, guardado en una cookie segura. Permite que el camarero no tenga que volver a hacer login durante todo el turno.

El control de acceso se implementa en el servidor con un middleware de Express que comprueba el token y el rol antes de ejecutar cada operación. Por ejemplo, el endpoint de cobrar un ticket solo es accesible para el rol Administrador.

---------------------------------------

### Kitchen Display Systems (KDS)

Un KDS  es una pantalla digital en cocina que sustituye a los tickets de papel. Cuando el camarero registra una comanda, el sistema la envía automáticamente a la pantalla de cocina, donde el cocinero puede verla y cambiar su estado.

El flujo funcionaría así:

1. El camarero registra la comanda desde su dispositivo.
2. El pedido aparece en la pantalla de cocina como **pendiente**.
3. El cocinero lo pone **en preparación**.
4. Cuando está listo, lo marca como **listo** y el camarero recibe un aviso automático.

Los sistemas KDS más avanzados, como Oracle KDS o Fresh KDS, permiten redirigir pedidos a distintas zonas de la cocina (frío, caliente, postres), usar colores para indicar el tiempo de espera y generar métricas de rendimiento.

En cuanto al hardware, las pantallas para cocina profesional necesitan protección contra grasa y vapor (con certificación IP-54 o mayor) y montaje fijo en pared o encimera.

En este proyecto, la funcionalidad KDS se implementa como una vista de la propia aplicación web, accesible desde cualquier dispositivo con navegador, lo que elimina la necesidad de comprar hardware específico.

--------------------------

###  Gestión digital de reservas

Los sistemas de reservas digitales resuelven los problemas del libro en papel: eliminan las reservas duplicadas, centralizan la información y permiten ver el estado de cada mesa en vivo.

Plataformas como TheFork Manager, Cover Manager o ZenChef ofrecen estas funcionalidades junto con recordatorios automáticos al cliente y análisis de ocupación. Sin embargo, ninguna contempla la lógica específica que necesita La Unión: calcular cuánto tiempo queda hasta la próxima reserva de una mesa concreta y avisar al camarero con antelación.

Esto requiere un proceso que se ejecute repetidamente en el servidor, consulte el estado de ocupación de cada mesa y su próxima reserva, y envíe un aviso al camarero cuando queden menos de 5 minutos. Ninguna solución comercial analizada implementa este suceso de forma nativa.

### Gestión de alérgenos 

El Reglamento (UE) Nº 1169/2011 obliga a informar sobre los 14 alérgenos de manera obligatoria en todos los alimentos servidos en restaurantes. No cumplirlo tiene consecuencias legales para el establecimiento y puede poner en riesgo la salud de los clientes.

En el sistema analógico actual, los alérgenos se comunican anotados a mano en la comanda, lo que genera riesgo de pérdida o ilegibilidad. En los sistemas digitales, este campo debería ser obligatorio y no modificable una vez que la comanda llegue a cocina, garantizando que siempre quede registrado correctamente.

En este proyecto, los alérgenos son un campo obligatorio validado en el servidor antes de guardar la comanda, quedan registrados en el historial y se muestran con un indicador visual destacado en la pantalla de cocina para que el cocinero no pueda pasarlos por alto.

-------
##  Alcance y limitaciones

###  ¿Qué cubre el sistema?

El sistema cubre toda la operativa diaria del Restaurante La Unión identificada en la primera entrevista con el cliente:

- Gestión de mesas por zona en tiempo real
- Comandas digitales con lógica variable del menú del día
- Registro obligatorio e inmutable de alérgenos y observaciones
- Pantalla KDS para cocina con estados (pendiente / en preparación / listo)
- Solicitud de segundos platos desde sala hacia cocina
- Notificaciones en tiempo real al camarero cuando los platos están listos
- Gestión de reservas con asignación de mesa y número de comensales
- Aviso automático al camarero cuando una mesa debe quedar libre (5 min antes)
- Envío y reclamación de tickets con log de auditoría completo
- Cobro en caja restringido al rol Administrador
- Control de acceso diferenciado por rol
- Funcionamiento ante fallos puntuales de la red WiFi local

----------

###  ¿Qué no cubre el sistema?

Las siguientes funcionalidades quedan fuera del alcance de este proyecto:

**Portal de cliente y carta digital pública**: el sistema es exclusivamente una herramienta de gestión interna. Los clientes no pueden hacer reservas online, consultar la carta ni acceder a la aplicación en ningún momento.

**Integración con plataformas de delivery**: el restaurante no tiene servicio a domicilio. 

**Gestión de inventario y stock**: el control de materias primas y proveedores que no fue identificado como necesidad por el cliente.

**Contabilidad**: requiere conocimiento específico y esta fuera del ámbito del autor.

**App nativa iOS/Android**: la PWA permite instalar la app en cualquier dispositivo con experiencia equivalente a una app nativa, sin los costes de publicar en el App Store y Google Play.


**Gestión de personal y turnos**: es un dominio fuera del alcance definido con el cliente.

--------

### ¿Por qué esta solución y no una comercial?

En el mercado existen soluciones mas completas y robustas para la gestión de restaurantes, pero ninguna se adapta perfectamente a las necesidades específicas de La Unión sin requerir modificaciones costosas o compromisos en la funcionalidad.


**Funcionalidades no disponibles sin intervención del proveedor.** Los sistemas POS analizados están diseñados para un restaurante estándar. 
- El menú del día en La Unión es variable: a veces son dos primeros, otras un único plato, o primero y segundo. Ningún POS permite configurar esto sin modificar el código o pagar al proveedor. 
- Lo mismo ocurre con las zonas del restaurante: la mayoría usa una numeración única para todas las mesas, mientras que aquí cada zona necesita su propia numeración independiente.
- A esto se suma que ningún sistema calcula cuánto tiempo queda para la próxima reserva de una mesa ni avisa al camarero con antelación, algo que requiere un proceso automático en el servidor que los sistemas analizados no incluyen. 
- Los alérgenos tampoco se tratan con el rigor necesario: en los POS revisados son un campo opcional y libre, cuando deberían ser obligatorios, validados en el servidor e inmutables una vez que la comanda llega a cocina.
- Por último, el registro detallado de tickets —qué camarero lo envió, desde qué mesa y en qué momento— solo está disponible en soluciones empresariales como Oracle Simphony, cuyo coste es inasumible para un negocio de este tamaño.

**Coste inasumible para un restaurante familiar.** Las plataformas más completas requieren licencias, hardware específico o suscripciones mensuales que no son viables para un negocio de estas características. Este proyecto se despliega sobre la red WiFi y los tablets que ya tiene el local, usando tecnologías open source.

**Independencia de proveedor.** Las soluciones comerciales atan al negocio a un proveedor que puede modificar precios, discontinuar el producto o exigir hardware propietario. Una solución a medida elimina esa dependencia y puede evolucionar con las necesidades reales del negocio sin coste adicional.

En resumen, esta solución no pretende competir con las plataformas analizadas en términos de funcionalidades globales, sino resolver exactamente el problema de este restaurante con los recursos disponibles, de forma sostenible y económica a largo plazo.

---

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

## Referencias bibliograficas

Añadir
