---
name: consultar-actividad
description: "Muestra el registro de actividad de aprendizaje del estudiante en la plataforma BreatheCode."
metadata: { "openclaw": { "emoji": "📈" } }
---

# Consultar actividad

Usar cuando el usuario pregunte por su registro de actividad, tiempo de estudio o acciones recientes registradas en la plataforma.

Necesita el token guardado en el almacén de secretos (`FOURGEEKS_TOKEN`) y la dirección base de la API.

## Procedimiento

1. Pedir `GET /v1/activity/me` con header `Authorization: Token <token>`.
2. Comprobar que la respuesta sea exitosa (HTTP 200).
3. Extraer el registro de tiempo y eventos de aprendizaje recientes del estudiante.
4. Devolver un resumen estructurado con las actividades registradas.

## Salida esperada

Un desglose claro de las actividades recientes del usuario (por ejemplo: ejercicios resueltos, tiempo dedicado o eventos de estudio).

## Notes

- Si no hay registros recientes, informar al usuario.
- Si la API no responde, indicar en qué paso se cortó.