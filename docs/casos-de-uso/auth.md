# Casos de Uso — Auth

## CU-01: Registrar Usuario

**Actor principal:** Usuario final  
**Precondiciones:**  
- El usuario no existe en `auth.users`.

**Flujo principal:**  
1. El usuario ingresa correo y contraseña.  
2. El módulo Auth envía la solicitud a Supabase Auth.  
3. Supabase crea la cuenta y retorna un identificador único.  
4. La API crea el registro correspondiente en la tabla `usuario`.  
5. Se retorna un token JWT válido.

**Postcondición:**  
- El usuario queda registrado y autenticado.

---

## CU-02: Iniciar Sesión

**Actor:** Usuario final  
**Precondición:** Usuario registrado.

**Flujo:**  
1. El usuario ingresa sus credenciales.  
2. Supabase valida la autenticación.  
3. La API entrega un token JWT.  
4. El usuario obtiene acceso al sistema.

**Postcondición:**  
- Token de autenticación válido habilitado.
