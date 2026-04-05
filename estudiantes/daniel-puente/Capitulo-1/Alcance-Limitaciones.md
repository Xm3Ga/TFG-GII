#  Alcance y limitaciones

##  ¿Qué cubre el sistema?

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

##  ¿Qué no cubre el sistema?

Las siguientes funcionalidades quedan fuera del alcance de este proyecto:

**Portal de cliente y carta digital pública**: el sistema es exclusivamente una herramienta de gestión interna. Los clientes no pueden hacer reservas online, consultar la carta ni acceder a la aplicación en ningún momento.

**Integración con plataformas de delivery**: el restaurante no tiene servicio a domicilio. 

**Gestión de inventario y stock**: el control de materias primas y proveedores que no fue identificado como necesidad por el cliente.

**Contabilidad**: requiere conocimiento específico y esta fuera del ámbito del autor.

**App nativa iOS/Android**: la PWA permite instalar la app en cualquier dispositivo con experiencia equivalente a una app nativa, sin los costes de publicar en el App Store y Google Play.


**Gestión de personal y turnos**: es un dominio fuera del alcance definido con el cliente.

--------

### ¿Por qué esta solución y no una comercial?

Hay soluciones como Oracle Simphony que es más completa que lo que se desarrolla aquí, pero ninguna de las opciones analizadas resuelve todos los requisitos de este proyecto.

- La **lógica variable del menú**, el **aviso de doblado por reserva próxima**, la **solicitud de segundos platos** y el **log de tickets con usuario y timestamp** no están implementados en ninguna solución sin intervención del proveedor y coste adicional.

- Las soluciones más completas (Oracle, Revo XEF, Last.app) tienen costes de licencia y hardware que no son asumibles para un restaurante familiar. Este proyecto se despliega sobre la red WiFi y tablets que ya tiene el local, con tecnologías open source sin coste de licencia (React, Node.js, PostgreSQL).

- No depende de un proveedor concreto que podría subir precios.

En resumen, esta solución no pretende ser mejor que las otras opciones analizadas, sino resolver exactamente el problema de este restaurante con los recursos disponibles y de una forma sostenible y economica a largo plazo.

-------------
