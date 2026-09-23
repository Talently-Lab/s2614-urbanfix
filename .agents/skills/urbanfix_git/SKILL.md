---
name: UrbanFix Git Workflow
description: Guía automática para crear ramas y escribir commits siguiendo las convenciones del monorepo de UrbanFix (prefijos front/back).
---

# Habilidad: UrbanFix Git Workflow

Cuando el usuario te pida crear una nueva rama, hacer commits de los cambios realizados, o preparar el repositorio para un Pull Request, DEBES seguir estas instrucciones exactas:

## 1. Creación de Ramas
Nunca trabajes ni hagas commits directamente sobre la rama `main` o `develop`.
Dependiendo de los archivos modificados, crea la rama con el prefijo correcto:
- Si modificaste código en `/frontend`: `git checkout -b front/feat-<nombre-descriptivo>`
- Si modificaste código en `/backend`: `git checkout -b back/fix-<nombre-descriptivo>`
- Si modificaste archivos en la raíz (ej. documentación): `git checkout -b docs/<nombre-descriptivo>`

## 2. Formato de Commits
Este repositorio usa Conventional Commits adaptado a Monorepo. El *scope* (ámbito) es obligatorio.

Formato requerido: `<tipo>(<scope>): <mensaje en imperativo>`

- **Tipos permitidos:** `feat` (nueva característica), `fix` (corrección), `docs` (documentación), `chore` (mantenimiento/configuración), `refactor` (refactorización).
- **Scopes permitidos:** `front`, `back`, `root`.

**Ejemplos de comandos a ejecutar:**
```bash
git commit -m "feat(front): crear componente de tarjeta de servicio"
git commit -m "fix(back): resolver caída del servidor en endpoint de login"
git commit -m "docs(root): actualizar instrucciones de despliegue"
```

## 3. Asistencia en Pull Requests
Si el usuario te pide ayuda para crear un PR o documentar los cambios para GitHub:
1. Lee los commits recientes de la rama actual para entender los cambios.
2. Genera automáticamente el texto para la descripción del PR basándote en el archivo `.github/PULL_REQUEST_TEMPLATE.md` del repositorio.
3. Recuerda al usuario que necesita al menos 1 aprobación cruzada de su equipo (Front/Back) o de QA antes de hacer merge.
