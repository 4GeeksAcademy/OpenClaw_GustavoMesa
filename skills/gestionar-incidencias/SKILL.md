---
name: gestionar-incidencias
description: "Evalúa, clasifica y centraliza incidencias críticas del workspace del agente usando las reglas definidas en .cursorrules."
metadata: { "openclaw": { "emoji": "🚨" } }
status: active
enabled: true
---

# Gestionar incidencias

Usar cuando se detecte una incidencia crítica en el workspace (skill obsoleta, dependencia caída, violación de reglas, archivo huérfano o inconsistencia de configuración).

Requiere acceso al repositorio local y conocimiento de las reglas definidas en `.cursorrules` y `memory-bank/README.md`.

## Procedimiento

0. **Healthcheck del entorno:**
   - Verificar que el directorio `.git` existe y el repositorio está accesible.
   - Verificar que el archivo `.cursorrules` existe y tiene al menos 100 líneas.
   - Verificar que `memory-bank/README.md` existe y contiene las secciones de estado y cumplimiento.
   - Si alguna verificación falla, informar al usuario que el entorno no está listo y abortar.

1. Clasificar la incidencia según su tipo:
   a) **Skill obsoleta** — skill con `status: obsolete` o `[OBSOLETA]` en su SKILL.md.
   b) **Integración caída** — servicio externo que no responde (MCP, API, Telegram).
   c) **Violación de regla** — archivo o skill que incumple las reglas del `.cursorrules`.
   d) **Archivo huérfano** — archivo en la raíz del workspace que no es de configuración.

2. Para cada incidencia detectada, registrar en `docs/incidencias/` un archivo `YYYY-MM-DD--<tipo>.md` con:
   - Fecha y hora de detección.
   - Descripción del hallazgo.
   - Regla de `.cursorrules` que aplica (sección y nombre).
   - Nivel de criticidad (🔴 Alta / 🟡 Media / 🔵 Baja).
   - Acción correctiva aplicada o propuesta.
   - Estado (pendiente / en-progreso / resuelta).

3. Si la incidencia es de tipo **skill obsoleta**:
   - Verificar el metadato `status` en el frontmatter del SKILL.md.
   - Si `status: obsolete` o `enabled: false`, no ejecutar la skill e informar al usuario del reemplazo definido en `replaced_by`.

4. Si la incidencia es de tipo **integración caída**:
   - Verificar conectividad con el servicio (curl, ping o healthcheck MCP).
   - Si el servicio no responde, informar al usuario con el código de error exacto y sugerir acciones.

5. Cerrar la incidencia en el archivo correspondiente cuando se aplique la corrección y se verifique su efectividad.

## Salida esperada

Reporte estructurado con: tipo de incidencia, fecha, regla violada, criticidad, acción tomada y estado final.

## Notes

- No crear incidencias duplicadas para el mismo hallazgo.
- Las incidencias resueltas se archivan en `docs/incidencias/` con estado `resuelta`.
- Si el directorio `docs/incidencias/` no existe, crearlo automáticamente.
- Esta skill cumple con la regla 3.1 (frontmatter completo), regla 1.2 (status: active), regla 4.5 (healthcheck como paso 0) y regla 1.4 (no edita DREAMS.md).