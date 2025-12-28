# Manejo de Errores
## Descripción

Este documento describe la estrategia de manejo de errores implementada en el Worker, cuyo objetivo es garantizar continuidad operativa, trazabilidad y ausencia de pérdida de datos durante el procesamiento ETL.

El manejo de errores está diseñado para aislar fallos por unidad de procesamiento, evitando bloqueos globales y permitiendo análisis posterior mediante auditoría.

## Objetivo

- Garantizar que los errores no detengan el Worker.
- Evitar pérdida de mensajes provenientes de Redis.
- Asegurar consistencia de datos mediante transacciones.
- Proveer trazabilidad completa para debugging y operación.
- Permitir reprocesamiento controlado de mensajes fallidos.

## Tipos de Errores
**Errores de Validación**

Ocurren cuando el payload recibido no cumple con la estructura mínima requerida.

Ejemplos:

- Campos obligatorios ausentes.
- Tipos de datos inválidos.
- Estructura incompatible con el modelo esperado.

Estos errores impiden continuar con la normalización y el mensaje no se persiste.

## Errores de Normalización

Se producen cuando los datos, aun siendo estructuralmente válidos, no pueden ser transformados al modelo interno.

Ejemplos:

- Datos imposibles de mapear.
- Inconsistencias detectadas por reglas de negocio.
- Valores incompatibles con la semántica del dominio.

El procesamiento del mensaje se detiene antes de iniciar la persistencia.

## Errores de Persistencia

Ocurren durante la ejecución de operaciones en la base de datos.

Ejemplos:

- Violaciones de integridad referencial.
- Errores de tipo o overflow.
- Fallos de conexión o timeout.
- Errores durante COMMIT o ROLLBACK.

Estos errores se manejan siempre dentro de una transacción.

## Estrategia de Manejo
**Aislamiento por Producto**

Cada mensaje se procesa como una unidad independiente.

Características:

- Una transacción por producto.
- Un fallo no afecta a otros mensajes.
- El Worker continúa procesando tras un error.

## Uso de Colas de Procesamiento

El Worker utiliza colas auxiliares _processing para controlar el estado de cada mensaje.

Beneficios:

- Evita pérdida de mensajes ante reinicios.
- Permite detectar mensajes atascados.
- Facilita recuperación automática de lotes.

## Manejo de Fallos en Persistencia

Ante un error durante la persistencia:

1. Se ejecuta ROLLBACK de la transacción activa.
2. El mensaje se elimina de la cola _processing.
3. Se registra el error en auditoría con contexto completo.
4. El Worker continúa con el siguiente mensaje.

No se produce bloqueo ni detención del flujo global.

## Errores Críticos

Se consideran errores críticos aquellos que no pueden ser resueltos de forma automática.

Estrategia aplicada:

- Registro completo en auditoría.
- Conservación del contexto del mensaje.
- Descarte controlado del mensaje para evitar loops infinitos.
- Posibilidad de reprocesamiento manual.

## Auditoría de Errores

Cada error registrado incluye:

- Tipo de error.
- Mensaje descriptivo.
- Contexto del producto.
- Cola de origen.
- Timestamp del evento.

La auditoría permite análisis posterior y toma de decisiones operativas.

## Consideraciones técnicas

- La auditoría no bloquea el flujo principal.
- El Worker no reintenta indefinidamente un mensaje fallido.
- No existe dependencia directa con sistemas externos.
- El manejo de errores está centralizado en el WorkerService.
- El diseño prioriza simplicidad y robustez operativa.

## Observaciones

- La estrategia facilita debugging y mantenimiento.
- Permite escalar el Worker sin complejidad adicional.
- Es compatible con monitoreo y alertas futuras.
- Provee una base sólida para operación en producción.