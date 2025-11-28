# Casos de Uso — Usuario

## CU-03: Consultar Perfil del Usuario

**Actor:** Usuario  
**Precondición:** Autenticación válida.

**Flujo:**  
1. El usuario solicita su información personal.  
2. La API consulta la tabla `usuario`.  
3. La API retorna el perfil del usuario.

---

## CU-04: Actualizar Perfil del Usuario

**Actor:** Usuario  
**Precondiciones:**  
- Usuario autenticado.

**Flujo:**  
1. El usuario envía los nuevos datos.  
2. La API actualiza la tabla `usuario`.  
3. El sistema retorna el perfil actualizado.
