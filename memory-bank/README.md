# 🐢 Ogway — Banco de Memoria del Proyecto

## Objetivo del Producto y Contexto de Negocio

**Workspace de agente conversacional OpenClaw** para la gestión del progreso académico de Gustavo Mesa en **4Geeks Academy**. El agente **Ogway** funciona como asistente personal integrado con Telegram, Google Drive/Calendar y la API educativa de 4Geeks (BreatheCode).

Este repositorio contiene la configuración completa del agente: personalidad, reglas de comportamiento, skills de integración y memoria persistente. No es una aplicación backoffice tradicional — es un monorepo Markdown-driven que orquesta un agente de IA.

### Evolución hacia Centralizado de Incidencias (rama `feature/incident-manager`)
A partir del 17 de octubre de 2026 se inició el proyecto **"Backoffice de Operaciones – Centralizado de Incidencias"** con el objetivo de auditar, formalizar y robustecer las reglas de comportamiento del agente. Se ejecutan en la rama `feature/incident-manager` los siguientes pasos:

| Paso | Estado | Descripción |
|---|---|---|
| Paso 1 | ✅ Completado | Auditoría técnica del código base: convenciones, patrones y hallazgos |
| Paso 2 | ✅ Completado | Actualización de `.cursorrules` con 4 nuevas reglas imperativas |
| Paso 3 | ✅ Completado | Creación/actualización del Banco de Memoria (`memory-bank/`) |
| Paso 4 | ✅ Completado | Implementación del flujo de gestión y centralizado de incidencias |

---

## Stack Tecnológico y Dependencias Clave

| Componente | Tecnología / Versión | Propósito |
|---|---|---|
| Plataforma agente | **OpenClaw** v2026.9.7 | Orquestador de agente conversacional |
| Entorno de ejecución | **Node.js** v24.21.0 (Linux) | Runtime del Gateway |
| Host | VPS `bc-vps-412` (Debian/Ubuntu) | Servidor dedicado |
| Canal comunicación | **Telegram Bot API** (polling) | Interfaz con el usuario |
| API educativa | **BreatheCode REST API** (`breathecode.herokuapp.com`) | Datos académicos de 4Geeks |
| Integración Google | **Zapier MCP** (Drive + Calendar) | Documentos y eventos |
| Almacén secreto | **OpenClaw Secrets Store** (SQLite cifrado) | Tokens y credenciales |
| Control de versiones | **Git + GitHub** (`4GeeksAcademy/OpenClaw_GustavoMesa`) | Código fuente |

### Autenticación API
- Formato: `Authorization: Token <token>` (no Bearer)
- Endpoints de admissions requieren header adicional `Academy: 6`
- Token almacenado como `FOURGEEKS_TOKEN` en Secrets Store

---

## Estado Actual del Proyecto

### 🔖 Ramas
- `main` — Rama estable con configuraciones y skills operativas
- `feature/incident-manager` — Rama activa de trabajo (Paso 3 en curso)

