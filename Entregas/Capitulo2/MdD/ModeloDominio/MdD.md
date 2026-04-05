# Modelo de Dominio

![Modelo de Dominio](./MdD.svg)

Una `Organizacion` es la empresa cliente que utiliza la plataforma Horizon. Cada organización tiene `Usuarios` que interactúan con el sistema, `Credenciales` para conectar con los `ProveedoresCloud` y `AgentesIA` desplegados en dichos proveedores.

La `Mision` es el concepto central: un flujo de trabajo guiado por el `CAIO` para configurar y aplicar políticas de seguridad. Se especializa en tres tipos: `MisionDeGuardarrail`, `MisionDeMinimoPrivilegio` y `MisionDeAutonomia`.

## Guardarraíles

Un `Guardarrail` agrupa varias políticas de seguridad (`PoliticaDeContenido`, `PoliticaDeTemas`, `PoliticaDePII`, `PoliticaDeGrounding`) y se aplica a uno o varios `AgentesIA`. La `MisionDeGuardarrail` configura y crea guardarraíles en los proveedores cloud.

## Mínimo Privilegio

Cada `AgenteIA` tiene `Permisos` asignados y genera un `PerfilDeUso` a partir de los logs de actividad del proveedor. La `MisionDeMinimoPrivilegio` produce un `AnalisisDePrivilegios` por agente, que compara permisos asignados con permisos utilizados. El análisis genera `RecomendacionesDeReduccion`, cada una señalando un `Permiso` específico que se recomienda eliminar.

## Autonomía

Cada `AgenteIA` puede ejecutar `OperacionesDeRiesgo`, clasificadas en `CategoriasDeRiesgo` (financiero, datos personales, infraestructura, comunicaciones). La `MisionDeAutonomia` genera una `PoliticaDeAutonomia` por agente, que asigna un `NivelDeAutonomia` (total, supervisado, restringido) y define `ReglasDeSupervision`. Cada regla especifica qué operaciones requieren `AprobacionHumana`, quién es el `Usuario` aprobador y el comportamiento en caso de timeout.

## Trazabilidad

Todas las operaciones quedan registradas en `RegistroDeAuditoria`, garantizando la trazabilidad completa del proceso de securización.
