# Disciplina de requisitos

Esta disciplina tiene como finalidad establecer, de manera clara y estructurada, quiénes interactúan con el sistema y qué funcionalidades deben estar disponibles para cada uno de ellos. A partir de esto, se definen los casos de uso principales, su prioridad, y se detalla su comportamiento esperado.

-------

## Identificación Actores

// Fotos en otro repo
![Actores](/documentos/CDU/imagenes/Identificacion-Actores/Actores.svg)

### Cocinero

Actor primario que aparte de realizar sus funciones en cocina, interactua con la pantalla KDS. En ella visualiza y gestiona las comandas. Además, ve en tiempo real cualquier modificación realizada sobre una comanda que se encuentre ya en cocina.

### Camarero

Actor primario muy activo que gestiona mesas, toma y edita comandas, solicita segundos platos, gestiona tickets y recibe notificaciones en tiempo real.

### Administrador

Actor primario con completo acceso. Hereda todos los casos de uso del camarero y además gestiona reservas, caja, usuarios, auditoría y la configuración del sistema.

### Sistema de notificaciones

El Sistema de Notificaciones se modela como actor secundario para representar los procesos automáticos del sistema que se disparan sin intervención humana. Aunque técnicamente forma parte del backend, se representa como actor para visibilizar los flujos de notificación en tiempo real en los diagramas de casos de uso.


## Tabla actores y casos de uso 

| ID | Caso de uso | Camarero | Administrador | Cocinero | Sist. Notif. |
|---|---|:---:|:---:|:---:|:---:|
| CU-01 | Iniciar sesión | ✅ | ✅ | ✅ | |
| CU-02 | Cerrar sesión | ✅ | ✅ | ✅ | |
| CU-03 | Ver estado de mesas por zona | ✅ | ✅ | | |
| CU-04 | Abrir mesa | ✅ | ✅ | | |
| CU-05 | Cerrar mesa | | ✅ | | |
| CU-06 | Tomar comanda | ✅ | ✅ | | |
| CU-07 | Ver comanda de una mesa | ✅ | ✅ | | |
| CU-08 | Editar comanda | ✅ | ✅ | | |
| CU-09 | Solicitar segundos platos | ✅ | ✅ | | |
| CU-10 | Ver comandas pendientes en KDS | | | ✅ | |
| CU-11 | Marcar comanda en preparación | | | ✅ | |
| CU-12 | Marcar plato como listo | | | ✅ | |
| CU-13 | Ver alérgenos y observaciones | | | ✅ | |
| CU-14 | Enviar ticket a caja | ✅ | ✅ | | |
| CU-15 | Editar ticket | ✅ | ✅ | | |
| CU-16 | Reclamar ticket enviado | ✅ | ✅ | | |
| CU-17 | Cobrar ticket en caja | | ✅ | | |
| CU-18 | Ver listado de reservas del día | | ✅ | | |
| CU-19 | Crear reserva | | ✅ | | |
| CU-20 | Asignar mesa a reserva | | ✅ | | |
| CU-21 | Editar reserva | | ✅ | | |
| CU-22 | Cancelar reserva | | ✅ | | |
| CU-23 | Ver listado de usuarios | | ✅ | | |
| CU-24 | Crear usuario | | ✅ | | |
| CU-25 | Editar usuario y rol | | ✅ | | |
| CU-26 | Eliminar usuario | | ✅ | | |
| CU-27 | Configurar zonas y mesas | | ✅ | | |
| CU-28 | Añadir mesa a zona | | ✅ | | |
| CU-29 | Eliminar mesa de zona | | ✅ | | |
| CU-30 | Mover mesa dentro del plano | | ✅ | | |
| CU-31 | Unir mesas | | ✅ | | |
| CU-32 | Separar mesas unidas | | ✅ | | |
| CU-33 | Consultar log de auditoría | | ✅ | | |
| CU-34 | Añadir plato a la carta | | ✅ | | |
| CU-35 | Editar plato de la carta | | ✅ | | |
| CU-36 | Eliminar plato de la carta | | ✅ | | |
| CU-37 | Marcar plato como menú del día | | ✅ | | |
| CU-38 | Enviar notificación de plato listo | | | | ✅ |
| CU-39 | Enviar aviso de mesa próxima a liberar | | | | ✅ |
| CU-40 | Actualizar KDS al editar comanda | | | | ✅ |

