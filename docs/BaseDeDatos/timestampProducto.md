# Diagrama de Secuencia — Actualización Automática de Timestamp en Producto
## Descripción

Este diagrama representa el flujo de actualización automática del campo timestamp en la tabla producto.
Un trigger asegura que cualquier modificación actualice la fecha sin intervención de la aplicación.
El objetivo es mantener trazabilidad temporal consistente.

![Diagrama de Secuencia — Actualización Automática de Timestamp en Producto](/img/actualizacion-de-timestamp-de-producto.png)

# Componentes involucrados

- Aplicación
- Tabla producto
- Trigger trg_update_producto_timestamp
- Función update_producto_timestamp()

## Flujo / Comportamiento

1. La aplicación ejecuta un UPDATE sobre la tabla producto.
2. Antes del update, se dispara el trigger BEFORE UPDATE.
3. El trigger ejecuta la función update_producto_timestamp().
4. La función asigna NEW.timestamp = NOW().
5. Se retorna NEW.
6. PostgreSQL completa la operación de actualización.
7. La aplicación recibe confirmación de UPDATE OK.

## Consideraciones técnicas

- El timestamp no depende del backend.
- Evita errores por olvidos en el código.
- Centraliza la lógica temporal en la base de datos.
- Es transparente para cualquier cliente.

## Observaciones

- Patrón reutilizable en otras tablas.
- Ideal para auditoría y sincronización.
- Compatible con batch updates y workers.