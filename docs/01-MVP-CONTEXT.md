# Contexto del MVP - UrbanFix Solutions

Este documento define el problema de negocio, la solución propuesta y el alcance estricto que debemos cumplir para el Producto Mínimo Viable (MVP) en el marco de las 8 semanas de desarrollo.

## 1. El Problema
Contratar oficios (plomeros, electricistas, etc.) es actualmente un caos. El proceso es informal (generalmente vía WhatsApp o el "boca en boca"), no hay registro del estado del trabajo, y la confianza suele ser baja.

## 2. La Solución (Hipótesis)
Crear una plataforma web que conecte a clientes con técnicos de forma estructurada, organizada y transparente, funcionando como un marketplace bilateral.

## 3. Alcance Estricto (Scope)

**⚠️ IMPORTANTE (Prevención del Scope Creep):** Cualquier idea que no esté en la columna de "SÍ está incluido" debe anotarse para la Versión 2.0. No implementarla en el MVP.

### ✅ SÍ está incluido:
- Registro y login para 3 tipos de roles.
- Creación de solicitudes de servicio por parte de los clientes.
- Visualización y gestión del estado de las solicitudes.
- Flujo para que el técnico acepte o rechace trabajos.
- Panel de administración básico para moderar la plataforma.
- Diseño Web Responsive (adaptable a celulares).

### ❌ NO está incluido:
- Pagos integrados (la plataforma no cobra comisiones en el MVP).
- Sistema de reseñas o reputación.
- Chat en tiempo real.
- Aplicación móvil nativa (iOS/Android).
- Notificaciones push.
- Geolocalización avanzada o mapas integrados.

## 4. Roles y Permisos (RBAC)

La lógica de negocio depende en gran medida de separar correctamente lo que cada usuario puede hacer.

| Rol | Descripción y Permisos Clave |
|---|---|
| **Cliente** | Busca ayuda. Puede crear solicitudes de servicio, ver el historial de sus solicitudes, y cancelar solicitudes pendientes. No puede ver solicitudes de otros clientes. |
| **Técnico** | Ofrece sus servicios. Puede ver trabajos disponibles, aceptar o rechazar solicitudes, y cambiar el estado del trabajo a "Completado". No puede crear solicitudes. |
| **Administrador** | Modera la plataforma. Tiene acceso a todas las solicitudes de todos los usuarios y puede gestionar (suspender) cuentas en caso de problemas. |
