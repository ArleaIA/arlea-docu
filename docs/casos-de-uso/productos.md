# Casos de Uso — Productos

## CU-07: Obtener Todos los Productos

**Actor:** Usuario  

**Flujo:**  
1. El usuario consulta el endpoint `/productos`.  
2. La API obtiene todos los registros de la tabla `producto`.  
3. La API retorna la lista completa de productos.

---

## CU-08: Buscar Productos por Palabra Clave

**Actor:** Usuario  

**Flujo:**  
1. El usuario ejecuta la consulta en `/productos/search?query=...`.  
2. La API filtra por coincidencias en el campo `producto.nombre`.  
3. Se retorna la lista filtrada.

---

## CU-09: Consultar Detalle de un Producto

**Actor:** Usuario  

**Flujo:**  
1. El usuario solicita el recurso `/productos/:id`.  
2. La API consulta las siguientes tablas:  
   - `producto`  
   - `producto_tienda`  
   - `tienda`  
3. La API retorna la información consolidada, incluyendo:  
   - Nombre  
   - Precio normal y precio en oferta  
   - Imagen  
   - URL  
   - Disponibilidad por tienda  
   - Colores asociados