## Diagrama de casos de uso por actor 
// Fotos en otro repo privado
### Cocinero
![CDU-Cocinero](/documentos/CDU/imagenes/DiagramaCDU-Actor/CDU-Cocinero.svg)
### Camarero
![CDU-Camarero](/documentos/CDU/imagenes/DiagramaCDU-Actor/CDU-Camarero.svg)
### Admin
![CDU-Admin](/documentos/CDU/imagenes/DiagramaCDU-Actor/CDU-Admin.svg)
### Sistema de Notificaciones
![CDU-SistNoti](/documentos/CDU/imagenes/DiagramaCDU-Actor/CDU-SistNoti.svg)

## Priorización MoSCoW

| **#** | **Caso de Uso** | **Prioridad** |
|-------|-----------------|:-------------:|
| CU-01 | **Iniciar sesión** | 🔴 Must |
| CU-02 | **Cerrar sesión** | 🔴 Must |
| CU-03 | **Ver estado de mesas por zona** | 🔴 Must |
| CU-04 | **Abrir mesa** | 🔴 Must |
| CU-05 | **Cerrar mesa** | 🔴 Must |
| CU-06 | **Tomar comanda** | 🔴 Must |
| CU-07 | **Ver comanda de una mesa** | 🔴 Must |
| CU-08 | **Editar comanda** | 🔴 Must |
| CU-10 | **Ver comandas pendientes en KDS** | 🔴 Must |
| CU-11 | **Marcar comanda en preparación** | 🔴 Must |
| CU-12 | **Marcar plato como listo** | 🔴 Must |
| CU-13 | **Ver alérgenos y observaciones** | 🔴 Must |
| CU-14 | **Enviar ticket a caja** | 🔴 Must |
| CU-17 | **Cobrar ticket en caja** | 🔴 Must |
| CU-18 | **Ver listado de reservas del día** | 🔴 Must |
| CU-19 | **Crear reserva** | 🔴 Must |
| CU-20 | **Asignar mesa a reserva** | 🔴 Must |
| CU-24 | **Crear usuario** | 🔴 Must |
| CU-25 | **Editar usuario y rol** | 🔴 Must |
| CU-37 | **Marcar plato como menú del día** | 🔴 Must |
| CU-38 | **Enviar notificación de plato listo** | 🔴 Must |
| CU-09 | **Solicitar segundos platos** | 🟠 Should |
| CU-15 | **Editar ticket** | 🟠 Should |
| CU-16 | **Reclamar ticket enviado** | 🟠 Should |
| CU-21 | **Editar reserva** | 🟠 Should |
| CU-22 | **Cancelar reserva** | 🟠 Should |
| CU-27 | **Configurar zonas y mesas** | 🟠 Should |
| CU-34 | **Añadir plato a la carta** | 🟠 Should |
| CU-35 | **Editar plato de la carta** | 🟠 Should |
| CU-39 | **Enviar aviso de mesa próxima a liberar** | 🟠 Should |
| CU-40 | **Actualizar KDS al editar comanda** | 🟠 Should |
| CU-23 | **Ver listado de usuarios** | 🟢 Could |
| CU-26 | **Eliminar usuario** | 🟢 Could |
| CU-28 | **Añadir mesa a zona** | 🟢 Could |
| CU-29 | **Eliminar mesa de zona** | 🟢 Could |
| CU-30 | **Mover mesa dentro del plano** | 🟢 Could |
| CU-31 | **Unir mesas** | 🟢 Could |
| CU-32 | **Separar mesas unidas** | 🟢 Could |
| CU-33 | **Consultar log de auditoría** | 🟢 Could |
| CU-36 | **Eliminar plato de la carta** | 🟢 Could |


