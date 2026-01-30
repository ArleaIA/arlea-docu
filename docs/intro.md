---
sidebar_position: 1
---

# Introducción a la Documentación del Proyecto Arlea

Bienvenido a la documentación técnica oficial del proyecto **Arlea**.

## Contenido principal

### 1. Arquitectura
- [Arquitectura API](arquitectura/arquitectura-Api.mdx)
- [Módulo Auth](arquitectura/modulo-auth.mdx)
- [Módulo Usuario](arquitectura/modulo-usuario.mdx)
- [Colorimetría — Crear análisis](arquitectura/colorimetria.mdx)
- [Colorimetría — Obtener análisis](arquitectura/colorimetria-obtener.mdx)
- [Cosmetiquero — Crear](arquitectura/cosmetiquero-crear.mdx)
- [Cosmetiquero — Listar](arquitectura/cosmetiquero-listar.mdx)
- [Influencer](arquitectura/influencer.mdx)
- [Productos — Búsqueda](arquitectura/productos-busqueda.mdx)
- [Productos — Detalle](arquitectura/productos-detalle.mdx)


### 2. Casos de Uso
- [Auth](casos-de-uso/auth.md)
- [Usuario](casos-de-uso/usuarios.md)
- [Colorimetría](casos-de-uso/colorimetria.md)
- [Productos](casos-de-uso/productos.md)

### 3. API (Endpoints)
- [Auth](api/auth.mdx)
- [Usuario](api/usuario.mdx)
- [Colorimetría](api/colorimetria.mdx)
- [Productos](api/productos.mdx)
- [Cosmetiquero](api/cosmetiquero.mdx)
- [Influencer](api/influencer.mdx)
- [Suscripciones](api/suscripciones.mdx)

### 4. Worker
- [Arquitectura del Worker](workers/arquitectura.md)
- [Normalización de Datos](workers/normalizacion.md)
- [Manejo de Errores](workers/manejo-errores.md)

### 5. Base de Datos
- [Modelo Entidad-Relación (Conceptual)](BaseDeDatos/entidadrelacion.md)
- [Modelo Relacional](BaseDeDatos/modeloRelacional.md)
- [Diagrama de Secuencia — Confirmación de Usuario](BaseDeDatos/confirmarusuario.md)
- [Diagrama de Secuencia — Actualización Automática de Timestamp en Producto](BaseDeDatos/timestampProducto.md)
- [Diagrama de Secuencia — RPC obtener_tipos_producto()](BaseDeDatos/obtenerProductos.md)

### 6. Mercado Pago

### [Introducción](MercadoPago/intro.mdx)

### Arquitectura
  - [Flujo general](MercadoPago/arquitectura/flujoGeneral.mdx)
  - [Checkout](MercadoPago/arquitectura/Checkout.mdx)

### Webhooks
  - [Concepto](MercadoPago/arquitectura/webhook.mdx)
  - [Eventos](MercadoPago/webhooks/eventos.mdx)

### API
  - [Endpoints](MercadoPago/api/endpoints.mdx)
  - [Errores](MercadoPago/api/errores.mdx)

### Base de Datos
  - [Modelo relacional](MercadoPago/BaseDeDatos/modelo-relacional.mdx)
  - [Tablas](MercadoPago/BaseDeDatos/tablas.mdx)

- [Seguridad](MercadoPago/seguridad.mdx)
- [Estados](MercadoPago/estados.mdx)
- [Checkout en Producción](MercadoPago/checkoutProduccion.mdx)