---
name: nota-en-drive
description: Guarda una nota en Google Drive como documento editable.
---

# Skill: nota en Drive

## Cuándo usar
Cuando te pida guardar una nota, una idea o un resumen en Drive.

## Prerrequisitos
- El servidor MCP de Zapier conectado.
- Las convenciones de documentos de TOOLS.md.

## Procedimiento
1. Si no te dieron el texto, preguntá qué guardar.
2. Armá el título con la fecha de hoy y un resumen corto del contenido.
3. Creá el documento con google_drive_create_file_from_text, con convert en verdadero.
4. Devolvé el enlace.

## Salida esperada
Existe un documento en Drive cuyo nombre empieza con AIE- y la fecha de hoy, y el agente devolvió su enlace.

## Casos especiales
- Si el texto está vacío, no crees nada.
