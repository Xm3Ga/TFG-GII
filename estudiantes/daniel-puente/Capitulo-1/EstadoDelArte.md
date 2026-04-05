# Estado del arte
-------------------------------------
## Sistemas POS para la hosteleria

Los Point of Sale conocidos como TPV funcionan la mayoria con una arquitectura cliente-servidor, donde los dispositivos de los camareros dependen de un servidor central para cualquier operación, de esta forma se centraliza la gestión del restaurante.

Las soluciones mas conocidas en el mercado son:

  **Agora POS** - Funciona tanto en la nube como en servidor local, requiere intervención del proveedor para todos los requisitos, lo que supondra un elevado coste. Además del alto coste de la licencia y el hardware. Tiene modulos de mesas, comandas y caja.

  **Revo XEF** -  Orientada exclusivamente para iPad y basada en la nube. Es muy intuitiva y tiene buena interfaz pero esta atada al enterno Apple y depende completamente de internet para funcionar.

  **Last.app** - Integra sala, reservas y stock pero si se cae Internet, el sistema deja de funcionar por completo, ya que, es una plataforma cloud con API REST. 

  **Square** - Tiene el modulo de cocina integrado y cierto soporte offline, pero no permite configurar varias zonas de servicio con mesas numeradas independientemente.

  **Oracle Simphony** - Orientada a grandes cadenas de negocios, pero su coste es realmente alto para una pequeña empresa, a pesar de que, permite configuraciones complejas y es una plataforma muy completa

  **Glop/Camarero10** - Orientada a pequeños negocios, funciona en un entorno local. Tienen un precio asequible pero su personalización es limitada y no se contemplan los menús variables.

  Todas estas soluciones están pensadas para un restaurante estándar, en cambio, adaptarlas a un escenario específico como el del restaurante La Unión,  puede requerir un coste adicional y la intervención del proveedor


### ¿Por qué las soluciones existentes no sirven?
**No tienen en cuenta el menú variable**
En La Unión el menú puede cambiar: a veces son dos primeros, otras solo un plato, o primero y segundo. Los sistemas POS analizados no permiten configurar esto fácilmente; habría que modificar el código o pedir cambios al proveedor.


**No gestionan bien las zonas del restaurante**
La mayoría de sistemas usan una numeración única para todas las mesas. Sin embargo, el restaurante necesita que cada zona tenga su propia numeración de mesas, por lo que es necesario tratar las zonas como entidades independientes en la base de datos.


**No avisan cuando una mesa debe liberarse para la siguiente reserva**
Ningún sistema calcula cuánto tiempo queda para la próxima reserva de una mesa ni avisa al camarero con antelación. Para hacer esto se necesita un proceso automático en el servidor que los sistemas analizados no incluyen.


**Los alérgenos no son obligatorios**
En los sistemas revisados, los alérgenos se introducen como un campo opcional y libre. En este proyecto, en cambio, se consideran obligatorios, se validan en el servidor y quedan guardados en el historial de la comanda.


**No registran un historial detallado de los tickets**
No es habitual que los sistemas registren qué camarero envió o reclamó un ticket, en qué mesa y en qué momento. Solo soluciones empresariales como Oracle Simphony ofrecen algo así, pero su coste es demasiado alto para un restaurante de este tamaño.


## Tabla comparativa

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
2. El pedido aparece en la pantalla de cocina como *pendiente*.
3. El cocinero lo pone *en preparación*.
4. Cuando está listo, lo marca como *listo* y el camarero recibe un aviso automático.

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

