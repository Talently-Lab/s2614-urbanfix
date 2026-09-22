# UrbanFix — Semana 1 Data Analyst

**Audiencia:** Backend (y PM / Frontend)  
**Objetivo:** Definir los 3 KPIs de salud del Marketplace, el Data Plan de eventos/campos necesarios, y cómo medir **liquidez**.  
**Alcance:** definición y contrato de datos. No incluye dashboard ni Power BI.

Contexto: [contexto_urbanfix.md](../../context/contexto_urbanfix.md)

---

## 1. Los 3 KPIs principales

| # | KPI | Definición (negocio) | Fórmula | Unidad | Frecuencia | Fuente (eventos) |
|---|-----|----------------------|---------|--------|------------|------------------|
| 1 | **Tasa de solicitudes aceptadas** | % de solicitudes que pasan de “abiertas” a “aceptadas” por un técnico | `aceptadas / creadas` en el período (excluir borradores si existen) | % | diaria / semanal | `service_request.created`, `service_request.accepted` |
| 2 | **Tiempo medio de aceptación (TTA)** | Tiempo promedio desde que el cliente publica hasta que un técnico acepta | `AVG(accepted_at − created_at)` solo sobre solicitudes aceptadas | horas (o minutos) | diaria / semanal | mismos eventos + timestamps |
| 3 | **Ratio Cliente / Técnico activos** | Balance oferta–demanda del Marketplace | `clientes_activos / tecnicos_activos` en el período | ratio (adimensional) | semanal | `user.registered` + actividad (`request` / `accept` / `login`) |

### Definiciones operativas

**Tasa de solicitudes aceptadas**

- Numerador: solicitudes con al menos un estado `accepted` (o equivalente) en el período.
- Denominador: solicitudes con estado inicial `created` / `open` / `pending` creadas en el mismo período.
- Interpretación: cerca de 0 → mucha demanda sin match; cerca de 1 → buena conversión oferta/demanda (revisar también TTA).

**Tiempo medio de aceptación (TTA)**

- Solo solicitudes que fueron aceptadas (no canceladas ni expiradas sin aceptación).
- Si el MVP permite re-apertura, usar el primer `accepted_at` válido.
- Meta orientativa MVP (hipótesis, no SLA del brief): documentar el valor observado; el Admin debería ver distribución (p50 / p90) cuando haya dashboard.

**Ratio Cliente / Técnico activos**

- **Cliente activo:** usuario con rol `client` que creó ≥1 solicitud en el período (alternativa más laxa: login en el período — marcar como Should).
- **Técnico activo:** usuario con rol `technician` que aceptó o rechazó ≥1 solicitud en el período (alternativa: marcado `available` + login).
- Ratio alto (muchos clientes por técnico) → riesgo de baja liquidez; ratio bajo → técnicos ociosos / poca demanda.

### KPIs de soporte (no son los 3 core, útiles para Admin)

| KPI | Fórmula breve | Por qué |
|-----|---------------|---------|
| Tasa de rechazo técnico | `rechazadas / (aceptadas + rechazadas)` | Calidad del match / fatiga de oferta |
| Tasa de cancelación post-aceptación | `canceladas_despues_de_aceptar / aceptadas` | Fricción del flujo (edge cases QA) |
| Solicitudes sin respuesta | creadas sin accept/reject en ventana T | Señal temprana de iliquidez |

---

## 2. Liquidez del Marketplace

### Qué es

En UrbanFix, **liquidez** = probabilidad de que un cliente encuentre un técnico **rápido** (match oferta–demanda en el tiempo).

No es un cuarto KPI suelto: es la métrica “reina” que se construye con los 3 KPIs anteriores + una ventana temporal.

### Cómo medirla (en simple)

Acordamos una **ventana de tiempo T** (propuesta MVP: **24 horas**).  
Pregunta de negocio: *de las solicitudes creadas en el período, ¿qué % consiguió técnico en ≤ 24 h?*

**Paso 1 — Por cada solicitud**

| Condición | Resultado |
|-----------|-----------|
| Tiene `accepted_at` **y** (`accepted_at` − `created_at`) ≤ T | Cuenta como **match rápido** (1) |
| No fue aceptada, o se aceptó **después** de T | **No** cuenta (0) |

**Paso 2 — Del período**

```text
Liquidez = (solicitudes con match rápido) / (solicitudes creadas en el período)
```