---

###  Descripción de la priorización

#### 🔴 Must have — Alta prioridad
Los 21 casos de uso clasificados como Must cubren el núcleo operativo del sistema. Sin ellos el restaurante no puede funcionar con la aplicación: autenticación, gestión de mesas, comandas, KDS de cocina, tickets, reservas y usuarios son funcionalidades críticas que conforman el MVP del proyecto.

#### 🟠 Should have — Media prioridad
Los 10 casos de uso Should son importantes para la calidad del servicio pero no bloquean la operativa básica. Incluyen la solicitud de segundos platos, la edición y reclamación de tickets, la configuración del plano y las notificaciones avanzadas de WebSocket. Se implementarán tras completar los Must.

#### 🟢 Could have — Baja prioridad
Los 9 casos de uso Could añaden valor al sistema pero no son críticos para el lanzamiento del MVP. Cubren funcionalidades de configuración avanzada del plano de mesas, gestión completa de usuarios y auditoría. Se abordarán si el tiempo de desarrollo lo permite.

## Detalle de casos de uso

### CU-01 INICIO DE SESIÓN

- **Actor principal:** Camarero / Cocinero / Administrador
- **Precondición:** El usuario dispone de sus credenciales válidas y la aplicación está disponible en la red local del restaurante.
- **Flujo principal:**
  - El usuario accede a la pantalla de inicio de la aplicación.
  - Introduce sus credenciales.
  - El sistema valida las credenciales frente a la base de datos.
  - El servidor genera un access token JWT y un refresh token.
  - El sistema envia al usuario a la pantalla principal según su rol (camarero, cocinero o administrador).
- **Flujo alternativo:**
  - Credenciales incorrectas → El sistema muestra un mensaje de fallo y permite reintentar. Tras 5 intentos fallidos se bloquea el acceso temporalmente.
  - Sin conexión a la red local → El sistema informa de que no es posible autenticarse sin conexión. (La autenticación requiere servidor).
- **Postcondición:** El usuario está autenticado y accede a las funcionalidades correspondientes de su rol.
 
---

### CU-06 TOMAR COMANDA

- **Actor principal:** Camarero/Administrador
-  **Precondición:** El usuario esta autenticado y la mesa esta en estado abierta
- **Flujo principal:** 
  - El camarero selecciona una mesa abierta.
  - El sistema muestra la pantalla de comanda con los platos disponibles agrupados por categoría (primeros, segundos, postres, bebidas, café).
  - El camarero indica si el comensal pide de carta o de menú del día.
  - Selecciona los platos con su cantidad.
  - Registra alérgenos y observaciones por plato (campo obligatorio).
  - Confirma la comanda.
  - El sistema guarda la comanda localmente en IndexedDB y la sincroniza con el servidor si hay conexión.
  - La comanda aparece automáticamente en el KDS de cocina.
- **Flujo alternativo:**
  - Sin conexión → la comanda se guarda en IndexedDB y se sincroniza en cuanto vuelve la conexión mediante Background Sync. El camarero no nota diferencia.
  - El camarero no rellena los alérgenos → el sistema bloquea el envío y muestra un aviso de campo obligatorio.
- **Postcondición:** La comanda queda registrada en el sistema y visible en el KDS de cocina.
- **Reglas de Negocio:**
  - Los alérgenos son obligatorios y no se pueden dejar vacíos.
  - Si el comensal pide de menú, el precio aplicado es el precio fijo del menú independientemente de la combinación de platos elegida.
  - Si pide de carta, cada plato tiene su precio individual.

### CU-08 EDITAR COMANDA

- **Actor principal:** Camarero/Administrador
-  **Precondición:** El usuario está autenticado y existe una comanda activa para la mesa.
- **Flujo principal:**
  - El camarero accede a la comanda activa de una mesa (CU-07).
  - Puede añadir nuevos platos, modificar cantidades o eliminar platos.
  - Confirma los cambios.
  - El sistema actualiza la comanda y notifica al KDS en tiempo real (CU-40).
