# Arquitectura del Worker

## Descripción

Este documento describe la arquitectura técnica del Worker encargado del procesamiento ETL de productos.
El Worker es un microservicio independiente desarrollado en NestJS cuya responsabilidad es consumir datos desde Redis, normalizarlos y persistirlos de forma transaccional en PostgreSQL (Supabase).

El diseño del Worker prioriza desacoplamiento, tolerancia a fallos, trazabilidad y continuidad operativa.
La arquitectura se apoya en colas Redis, separación clara de responsabilidades y auditoría estructurada.

## Diagrama de Componentes — Worker

### Descripción

Este diagrama describe la arquitectura de componentes internos del Worker NestJS y su interacción con sistemas externos.
Muestra cómo el Worker consume mensajes desde Redis, orquesta la normalización, ejecuta persistencia transaccional y registra auditoría.

El diseño prioriza responsabilidad única por componente, aislamiento de errores y facilidad de escalamiento.

![Diagrama de Componentes del Worker](/img/worker-componentes.png)


### Componentes involucrados
- Scraper (Productor de datos)
- Redis
- Worker NestJS
- WorkerService
- RedisService
- Normalizer
- DatabaseService
- AuditoriaService
- PostgreSQL / Supabase

### Flujo / Comportamiento

1.- El Scraper envía productos crudos a Redis mediante colas por tienda.
2.- Redis almacena los mensajes como sistema desacoplado de ingesta.
3.- El Worker NestJS consume los mensajes desde Redis a través del RedisService.
4.- El WorkerService actúa como orquestador del flujo completo.
5.- El WorkerService delega la normalización de datos al Normalizer.
6.- El WorkerService delega la persistencia transaccional al DatabaseService.
7.- El DatabaseService inserta o reutiliza entidades en PostgreSQL (Supabase).
8.- El WorkerService registra eventos y errores mediante el AuditoriaService.

### Consideraciones técnicas

- El Worker no expone endpoints HTTP.
- Redis desacopla scraping y persistencia.
- La lógica de negocio está centralizada en el WorkerService.
- La persistencia está completamente aislada en DatabaseService.
- La auditoría es transversal y no bloqueante.

### Observaciones

- La arquitectura facilita escalamiento horizontal del Worker.
- Cada componente tiene una única responsabilidad.
- El diseño simplifica mantenimiento y debugging.
- Permite extender el flujo sin impactar a la API principal.

## Diagrama de Secuencia — Procesamiento de Productos en Worker

### Descripción

Este diagrama representa el flujo completo de procesamiento de productos dentro del Worker, desde la recuperación del mensaje en Redis hasta la confirmación de la persistencia o el manejo de errores.

El flujo utiliza colas auxiliares (_processing) y transacciones SQL para garantizar que no exista pérdida de datos incluso ante fallos del proceso.

![Diagrama de Secuencia de Procesamiento de Productos en Worker](/img/worker-secuencia.png)


### Componentes involucrados
- Redis
- Worker NestJS
- RedisService
- WorkerService
- Normalizer
- DatabaseService
- AuditoriaService
- PostgreSQL / Supabase

### Flujo / Comportamiento

1. El Worker inicia y registra el evento de arranque en auditoría.
2. El Worker consulta Redis para descubrir colas dinámicas asociadas a tiendas.
3. El Worker revisa colas _processing para detectar mensajes atascados.
4. Los mensajes atascados son devueltos a la cola principal.
5. El Worker consume un mensaje usando RPOPLPUSH hacia la cola _processing.
6. Si no existen mensajes, el procesamiento de la cola finaliza.
7. El Worker registra la recepción del producto.
8. El Worker delega la transformación del payload al Normalizer.
9. El Normalizer retorna un objeto normalizado.
10. El Worker inicia una transacción mediante DatabaseService.
11. Si la persistencia es exitosa, se ejecuta COMMIT.
12. El mensaje se elimina de la cola _processing.
13. Se registra el evento de producto insertado.
14. Si ocurre un error, se ejecuta ROLLBACK.
15. El mensaje fallido se elimina de _processing.
16. Se registra el error en auditoría.
17. El Worker continúa con el siguiente mensaje.
### Consideraciones técnicas

- El uso de RPOPLPUSH evita pérdida de mensajes.
- Cada producto se procesa en una unidad con otros 99 producots.
- Un error no bloquea el procesamiento global.
- El sistema es tolerante a reinicios inesperados.
- La auditoría no interfiere con el flujo principal.
### Observaciones

- El flujo permite reprocesamiento manual de mensajes.
- Es compatible con ejecución concurrente de múltiples workers.
- Facilita análisis de errores y métricas operacionales.
- El diseño es consistente con arquitecturas ETL productivas.