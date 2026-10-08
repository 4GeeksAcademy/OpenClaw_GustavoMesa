---
name: obtener-proyectos
description: "Obtiene la lista de todos los proyectos asignados en 4Geeks junto con su estado actual."
metadata: { "openclaw": { "emoji": "📁" } }
---

# Obtener mis proyectos

Usar cuando el usuario pregunte qué proyectos tiene asignados, cuáles son sus proyectos del bootcamp o pida el listado de sus entregas de tipo proyecto.

Necesita el token guardado en el almacén de secretos (`FOURGEEKS_TOKEN`) y la dirección base de la API.

## Procedimiento

1. Pedir `GET /v1/assignment/user/me/task?task_type=PROJECT` con header `Authorization: Token ***}`.
2. Comprobar que la respuesta sea exitosa (HTTP 200).
3. Extraer la lista de proyectos indicando el título (`title`), el estado de la tarea (`task_status`) y el estado de revisión (`revision_status`).
4. Devolver la lista formateada de proyectos asignados.

## Salida esperada

Una lista con el nombre de cada proyecto asignado y su estado actual (por ejemplo: PENDING, DONE, APPROVED).

## Notes

- Filtrar específicamente por `task_type=PROJECT` para no traer ejercicios o lecciones individuales.
- Si la lista viene vacía, informar que no hay proyectos asignados en el cohorte actual.
- Si la API no responde, indicar en qué paso se cortó.