- **Flujo alternativo:**
  - El camarero intenta modificar un plato que ya está en preparación o listo en cocina. Entonces el sistema impide la modificación y muestra un aviso de que ese plato ya no puede editarse.
  - Sin conexión → los cambios se guardan localmente y se sincronizan al recuperar la conexión
- **Postcondición:** La comanda queda actualizada y el KDS refleja los cambios en tiempo real.
- **Reglas de Negocio:**
  - Un plato marcado como "en preparación" o "listo" por el cocinero es inmutable.
  - Los alérgenos de los platos ya enviados a cocina no pueden modificarse.
  - Cualquier edición queda registrada en el log de auditoría con usuario y timestamp.

### CU-09 SOLICITAR SEGUNDOS PLATOS

- **Actor principal:** Camarero/Administrador
-  **Precondición:** El usuario está autenticado, la mesa tiene una comanda activa y los primeros platos han sido marcados como listos por cocina.
- **Flujo principal:**
  - El camarero accede a la comanda activa de la mesa.
  - Pulsa la opción "Solicitar segundos".
  - El sistema envía la solicitud a cocina mediante WebSocket.
  - El KDS muestra la solicitud de segundos platos para esa mesa.
- **Flujo alternativo:**
  - El camarero intenta solicitar segundos antes de que los primeros estén listos → el sistema muestra un aviso y bloquea la acción.
- **Postcondición:** Cocina recibe la solicitud de segundos platos en tiempo real en el KDS.
- **Reglas de negocio:**
  - Solo se puede solicitar segundos si los primeros platos de esa mesa están en estado "listo".
  - La solicitud queda registrada en el log de auditoría.

### CU-12 MARCAR PLATO COMO LISTO

- **Actor principal:** Cocinero
-  **Precondición:** El cocinero está autenticado y existe al menos un plato en estado "en preparación" en el KDS.
- **Flujo principal:**
  - El cocinero visualiza los platos en preparación en el KDS.
  - Marca un plato como listo.
  - El sistema actualiza el estado del plato.
  - El sistema dispara automáticamente una notificación al camarero.
- **Flujo alternativo:** 
- El camarero ha editado la comanda mientras el plato estaba en preparación → el KDS ya habrá mostrado el cambio (CU-40) y el cocinero trabaja con la versión actualizada.
- **Postcondición:** El plato queda marcado como listo y el camarero recibe una notificación en tiempo real.
- **Reglas de negocio:**
  - Un plato solo puede pasar a "listo" desde el estado "en preparación".
  - El cambio de estado queda registrado con timestamp en el log de auditoría.

### CU-14 ENVIAR TICKET A CAJA

- **Actor principal:** Camarero/Administrador
-  **Precondición:** El usuario está autenticado y la mesa tiene una comanda activa con todos los platos registrados.
- **Flujo principal:**
  - El camarero selecciona la mesa y accede a la opción "Enviar ticket a caja".
  - El sistema genera el ticket con el desglose de platos, precios y total.
  - El ticket queda registrado en el sistema con: camarero, mesa, zona, timestamp y detalle de platos.
  - El ticket aparece disponible para el Administrador en caja.
- **Flujo alternativo:**
  - El camarero detecta un error tras enviar el ticket → puede reclamarlo para editarlo antes de que sea cobrada esa mesa.
  - Sin conexión → el ticket se genera localmente y se sincroniza al recuperar la conexión.
- **Postcondición:**  El ticket queda registrado en el sistema con log de auditoría completo y disponible para cobro.
- **Reglas de negocio:**
  - El ticket incluye obligatoriamente: identificador de camarero, mesa, zona, timestamp y desglose de platos con precios.
  - Una vez enviado, el ticket solo puede ser editado mediante CU-15 y solo antes de ser cobrado.

### CU-19 CREAR RESERVA

