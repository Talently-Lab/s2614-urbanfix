# Entornos y Flujos de Despliegue

Ya que el proyecto es un **Monorepo**, Frontend y Backend comparten repositorio pero tienen ciclos de ejecución y despliegue independientes.

## 1. Entorno de Desarrollo (Local)

Para trabajar de forma local, cada equipo operará dentro de su carpeta correspondiente.

### Frontend (`/frontend`)
- Entrar a la carpeta: `cd frontend`
- Instalar dependencias: `npm install`
- Ejecutar servidor de desarrollo: `npm run dev`
- **Variables de Entorno:** Copiar `.env.example` a `.env` y configurar la URL base de la API. (Por defecto apuntará al servidor local del backend ej: `http://localhost:3000`).

### Backend (`/backend`)
- Entrar a la carpeta: `cd backend`
- Instalar dependencias: `npm install`
- Ejecutar servidor de desarrollo: `npm run dev` (o similar, definido en su `package.json`).
- **Variables de Entorno:** Copiar `.env.example` a `.env` e ingresar las credenciales de la base de datos (PostgreSQL) local o compartida de desarrollo.

## 2. Entorno de Producción / Staging (Free Tiers)

Usaremos herramientas gratuitas (Vercel para Frontend, Railway/Render para Backend). El despliegue se activa automáticamente (CI/CD nativo) al enviar cambios a la rama principal (ej: `main`).

### Despliegue del Frontend (Vercel)
Vercel se conectará al repositorio de GitHub de UrbanFix.
**Configuración clave en el Dashboard de Vercel:**
- **Framework Preset:** Vite (o el que corresponda a React).
- **Root Directory:** Se debe establecer explícitamente en `frontend`. Esto le indica a Vercel que ignore la carpeta del backend.
- **Environment Variables:** Agregar las variables necesarias para producción (URL del backend en la nube).

### Despliegue del Backend (Railway / Render)
El servicio de backend se conectará al mismo repositorio de GitHub.
**Configuración clave en el Dashboard:**
- **Root Directory:** Debe establecerse explícitamente en `backend`.
- **Start Command:** Comando necesario para ejecutar en producción (ej. `npm start`).
- **Environment Variables:** URL de la base de datos de producción (Prisma Postgres, Supabase, o la DB interna de Railway) y JWT secrets.

## 3. Entornos Preview (Opcional para QA)

- Vercel ofrece **Preview Deployments** gratuitos. Cada Pull Request generará una URL temporal del Frontend, ideal para que el equipo de QA valide visualmente los cambios antes de hacer merge.
- Para el Backend, si Render o Railway no ofrecen Preview Deployments en la capa gratuita, el QA deberá probar contra el entorno principal (producción) o levantar la rama localmente para validaciones exhaustivas.
