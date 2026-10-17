# Incidencia: consultar-actividad obsoleta sin filtro programático

**Fecha:** 17 de octubre de 2026
**Detectada por:** Auditoría Paso 1
**Tipo:** Skill obsoleta
**Regla aplicable:** `.cursorrules` sección 1.2 — Estado de skill
**Criticidad:** 🔴 Alta

## Descripción
La skill `consultar-actividad` está marcada como `status: obsolete` pero el sistema de skills de OpenClaw no tiene un filtro nativo que impida su ejecución. Si el agente recibe una solicitud que active esta skill, intentará ejecutar el endpoint `GET /v1/activity/me` y fallará con HTTP 401.

## Acción correctiva aplicada
1. ✅ Marcada como `[OBSOLETA]` en frontmatter de SKILL.md
2. ✅ Agregado `status: obsolete` y `replaced_by: listar-cohortes`
3. ✅ Regla imperativa agregada en `.cursorrules` sección 1.2
4. ✅ Skill `gestionar-incidencias` creada con healthcheck (paso 0) que filtra skills obsoletas antes de ejecutar

## Estado
**Resuelta** — mitigada por reglas y skill de gestión, pendiente de filtro programático nativo en plataforma OpenClaw.