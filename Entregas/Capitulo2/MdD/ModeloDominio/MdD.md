# Modelo de Dominio

![Modelo de Dominio](./MdD.svg)

Una `Organizacion` es la empresa cliente que utiliza Theia Officer. Cada organización tiene `Usuarios` que interactúan con el sistema, `Credenciales` cifradas para conectar con los `ProveedoresCloud` y `AgentesIA` desplegados en dichos proveedores.

La `Mision` es el concepto central: un flujo de trabajo guiado por el `CAIO` para configurar y aplicar políticas de seguridad. Se especializa en tres tipos: `MisionDeGuardarrail`, `MisionDeMinimoPrivilegio` y `MisionDeAutonomia`.

## Guardarraíles

Un `Guardarrail` agrupa varias políticas de seguridad (`PoliticaDeContenido`, `PoliticaDeTemas`, `PoliticaDePII`, `PoliticaDeGrounding`) y se aplica a uno o varios `AgentesIA`. La `MisionDeGuardarrail` configura y crea guardarraíles en los proveedores cloud.

## Mínimo Privilegio

Cada `AgenteIA` puede tener una identidad runtime asociada. La `MisionDeMinimoPrivilegio` produce una baseline por agente, comparando permisos actuales, evidencias disponibles y permisos propuestos. Si el usuario aprueba la baseline, el sistema crea una ejecución de aplicación con snapshot de rollback.

## Autonomía

Cada `AgenteIA` puede presentar señales de riesgo en sus metadatos. La `MisionDeAutonomia` genera un perfil de autonomía por agente, con nivel, confianza, evidencias y validaciones humanas propuestas. No crea reglas de aprobación runtime en el MVP.

## Trazabilidad

Todas las operaciones quedan registradas en `RegistroDeAuditoria`, garantizando la trazabilidad completa del proceso de securización.