### 📁 Estructura del Workspace
```
OpenClaw_GustavoMesa/
├── AGENTS.md              ← Comportamiento y herramientas del agente
├── SOUL.md                ← Personalidad y tono (tortuga tranquila 🐢)
├── USER.md                ← Perfil y directivas de Gustavo
├── IDENTITY.md            ← Identidad del agente (Ogway)
├── DREAMS.md              ← Consolidación automática de memoria (NO EDITAR)
├── SKILL_LOG.md           ← Bitácora de integraciones técnicas
├── .cursorrules           ← 24 reglas de comportamiento (4 áreas)
├── 4geeks.txt             ← Archivo temporal (vacío)
├── memory/                ← Memoria diaria + sueños auto-generados
│   ├── YYYY-MM-DD.md      ← Registros diarios
│   ├── .dreams/           ← Correlaciones de sesiones (auto-generado)
│   └── dreaming/          ← Sueños procesados (deep, light, rem)
├── memory-bank/           ← Banco de memoria del proyecto (este directorio)
├── docs/                  ← Documentación técnica de auditorías
│   ├── auditoria-paso1.md ← Reporte de reconocimiento del código base
│   └── incidencias/       ← Registro centralizado de incidencias
│       └── 2026-10-17--skill-obsoleta-sin-filtro.md
└── skills/                ← Biblioteca de skills del agente
    ├── autenticar-token/  🔑  ✅ Token 4Geeks válido (HTTP 200)
    ├── obtener-proyectos/ 📁  ✅ 39 proyectos (HTTP 200)
    ├── obtener-trabajo-pendiente/ ⏳  ✅ 34 tareas pendientes (HTTP 200)
    ├── resumen-de-progreso/ 📊  ✅ 161 tareas, 79% completado (HTTP 200)
    ├── listar-cohortes/   📋  ✅ 4 cohortes (HTTP 200, requiere Academy: 6)
    ├── buscar-recursos-educativos/ 📚  ✅ Catálogo abierto (HTTP 200, sin auth)
    ├── nota-en-drive/     📝  ✅ Google Docs vía Zapier MCP
    ├── gestionar-incidencias/ 🚨  ✅ Gestión centralizada de incidencias
    └── consultar-actividad/ ⛔  [OBSOLETA] endpoint 401
```

### ✅ Skills Operativas (7)

| Skill | Endpoint | Auth | Última prueba | Estado |
|---|---|---|---|---|
| `autenticar-token` | `GET /v1/admissions/user/me` | Token | 8 oct 2026 | ✅ HTTP 200 |
| `obtener-proyectos` | `GET /.../task?task_type=PROJECT` | Token | 8 oct 2026 | ✅ HTTP 200 |
| `obtener-trabajo-pendiente` | `GET /.../task?task_status=PENDING` | Token | 8 oct 2026 | ✅ HTTP 200 |
| `resumen-de-progreso` | `GET /.../task` (sin filtro) | Token | 8 oct 2026 | ✅ HTTP 200 |
| `listar-cohortes` | `GET /v1/admissions/academy/cohort/me` | Token + `Academy: 6` | 8 oct 2026 | ✅ HTTP 200 |
| `buscar-recursos-educativos` | `GET /v1/registry/asset` | Sin auth | 8 oct 2026 | ✅ HTTP 200 |
| `nota-en-drive` | Zapier MCP (Drive) | SecretRef | Creada | ✅ Operativa |
| `gestionar-incidencias` | Infraestructura local | — | 17 oct 2026 | ✅ Operativa |

### ⛔ Skills Obsoletas (1)

| Skill | Endpoint | Motivo | Reemplazo | Estado |
|---|---|---|---|---|
| `consultar-actividad` | `GET /v1/activity/me` | HTTP 401 (sin acceso) | `listar-cohortes` | ⛔ `status: obsolete` |

**Riesgo detectado en auditoría (Paso 1):** El sistema de skills de OpenClaw no filtra por metadato `status: obsolete`. Una invocación directa a `consultar-actividad` aún se ejecutaría y fallaría. Se añadió regla en `.cursorrules` para mitigar esto (ver sección de cumplimiento abajo).

---

## Correcciones Aplicadas (Fase 4, 17 oct 2026)

| # | Corrección | Archivo | Detalle |
|---|---|---|---|
| 1 | Typo en header Authorization | `autenticar-token/SKILL.md` | `***}` → `<token>` |
| 2 | Typo en header Authorization | `obtener-proyectos/SKILL.md` | `***}` → `<token>` |
| 3 | Emoji faltante + sección Notes + tono colombiano | `nota-en-drive/SKILL.md` | Añadido 📝, Notes, imperativos corregidos |
| 4 | Marcada como obsoleta | `consultar-actividad/SKILL.md` | `status: obsolete` + `replaced_by: listar-cohortes` |
| 5 | Registro de skill y mantenimiento | `SKILL_LOG.md` | Añadida Skill 7 (nota-en-drive) y sección 4 (Mantenimiento) |
| 6 | Archivo huérfano eliminado y restaurado | `4geeks.txt` | Eliminado del control de versiones, luego restaurado por solicitud |