Ejemplo: 10 solicitudes creadas; 7 aceptadas dentro de 24 h → liquidez = **7/10 = 0,70 (70%)**.

**Lectura:**

| Liquidez | Lectura de negocio |
|----------|--------------------|
| Alta (ej. ≥ 70%) | La mayoría de clientes consigue técnico en ≤ T |
| Media | Hay match, pero lento o desigual por oficio/zona |
| Baja | Problema huevo–gallina: demanda sin oferta (o al revés, si hay pocos pedidos) |

### Variantes recomendadas (misma data)

1. **Liquidez por oficio** (`category`): plomería vs electricidad pueden divergir.
2. **Misma fórmula con T = 2 h, 6 h y 24 h** (ver qué tan “rápido” es el match).
3. **Cruzar con el ratio cliente/técnico:** liquidez baja + ratio alto → faltan técnicos; liquidez baja + ratio bajo → falta demanda o mala distribución.

### Supuestos MVP (documentar con el equipo)

- Sin geolocalización avanzada: no segmentamos por distancia; opcional filtrar por `city` / `zone` si el modelo lo tiene.
- Sin chat/pagos: “match” = **aceptación del técnico**, no trabajo finalizado ni pago.
- T = 24 h es hipótesis de Semana 1; se puede ajustar con PM cuando haya datos.

---

## 3. Data Plan — eventos y campos (contrato para Backend)

Objetivo: que Front/Back **persistan** lo mínimo para calcular los 3 KPIs + liquidez sin rediseñar el modelo en Semana 6–8.

### 3.1 Eventos / acciones a registrar

Cada fila es algo que debe quedar reflejado en BD (tabla de entidades o log de eventos). Preferencia: **estado en la entidad** + timestamps; un event log es Should.

| Evento / acción | Quién lo dispara | Debe quedar en BD | Obligatoriedad |
|-----------------|------------------|-------------------|----------------|
| `user.registered` | Cliente / Técnico / Admin al registrarse | Usuario + `role` + `created_at` | **Must** |
| `user.login` | Cualquier rol | Último login o sesión (para “activo”) | Should |
| `service_request.created` | Cliente | Solicitud + `client_id`, `category`, timestamps, estado `pending`/`open` | **Must** |
| `service_request.accepted` | Técnico | `technician_id`, `accepted_at`, estado `accepted` | **Must** |
| `service_request.rejected` | Técnico | `rejected_at` o registro de rechazo + `technician_id` | **Must** |
| `service_request.cancelled` | Cliente (o Admin) | `cancelled_at`, `cancelled_by`, estado `cancelled` | **Must** |
| `technician.availability_updated` | Técnico | Flag `is_available` | Should |

Estados mínimos sugeridos de una solicitud: `pending` → `accepted` | `cancelled` (y opcional `rejected` a nivel de oferta si varios técnicos pueden verla).

Si el modelo es “una solicitud, un técnico asignado”, el rechazo puede ser del técnico antes de aceptar o la solicitud vuelve a `pending`. Lo importante para Data: **no perder** `created_at` ni el primer `accepted_at`.

### 3.2 Matriz de campos (Must / Should)

| Campo | Entidad | Tipo | Obligatoriedad | Ejemplo | KPI / uso |
|-------|---------|------|----------------|---------|-----------|
| `user_id` | User | UUID/PK | Must | `u_01` | Ratio, joins |
| `role` | User | enum | Must | `client` \| `technician` \| `admin` | Ratio, RBAC analytics |
| `user.created_at` | User | timestamptz | Must | ISO-8601 | Cohortes |
| `last_login_at` | User | timestamptz | Should | ISO-8601 | Activos laxos |
| `is_available` | User (técnico) | boolean | Should | `true` | Oferta disponible |
| `request_id` | ServiceRequest | UUID/PK | Must | `r_01` | Todos los KPIs |
| `client_id` | ServiceRequest | FK → User | Must | `u_01` | Demanda |
| `technician_id` | ServiceRequest | FK → User nullable | Must* | `u_99` | Match (*null hasta aceptar) |
| `category` | ServiceRequest | string/enum | Must | `plumbing` | Liquidez por oficio |
| `status` | ServiceRequest | enum | Must | `pending` / `accepted` / `cancelled` | Tasa aceptación |
| `created_at` | ServiceRequest | timestamptz | Must | | TTA, liquidez |
| `accepted_at` | ServiceRequest | timestamptz nullable | Must | | TTA, liquidez |
| `cancelled_at` | ServiceRequest | timestamptz nullable | Must | | Cancelaciones |
| `cancelled_by` | ServiceRequest | enum nullable | Should | `client` / `admin` | Auditoría |
| `city` o `zone` | ServiceRequest | string nullable | Should | `CABA` | Segmentar liquidez |
| `rejection` log | Offer/Response | fila o JSON | Should | técnico + timestamp | Tasa rechazo |

