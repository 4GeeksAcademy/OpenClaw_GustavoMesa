---
name: resumen-de-progreso
description: "Muestra un resumen global del progreso del estudiante, incluyendo tareas completadas y pendientes."
metadata: { "openclaw": { "emoji": "📊" } }
---

# Resumen de progreso

Usar cuando el usuario pida un reporte general de su avance, su porcentaje de rendimiento o un resumen de sus tareas finalizadas vs. pendientes.

Necesita el token guardado en el almacén de secretos (`FOURGEEKS_TOKEN`) y la dirección base de la API.

## Procedimiento

1. Consultar las tareas del usuario (`GET /v1/assignment/user/me/task`).
2. Contabilizar el total de tareas, desglosando cuántas están completadas (`DONE`) y cuántas están pendientes (`PENDING`).
3. Calcular opcionalmente el porcentaje de avance estimado.
4. Presentar un informe visualmente claro y estructurado con el resumen.

## Salida esperada

Un dashboard o resumen con:
- Total de tareas/entregables.
- Tareas completadas (DONE).
- Tareas pendientes (PENDING).
- Estado general del curso/bootcamp.

## Notes

- Permite consolidar la información recibida en las skills anteriores en un solo panel de estado.
- Si la API no responde, indicar el error de conexión.