---

## Análisis de Cumplimiento: Nuevas Reglas del `.cursorrules`

### Regla 1 — Estado de Skills (Sección 1.2)
**Enunciado:** "Prohibido ejecutar skills marcadas como `[OBSOLETA]` o con `status: obsolete` / `enabled: false`."

**Cumplimiento actual:**
- `consultar-actividad` tiene `status: obsolete` en su frontmatter. ✅
- Las 7 skills operativas NO tienen `status: obsolete`. ✅
- **Brecha:** OpenClaw no tiene un filtro nativo de `status`. La regla depende de que el agente la cumpla manualmente. ⚠️

**Acción recomendada:** Agregar verificación programática en el flujo de skills del agente para que rechace automáticamente skills con `status: obsolete` antes de ejecutar.

### Regla 2 — Healthcheck MCP (Sección 4.5)
**Enunciado:** "Toda skill que dependa de integraciones externas debe incluir un paso 0 de verificación de conectividad."

**Cumplimiento actual:**
- `nota-en-drive` tiene la dependencia documentada en `Notes` pero **no tiene paso 0 de healthcheck** en el procedimiento. ❌
- Las skills de BreatheCode API dependen del token pero no verifican conectividad antes de la petición. ❌

**Acción recomendada:** Modificar los SKILL.md de skills con dependencias externas para incluir un paso 0 de healthcheck.

### Regla 3 — Esquema Estricto Frontmatter (Sección 3.1)
**Enunciado:** "Todo SKILL.md debe incluir obligatoriamente `name`, `description` y `emoji`."

**Cumplimiento actual:**
- 7 de 8 skills tienen `name`, `description` y `emoji`. ✅
- `nota-en-drive` fue corregido para incluir `emoji` (📝). ✅
- **Saldo:** 8/8 skills cumplen. ✅

### Regla 4 — Exclusión de DREAMS.md (Sección 1.4)
**Enunciado:** "No editar manualmente archivos de consolidación automática."

**Cumplimiento actual:**
- `DREAMS.md` fue modificado en el commit `b3e616c` (antes de la regla). ⚠️
- No hay ediciones posteriores a la regla. ✅
- Se recomienda agregar `DREAMS.md` a `.gitignore` si el sistema lo regenera automáticamente.

---

## Pendientes Prioritarios

| # | Pendiente | Prioridad | Estado |
|---|---|---|---|
| 1 | Verificar cita duplicada de peluquería en Google Calendar | 🟡 Media | Pendiente de cancelación |
| 2 | Recordatorio padrón: alarma 2026-10-19 11:10 Madrid | 🟡 Media | Verificar si necesita "llevar impreso" |
| 3 | Agregar healthcheck (paso 0) a `nota-en-drive` y skills BreatheCode | 🟡 Media | Pendiente de implementación en SKILL.md |
| 4 | Evaluar filtro programático de `status: obsolete` en flujo de skills | 🟡 Media | Mitigado por skill `gestionar-incidencias` |
| 5 | Evaluar agregar `DREAMS.md` a `.gitignore` | 🔵 Baja | Decisión de arquitectura |

---

## Convenciones del Proyecto

- **Zona horaria:** Europe/Madrid (CET/CEST). Servidor en UTC.
- **Tono:** Colombiano, amigable y técnico.
- **Respuestas:** Máximo 5 líneas salvo que pida detalles.
- **Silencio en heartbeats:** Sin novedades = cero mensajes visibles.
- **Nomenclatura:** directorios kebab-case, SKILL.md constante, memoria `YYYY-MM-DD.md`.
- **Git:** Commits en inglés con prefijo convencional (`feat:`, `fix:`, `docs:`, `chore:`, `test:`).
- **Documentos en Drive:** Prefijo `AIE-DD-MM-YYYY — título`.
- **Autenticación API:** `Authorization: Token <token>` (no Bearer). Endpoints de admissions requieren `Academy: 6`.

---

*Última actualización: 17 de octubre de 2026 — Rama `feature/incident-manager` (Paso 3)*