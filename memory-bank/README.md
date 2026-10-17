# 🐢 Ogway — Resumen del Proyecto

## Objetivo del Producto

Agente conversacional **Ogway** desplegado en **OpenClaw v2026.9.7** sobre el VPS `bc-vps-412`. Actúa como asistente personal de Gustavo Mesa, estudiante de **4Geeks Academy**, integrando:

- **Telegram** como canal de comunicación principal.
- **API de BreatheCode (4Geeks)** para consultar proyectos, tareas, cohortes y progreso académico.
- **Google Drive / Calendar** vía Zapier MCP para crear documentos y eventos.

## Stack Tecnológico

| Componente | Tecnología |
|---|---|
| Plataforma agente | OpenClaw v2026.9.7 (Node.js v24.21.0) |
| Host | Linux, VPS bc-vps-412 |
| Canal comunicación | Telegram Bot API (polling, token cifrado) |
| API educativa | BreatheCode REST API (`breathecode.herokuapp.com`) |
| Integración Google | Zapier MCP (Drive + Calendar) |
| Almacén secreto | OpenClaw Secrets Store (SQLite local cifrado) |
| Control de versiones | Git → GitHub (`4GeeksAcademy/OpenClaw_GustavoMesa`) |

## Arquitectura del Workspace

```
OpenClaw_GustavoMesa/
├── AGENTS.md              ← Comportamiento y herramientas del agente
├── SOUL.md                ← Personalidad y tono (tortuga tranquila)
├── USER.md                ← Perfil y directivas de Gustavo
├── IDENTITY.md            ← Identidad del agente (Ogway 🐢)
├── DREAMS.md              ← Consolidación automática de memoria
├── SKILL_LOG.md           ← Bitácora de integraciones técnicas
├── .cursorrules           ← Reglas formales de comportamiento (20 reglas)
├── skills/                ← 7 skills funcionales + 1 obsoleta
│   ├── autenticar-token/  ✅ Token 4Geeks válido
│   ├── obtener-proyectos/ ✅ 39 proyectos consultados
│   ├── obtener-trabajo-pendiente/ ✅ 34 tareas pendientes
│   ├── resumen-de-progreso/ ✅ 79% completado (161 tareas)
│   ├── listar-cohortes/   ✅ 4 cohortes activos
│   ├── buscar-recursos-educativos/ ✅ Catálogo abierto
│   ├── nota-en-drive/     ✅ Google Drive vía Zapier MCP
│   └── consultar-actividad/ ⛔ [OBSOLETA] endpoint 401
├── memory/                ← Memoria diaria + sueños auto-generados
└── memory-bank/           ← Resumen del proyecto (este archivo)
```

## Estado Actual

### ✅ Skills operativas (7)
| Skill | Endpoint | Estado | Notas |
|---|---|---|---|
| autenticar-token | `GET /v1/admissions/user/me` | ✅ HTTP 200 | Token válido, sesión activa |
| obtener-proyectos | `GET /.../task?task_type=PROJECT` | ✅ HTTP 200 | 39 proyectos listados |
| obtener-trabajo-pendiente | `GET /.../task?task_status=PENDING` | ✅ HTTP 200 | 34 tareas pendientes |
| resumen-de-progreso | `GET /.../task` (sin filtro) | ✅ HTTP 200 | 161 tareas, 79% completado |
| listar-cohortes | `GET /v1/admissions/academy/cohort/me` | ✅ HTTP 200 | Requiere header `Academy: 6` |
| buscar-recursos-educativos | `GET /v1/registry/asset` | ✅ HTTP 200 | Sin autenticación |
| nota-en-drive | Zapier MCP | ✅ Operativa | Google Doc con formato `AIE-DD-MM-YYYY` |

### ⛔ Skill obsoleta
- **consultar-actividad** — endpoint `GET /v1/activity/me` devuelve 401. Reemplazada por `listar-cohortes`.

### 📋 Reglas de comportamiento (Fase 2-3)
Se definieron **20 reglas** en `.cursorrules` organizadas en 4 áreas:
1. **Estructura de Directorios** — skills en kebab-case, memoria diaria con fecha
2. **Tecnologías y Entorno** — OpenClaw, Telegram, BreatheCode, Zapier, secretos cifrados
3. **Convenciones de Código** — SKILL.md con frontmatter YAML, directivas imperativas, commits convencionales
4. **Flujos de Trabajo** — ciclo crear-probar-registrar, silencio en heartbeats, corrección de errores

### 🔧 Correcciones aplicadas (Fase 4, 17 oct 2026)
- `4geeks.txt` eliminado del repositorio (archivo huérfano)
- `autenticar-token`: typo `***}` corregido a `<token>`
- `obtener-proyectos`: typo `***}` corregido a `<token>`
- `nota-en-drive`: añadido emoji 📝, sección Notes, imperativos en tono colombiano
- `consultar-actividad`: marcada como `[OBSOLETA]` con `status: obsolete`
- `SKILL_LOG.md`: actualizado con Skill 7 y sección 4 de Mantenimiento

## Pendientes Prioritarios

1. **Verificar cita duplicada de peluquería** en Google Calendar (evento creado con fecha 09/10 en vez de 19/10). Pendiente de cancelación.
2. **Recordatorio padrón**: alarma programada para 2026-10-19 11:10 Madrid. Verificar si necesita mención explícita de "llevar impreso".
3. **API 4Geeks**: la Sección 1 de SKILL_LOG.md documenta el proceso de descubrimiento fallido (endpoints 404). Se conserva como registro histórico.

## Convenciones Clave

- **Zona horaria:** Europe/Madrid (CET/CEST). El servidor corre en UTC.
- **Tono:** Colombiano, amigable y técnico, sin cortesías superfluas.
- **Respuestas:** Máximo 5 líneas salvo que pida detalles.
- **Silencio en heartbeats:** Sin novedades = cero mensajes visibles.
- **Autenticación API:** `Authorization: Token <token>` (no Bearer). Endpoints de admissions requieren `Academy: 6`.
- **Git:** Commits en inglés con prefijo convencional (`feat:`, `fix:`, `test:`, `chore:`).