# Diagrama de Secuencia — RPC obtener_tipos_producto()
## Descripción

Este diagrama describe el uso de una función RPC para obtener los tipos de producto disponibles.
La API delega completamente la lógica de consulta y ordenamiento a PostgreSQL.
El objetivo es optimizar consultas repetidas y simplificar el backend.

![Diagrama de Secuencia — RPC obtener_tipos_producto()](/img/RPC-de-obtener-categorias.png)

## Componentes involucrados

- API
- PostgreSQL
- Función obtener_tipos_producto()

## Flujo / Comportamiento

1. La API recibe una solicitud.
2. ejecuta la llamada RPC obtener_tipos_producto().
3. PostgreSQL invoca la función.
4. La función ejecuta SELECT DISTINCT tipo FROM producto.
5. Los resultados se ordenan internamente.
6. PostgreSQL retorna la lista ordenada de tipos.
7. La API responde al cliente.

## Consideraciones técnicas

- Reduce lógica duplicada en backend.
- Aprovecha optimización del motor SQL.
- Ideal para filtros y catálogos.
- Evita exponer queries directas.

## Observaciones

- Puede extenderse para cache.
- Compatible con i18n o normalización.
- Patrón recomendado para consultas estáticas.