# Auditoría Técnica — OpenClaw_GustavoMesa
## Proyecto: Backoffice de Operaciones – Centralizado de Incidencias (Paso 1)

**Fecha:** 17 de octubre de 2026
**Rama:** `feature/incident-manager`
**Auditor:** Ogway 🐢

---

## 1. Resumen de Contexto

### Contexto de Negocio
Workspace de configuración para un agente conversacional (Ogway) integrado con Telegram, Google y la API educativa de 4Geeks Academy. El agente asiste a Gustavo Mesa en la gestión de su progreso académico, consulta de proyectos, tareas pendientes, cohortes activos y creación de documentos.

### Contexto Técnico
- **Plataforma:** OpenClaw v2026.9.7 (Node.js v24.21.0)
- **Host:** VPS bc-vps-412 (Linux)
- **Canal principal:** Telegram Bot API (polling, token cifrado en Secrets Store)
- **Integraciones externas:** BreatheCode REST API, Zapier MCP (Google Drive/Calendar)
- **Tipo de repositorio:** Monorepo Markdown-driven — no hay código compilado ni servidores web propios. Todo es configuración declarativa y documentación de integraciones.
- **Skills:** 8 skills en `skills/` (7 operativas, 1 obsoleta)
- **Reglas de comportamiento:** 20 reglas documentadas en `.cursorrules`

---

## 2. Convenciones No Escritas Detectadas

### a) Frontmatter YAML como interfaz de skill
Cada `SKILL.md` expone una interfaz mediante frontmatter YAML con `name`, `description` y `emoji`. Es la convención más importante del repositorio pero solo está documentada en `.cursorrules` sección 3.1. Cualquier skill nueva sin frontmatter quedaría invisible para el catálogo del agente.

**Evidencia:** Las 8 skills existentes tienen frontmatter, pero `nota-en-drive` carecía de `emoji` hasta la corrección de Fase 4.

### b) SKILL.md como triple responsabilidad (documentación + especificación + ejecución)
Cada skill combina tres roles en un solo archivo:
- Documentación de uso (`## Cuándo usar`)
- Especificación técnica del endpoint (método HTTP, ruta, headers)
- Procedimiento de ejecución (`## Procedimiento`)

No hay separación entre "contrato de API" y "guía de uso". Cualquier cambio de endpoint requiere editar el SKILL.md directamente.

**Evidencia:** Todos los SKILL.md tienen las secciones `## Procedimiento`, `## Salida esperada` y `## Notes`.

### c) `DREAMS.md` — archivo auto-generado pero versionado en git
`DREAMS.md` (22 KB) se describe como "consolidación automática de memoria a largo plazo" y la regla 1.4 de `.cursorrules` dice "No se edita manualmente". Sin embargo está trackeado en git y fue modificado en el commit `b3e616c`. Esto lo vuelve una zona gris: el sistema lo escribe automáticamente pero el versionado lo congela.

---

## 3. Reglas o Patrones Contradictorios

### a) `consultar-actividad` — skill viva en disco pero muerta en ejecución
- La skill existe como directorio `skills/consultar-actividad/` con `SKILL.md` completo.
- El endpoint `GET /v1/activity/me` devuelve HTTP 401.
- Se marcó como `[OBSOLETA]` con `status: obsolete` y `replaced_by: listar-cohortes`.
- **Problema:** El sistema de skills de OpenClaw no filtra por metadato `status: obsolete`. Cualquier invocación a esta skill la ejecutará igual y fallará en producción.
- **Riesgo:** Alto. Hay una skill elegible para ejecución que siempre fallará.

### b) `nota-en-drive` — dependencia MCP sin healthcheck previo
- La skill depende del servidor MCP de Zapier para crear documentos en Google Drive.
- El procedimiento no incluye verificación de conectividad antes de ejecutar.
- La sección `Notes` dice "El servidor MCP de Zapier debe estar operativo" pero no hay un paso `0` de healthcheck.
- **Riesgo:** Medio. Si Zapier está caído, la skill falla en el paso 3 sin diagnóstico claro para el usuario.

---

## 4. Recomendaciones para el Backoffice

| # | Aspecto | Riesgo | Recomendación |
|---|---|---|---|
| 1 | Skills sin filtro de `status` | 🔴 Alto | Agregar metadato `enabled: true/false` en frontmatter y que el agente lo respete antes de ejecutar |
| 2 | Dependencia MCP sin healthcheck | 🟡 Medio | Paso 0 en cada SKILL.md: verificar conectividad del servicio externo antes de ejecutar |
| 3 | Frontmatter sin schema estricto | 🟡 Medio | Definir un JSON Schema para validar `name`, `description`, `emoji`, `status` |
| 4 | DREAMS.md auto-editado en git | 🔵 Bajo | Agregar a `.gitignore` si es auto-generado, o mover a un directorio no versionado |

---

*Documento generado como parte del Paso 1 — Reconocimiento del código base.*