- **Actor principal:** Administrador
-  **Precondición:** El usuario está autenticado como Administrador.
- **Flujo principal:** 
  - El administrador accede al módulo de reservas.
  - Pulsa "Nueva reserva" e introduce: nombre del cliente, número de comensales, fecha, hora y zona asignada.
  - El sistema comprueba disponibilidad de mesas en ese tramo horario.
  - El administrador asigna una mesa disponible.
  - El sistema confirma la reserva y cambia el estado de la mesa a "reservada".
  - El sistema programa automáticamente el aviso de mesa próxima a liberar.
- **Flujo alternativo:**
  - No hay mesas disponibles en ese tramo → el sistema muestra un aviso de conflicto e impide la creación.
  - El administrador cancela el proceso → no se guarda ningún dato.
- **Postcondición:** La reserva queda guardada sin posibilidad de duplicidad, la mesa está asignada y el aviso automático programado.
- **Reglas de negocio:**
  - El sistema programa un aviso automático al camarero 5 minutos antes si la mesa sigue ocupada.
  - No puede haber dos reservas activas para la misma mesa en el mismo tramo horario.

### CU-37 MARCAR PLATO COMO MENÚ DEL DIA

- **Actor principal:** Administrador
-  **Precondición:** El usuario está autenticado como Administrador y el plato existe en la carta.
- **Flujo principal:**
  - El administrador accede al módulo de carta.
  - Selecciona uno o varios platos y los marca como "menú del día".
  - El sistema actualiza los platos marcados y los hace disponibles como opciones de menú al tomar una comanda.
- **Flujo alternativo:** 
  - El administrador desmarca un plato del menú del día → el plato vuelve a estar disponible solo como carta.
- **Postcondición:** Los platos marcados aparecen como opciones de menú del día en el flujo de toma de comanda.
- **Reglas de negocio:**
  - Debe haber al menos un plato marcado como primero y uno como segundo para activar el menú del día.
  - El precio del menú completo es único independientemente de la combinación elegida por el comensal.

### CU-38 ENVIAR NOTIFICACIÓN DE PLATO LISTO

- **Disparador:** Sistema de Notificaciones (proceso automático interno)
- **Precondición:** Un cocinero ha marcado un plato como listo.
- **Flujo principal:**
  - El sistema detecta el evento "plato listo".
  - Identifica el camarero responsable de la mesa.
  - Envía una notificación en tiempo real vía WebSocket al dispositivo del camarero.
  - El camarero recibe un aviso con la mesa y el plato listo.
- **Flujo alternativo:**
  - El dispositivo del camarero no está conectado al WebSocket → el sistema reintenta la notificación al reconectarse.
- **Postcondición:**  El camarero es informado en tiempo real de que sus platos están listos para servir.
- **Reglas de negocio:**
  - La notificación incluye: número de mesa, zona y nombre del plato listo.
  - El evento queda registrado en el log de auditoría con timestamp.

### CU-39 ENVIAR AVISO DE MESA PRÓXIMA A LIBERAR

- **Disparador:** Sistema de Notificaciones (proceso automático interno)
- **Precondición:** Existe una reserva activa para una mesa que en ese momento está ocupada.
- **Flujo principal:**
  - El sistema ejecuta un proceso automático cada 60 segundos.
  - Comprueba si alguna mesa ocupada tiene una reserva en los próximos 5 minutos.
  - Si se cumple la condición, envía un aviso vía WebSocket al camarero responsable de esa mesa.
  - El camarero recibe el aviso con la mesa afectada y el tiempo restante.
- **Flujo alternativo:**
  - La mesa se libera antes del aviso → el sistema cancela el aviso programado.
  - No hay mesas en esa situación → el proceso termina sin enviar nada.
- **Postcondición:** El camarero es avisado con antelación para gestionar el doblado de mesa a tiempo.
- **Reglas de negocio:**
  - El aviso se envía exactamente cuando quedan 5 minutos o menos para la reserva.
  - Solo se envía un aviso por reserva, no se repite cada 60 segundos.



