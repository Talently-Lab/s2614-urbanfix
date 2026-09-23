# Guía de Contribución (CONTRIBUTING.md)

¡Bienvenido al equipo de UrbanFix Solutions! Este documento establece las reglas de oro para mantener el orden en nuestro **monorepositorio**, ya que los equipos de Frontend y Backend conviven en el mismo lugar.

## 1. Flujo de Ramas (Git Workflow)

Para evitar conflictos y mantener claro qué parte del proyecto se está modificando, utilizaremos un sistema de prefijos obligatorios para todas las ramas nuevas.

- **Para el Frontend:** `front/` seguido del tipo de tarea y el nombre.
  - Ejemplos: `front/feat-login`, `front/fix-header-responsive`
- **Para el Backend:** `back/` seguido del tipo de tarea y el nombre.
  - Ejemplos: `back/feat-endpoint-servicios`, `back/fix-auth-token`
- **Para tareas generales (Docs, config):** `chore/` o `docs/`
  - Ejemplos: `docs/actualizar-readme`, `chore/setup-eslint`

### Regla de oro para crear ramas:
Nunca trabajes directamente sobre `main` o `develop`. **La rama `develop` es nuestra única rama de integración compartida.** Siempre crea tu rama a partir de `develop` actualizada:
```bash
git checkout develop
git pull origin develop
git checkout -b front/feat-mi-nueva-tarea
```

## 2. Convención de Commits

Utilizamos [Conventional Commits](https://www.conventionalcommits.org/) con la adición de "Scopes" (ámbitos) para saber exactamente qué área del monorepo se vio afectada.

El formato es: `tipo(scope): mensaje claro y en imperativo`

**Tipos permitidos:**
- `feat`: Nueva característica (feature).
- `fix`: Corrección de un error (bug).
- `docs`: Cambios en documentación.
- `chore`: Tareas de mantenimiento (dependencias, configuraciones).
- `refactor`: Cambios de código que no corrigen errores ni añaden funcionalidades (ej. limpieza de código).

**Scopes (ámbitos) permitidos:**
- `front`: Cambios en la carpeta `/frontend`
- `back`: Cambios en la carpeta `/backend`
- `root`: Cambios en la raíz del proyecto (ej. README)

**Ejemplos correctos:**
- `feat(front): crear formulario de inicio de sesión`
- `fix(back): corregir validación de contraseña en endpoint /login`
- `docs(root): actualizar guía de contribución`

## 3. Pull Requests (PRs)

1. Cuando termines tu tarea, haz push de tu rama y crea un PR hacia **la rama `develop`** (no hacia `main`). La rama `main` está reservada estrictamente para el pase a Producción.
2. Al crear el PR, se cargará automáticamente una plantilla. **Debes completarla obligatoriamente.**
3. **Revisión Cruzada:** Ningún PR puede ser mergeado ("unido") sin al menos **1 aprobación (Approve)** de otro miembro del equipo, idealmente un desarrollador de tu misma área (Front con Front, Back con Back) o un QA.
4. Mantén tus PRs pequeños. Es más fácil revisar 5 PRs pequeños que 1 PR gigante con 40 archivos modificados.

## 4. Sincronización de Equipo

- Si tu tarea en el Frontend depende de un endpoint que el Backend aún no ha terminado, repórtalo en tu herramienta de gestión (Trello/Notion) y avanza usando datos falsos (*mocks*).
- Si hay bloqueos, usa los canales de comunicación (Slack/Discord) mencionando al encargado.
