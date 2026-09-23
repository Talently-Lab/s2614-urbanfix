¡Bienvenido/a al proyecto UrbanFix Solutions!
Durante las próximas 8 semanas vas a construir un producto digital colaborativo, fortaleciendo tus competencias técnicas en un entorno realista[cite: 1]. Lucas Fernández, fundador de la startup, detectó que contratar oficios (plomeros, electricistas) es un caos informal vía WhatsApp. La misión del equipo es lanzar una plataforma web (Marketplace) que conecte a clientes con técnicos independientes de forma estructurada.

🎯 Vas a poder mostrar una SPA completa con autenticación, manejo de múltiples roles, consumo de API REST y deployment en producción. Exactamente la evidencia técnica que buscan las empresas en perfiles junior.

## 📚 Documentación del Proyecto

Toda la documentación técnica, guías de contribución y detalles operativos se encuentran en archivos separados. **Por favor, revisa esta documentación antes de empezar a escribir código.**

- 📖 [Contexto del MVP y Reglas de Negocio](docs/01-MVP-CONTEXT.md)
- ⚙️ [Entornos de Desarrollo y Despliegue](docs/02-ENVIRONMENTS.md)
- 🤝 [Guía de Contribución y Git Workflow](CONTRIBUTING.md)

---

UrbanFix Solutions es un proyecto colaborativo diseñado para que demuestres tus habilidades en un entorno realista. Al finalizar la Semana 8, estos son los entregables obligatorios por rol para lanzar este Marketplace:

🚀 Project Manager
Tablero Kanban configurado y priorizado.
Control del Scope Creep (Frenar nuevas ideas).
Coordinación de la Demo Final y Retrospectiva.

🟢 Back End Developer
API REST deployada y documentada.
Gestión de permisos complejos (Cliente, Técnico, Admin).
CRUD y lógica de solicitudes de servicio.

🔵 Front End Developer
SPA deployada con autenticación.
Paneles diferenciados por tipo de rol.
Lógica visual de estados (Pendiente, Aceptado).

🩷 UX/UI Designer
Prototipo navegable responsivo.
UI Kit e identidad de marca.
Flujo claro de reserva y aceptación de trabajo.

🟡 QA Tester
Test Plan de estados de servicio.
Validación de permisos cruzados (RBAC).
Reporte de calidad del MVP final.

📊 Data Analyst
Matriz de telemetría y eventos del Marketplace.
Diseño de métricas de «Match» (Oferta vs Demanda).
Dashboard analítico para el Administrador.

📈 Especialista de Marketing
Redacción de correos transaccionales (Nuevo trabajo).
Microcopy que genere confianza en los usuarios.
Pitch comercial para la presentación final.

🎨 Diseñador Gráfico
Identidad visual corporativa.
Set de íconos representativos de oficios.
Mockups realistas para el portfolio.


🧠 Mentalidad profesional: Reglas de oro Antes de abrir el editor de código, Figma o Excel, interiorizá estas reglas:

Tratá cada entregable (código, diseño, reporte) como si tu Tech Lead o Director lo fuera a revisar hoy.

Si te trabás, investigá 20–30 minutos leyendo documentación antes de pedir ayuda. Documentá lo que intentaste.

La comunicación escrita clara en Trello/Notion vale tanto como el trabajo técnico.

Un MVP incompleto pero bien documentado supera a un proyecto completo que nadie sabe cómo funciona.

No esperes tener todo el panorama claro para empezar. Avanzá con las certezas que tenés.

Herramientas Generales (Todos los roles)

Control de versiones de archivos y código (GitHub / Google Drive corporativo).

Tablero Kanban (Trello, Asana o Notion).

Herramienta de comunicación asincrónica (Discord o Slack).

Stack y Herramientas por Rol

Project Manager: Trello / Notion para gestión ágil y minutas.

Diseño Gráfico: Illustrator, Photoshop o Canva para manual de marca y assets.

Marketing: Google Sheets (planificación), Meta Ads (simulación), Google Analytics.

Data Analyst: Excel/Google Sheets, SQL, Power BI o Looker Studio.

UX/UI: Figma (plan gratuito).

Frontend: React + Vite, Tailwind CSS o CSS vanilla, VS Code.

Backend: Node.js + Express, PostgreSQL o MongoDB, VS Code.

QA Tester: Postman, Google Sheets (casos de prueba), Trello (reporte de bugs).

📁 Estructura de repositorios y carpetas recomendada
Al ser un **Monorepo**, la estructura centralizada es la siguiente:

```text
urbanfix-workspace/
├── backend/                  # API REST (Node.js + Express) 
├── frontend/                 # Aplicación Web (React)
├── docs/                     # Documentación técnica ampliada (MVP, Entornos)
├── product_y_growth/         # Áreas de Negocio (Marketing, Data, Design)
├── CONTRIBUTING.md           # Guía de git y PRs
└── README.md                 # Presentación del proyecto para el portfolio
```

## ⚙️ ¿Cómo empezar?
Para conocer cómo clonar el proyecto, qué ramas utilizar y cómo desplegar, lee:
1. [Guía de Contribución](CONTRIBUTING.md) (Ramas y PRs).
2. [Guía de Entornos](docs/02-ENVIRONMENTS.md) (Local y Prod).