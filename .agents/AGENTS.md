# UrbanFix Solutions - Reglas para Agentes (Agent Guidelines)

Como asistente de IA trabajando en este espacio de trabajo (Workspace), DEBES seguir rigurosamente las siguientes reglas para mantener la integridad del proyecto.

## 1. Conciencia de Monorepositorio
Este proyecto es un **Monorepo** que contiene dos aplicaciones principales:
- `/frontend`: Aplicación web (React).
- `/backend`: API REST (Node.js + Express).
Siempre que se te pida realizar una tarea, identifica claramente si pertenece al frontend, al backend, o a ambos, y restringe tus modificaciones a los directorios correctos. No mezcles dependencias entre ellos.

## 2. Convenciones de Git
Utilizamos convenciones estrictas para evitar conflictos entre los equipos.
- **Ramas (Branches):** Deben iniciar con el prefijo `front/` o `back/` (ej. `front/feat-login`, `back/fix-auth`). Las tareas generales usan `chore/` o `docs/`.
- **Commits:** Deben usar [Conventional Commits] con los *scopes* obligatorios: `(front)`, `(back)`, o `(root)`. (Ej. `feat(front): agregar botón de login`).

## 3. Restricciones del MVP (Prevención de Scope Creep)
NUNCA implementes funcionalidades que estén fuera del alcance del MVP (Producto Mínimo Viable), a menos que el usuario lo exija explícitamente y reconozca la desviación.
- **Permitido en MVP:** Autenticación (Cliente, Técnico, Admin), gestión de solicitudes de servicio, flujos de aceptar/rechazar trabajo, panel de administración básico.
- **Prohibido (Scope Creep):** Pagos integrados, chat en tiempo real, sistema de reseñas/reputación, geolocalización avanzada, notificaciones push.
Si el usuario pide algo prohibido, recuérdale amablemente que, según `docs/01-MVP-CONTEXT.md`, esa función pertenece a la "Versión 2.0".

## 4. Documentación
Al documentar, respeta la estructura existente en la carpeta `/docs`. No modifiques el `README.md` con detalles técnicos profundos; el README está reservado para la presentación del proyecto (portafolio).