\* `technician_id` puede ser null mientras `status = pending`; debe poblars e al aceptar.

### 3.3 Requisitos explícitos para Backend (Must)

Sin estos campos **no se pueden calcular** los KPIs de Semana 1:

1. `ServiceRequest.created_at` y `ServiceRequest.accepted_at` (timestamptz confiables, preferir UTC).
2. `ServiceRequest.status` con al menos `pending` | `accepted` | `cancelled`.
3. `User.role` discriminando `client` vs `technician`.
4. Vínculo `client_id` (y `technician_id` al aceptar).
5. `category` (oficio) en la solicitud.

**Tip Pro:** si no se persiste `accepted_at` desde el primer merge del CRUD, en Semana 6–8 no habrá TTA ni liquidez histórica.

### 3.4 Qué NO pedir esta semana

- Geolocalización en tiempo real / distancia km
- Eventos de chat o pagos
- Pipeline de BI / dashboard final
- Datasets externos tipo Kaggle (no aplican al brief UrbanFix Semana 1)

---

## 4. Consultas de referencia (orientativas)

Para que Backend valide que el modelo alcanza:

```sql
-- KPI 1: tasa de aceptación (semana)
SELECT
  COUNT(*) FILTER (WHERE status = 'accepted')::float
  / NULLIF(COUNT(*), 0) AS acceptance_rate
FROM service_requests
WHERE created_at >= date_trunc('week', now());

-- KPI 2: TTA en horas
SELECT AVG(EXTRACT(EPOCH FROM (accepted_at - created_at)) / 3600.0) AS tta_hours
FROM service_requests
WHERE accepted_at IS NOT NULL
  AND created_at >= date_trunc('week', now());

-- KPI 3: ratio clientes/técnicos activos
WITH active_clients AS (
  SELECT DISTINCT client_id FROM service_requests
  WHERE created_at >= now() - interval '7 days'
),
active_techs AS (
  SELECT DISTINCT technician_id FROM service_requests
  WHERE accepted_at >= now() - interval '7 days'
    AND technician_id IS NOT NULL
)
SELECT
  (SELECT COUNT(*) FROM active_clients)::float
  / NULLIF((SELECT COUNT(*) FROM active_techs), 0) AS client_tech_ratio;

-- Liquidez (T = 24h)
SELECT
  AVG(
    CASE
      WHEN accepted_at IS NOT NULL
       AND accepted_at - created_at <= interval '24 hours'
      THEN 1.0 ELSE 0.0
    END
  ) AS liquidity_24h
FROM service_requests
WHERE created_at >= date_trunc('week', now());
```

(Ajustar nombres de tablas/columnas al schema real.)

---

## 5. Checklist de alineación con el equipo

- [ ] Backend confirma Must de la matriz (§3.2–3.3) en el modelo de datos
- [ ] Frontend captura `category` y dispara create / accept / cancel / reject
- [ ] PM acuerda ventana T de liquidez (propuesta: 24 h)
- [ ] QA incluye casos: accept + cancel concurrente; request sin respuesta > T
- [ ] Data publica este doc en `product_y_growth/data/` y avisa en el canal del equipo

---

## 6. Resumen ejecutivo (1 párrafo)

UrbanFix mide salud de Marketplace con **tres KPIs**: tasa de solicitudes aceptadas, tiempo medio de aceptación y ratio cliente/técnico activos. La **liquidez** es el % de solicitudes creadas que obtienen aceptación en ≤ 24 h (ventana T acordada). Para calcularlo, Backend debe persistir desde el día uno timestamps (`created_at`, `accepted_at`), estados, roles, vínculo cliente–técnico y categoría de oficio; sin eso no hay telemetría de match ni dashboard de Admin en Semanas posteriores.
