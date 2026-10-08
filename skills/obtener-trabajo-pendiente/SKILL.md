---
name: obtener-trabajo-pendiente
description: "Obtiene la lista de tareas y proyectos que el estudiante tiene pendientes por entregar."
metadata: { "openclaw": { "emoji": "⏳" } }
---

# Obtener trabajo pendiente

Usar cuando el usuario pregunte qué tareas tiene pendientes, qué le falta por entregar o en qué debe trabajar a continuación.

Necesita el token guardado en el almacén de secretos (`FOURGEEKS_TOKEN`) y la dirección base de la API.

## Procedimiento

1. Pedir `GET /v1/assignment/user/me/task?task_status=PENDING` con header `Authorization: Token <token>`.
2. Comprobar que la respuesta sea exitosa (HTTP 200).
3. Extraer y listar las tareas filtradas devolviendo el título (`title`), el tipo de tarea (`task_type`) y la fecha límite o estado.
4. Devolver el listado claro de elementos pendientes.

## Salida esperada

Una lista organizada con los nombres y tipos de las tareas/proyectos cuyo estado sea PENDING.

## Notes

- Puedes usar los parámetros `task_status=PENDING` para filtrar solo el trabajo inconcluso.
- Si no hay nada pendiente, responder con un mensaje confirmando que todo está al día.
- Si la API no responde, indicar en qué paso se cortó.