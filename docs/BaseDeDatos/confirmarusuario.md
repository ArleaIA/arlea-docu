# Diagrama de Secuencia — Confirmación de Usuario (Trigger + Function)
## Descripción

Este diagrama describe el flujo automático que ocurre cuando un usuario confirma su correo electrónico en Supabase Auth.
Mediante un trigger y una función en PostgreSQL, el sistema migra al usuario desde una tabla temporal a la tabla definitiva.
El objetivo es asegurar consistencia entre autenticación y datos de dominio.

![Diagrama de Secuencia — Confirmación de Usuario](/img/insertar-usuario-confirmado.png)

## Componentes involucrados

- auth.users (Supabase Auth)
- Trigger trg_llamar_insertar_usuario_confirmado
- Función insertar_usuario_confirmado()
- Tabla usuarios_pendientes
- Tabla usuario

## Flujo / Comportamiento

1. Supabase actualiza el campo email_confirmed_at en auth.users.
2. Se ejecuta el trigger trg_llamar_insertar_usuario_confirmado.
3. El trigger invoca la función insertar_usuario_confirmado(NEW.id).
4. La función busca al usuario en usuarios_pendientes.
5. Si el usuario existe:
    - Inserta el registro en la tabla usuario.
    - Elimina el registro de usuarios_pendientes.
6. Si el usuario no existe:
    - Se emite un RAISE NOTICE.
7. El trigger retorna NEW y finaliza la transacción.

## Consideraciones técnicas

- El flujo es completamente server-side.
- Garantiza atomicidad entre confirmación y creación de usuario.
- Evita usuarios huérfanos o inconsistentes.
- El trigger solo se ejecuta una vez por confirmación.

## Observaciones

- Permite desacoplar Auth de la lógica de negocio.
- Puede extenderse para auditoría o métricas.
- Es clave para flujos de registro confiables.