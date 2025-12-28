# Diagrama Relacional — Esquema Físico de Base de Datos
## Descripción

Este diagrama representa el modelo relacional físico implementado en PostgreSQL.
Incluye tablas, claves primarias, claves foráneas y tablas intermedias.
El objetivo es documentar la estructura real de persistencia.

![Diagrama Relacional — Esquema Físico de Base de Datos](/img/modelo-relacional.png)

## Componentes involucrados

- Tablas principales (usuario, producto, colorimetria, etc.)
- Tablas intermedias (producto_tienda, makeup_producto, cosmetiquero_producto)
- Relaciones FK
- Campos técnicos (timestamps, ids)

## Consideraciones técnicas

- Normalización hasta N:M.
- Integridad referencial garantizada.
- Compatible con Supabase y RPC.
- Preparado para escalabilidad.

## Observaciones

- Refleja fielmente la implementación actual.
- Base para optimización e índices.
- Útil para onboarding técnico.