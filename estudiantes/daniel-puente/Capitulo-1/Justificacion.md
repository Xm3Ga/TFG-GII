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
