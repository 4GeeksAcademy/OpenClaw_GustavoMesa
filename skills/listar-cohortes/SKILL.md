---
name: listar-cohortes
description: "Lista los cohortes del estudiante en 4Geeks con su información detallada."
metadata: { "openclaw": { "emoji": "📋" } }
---

# Listar cohortes

Usar cuando el usuario pregunte por sus cohortes activos, en qué grupo está o quiera ver información de su bootcamp.

Necesita el token guardado en el almacén de secretos (`FOURGEEKS_TOKEN`) y la dirección base de la API.

## Procedimiento

1. Pedir `GET /v1/admissions/academy/cohort/me` con header `Authorization: Token <token>` y header `Academy: 6` (ID de 4Geeks Madrid).
2. Comprobar que la respuesta sea exitosa (HTTP 200).
3. Extraer los cohortes con nombre, academia, fechas, etapa y sílabo.
4. Devolver la lista de cohortes.

## Salida esperada

Una lista con los cohortes del estudiante, incluyendo nombre, academia, fechas, etapa y sílabo.

## Notes

- El header `Academy` requiere el ID numérico de la academia (ej: `6` para 4Geeks Madrid).
- Si no hay cohortes, informar al usuario.
- Si la API no responde, indicar en qué paso se cortó.