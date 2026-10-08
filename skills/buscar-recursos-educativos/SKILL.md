---
name: buscar-recursos-educativos
description: "Busca lecciones, ejercicios o proyectos en el catálogo de recursos de 4Geeks por tipo y tecnología."
metadata: { "openclaw": { "emoji": "📚" } }
---

# Buscar recursos educativos

Usar cuando el usuario quiera buscar materiales de estudio, ejercicios prácticos o proyectos de una tecnología específica en el catálogo.

Necesita la dirección base de la API.

## Procedimiento

1. Pedir `GET /v1/registry/asset?asset_type=EXERCISE&technologies=python`.
2. Comprobar que la respuesta sea exitosa (HTTP 200).
3. Extraer la lista de assets devueltos con sus títulos y slugs.
4. Devolver una selección clara de los recursos encontrados.

## Salida esperada

Una lista con los nombres, tipos y tecnologías de los recursos educativos disponibles en el registro.

## Notes

- Puedes combinar filtros como `asset_type` (LESSON, EXERCISE, PROJECT) y `technologies`.
- Si la API no responde, indicar en qué paso se cortó.