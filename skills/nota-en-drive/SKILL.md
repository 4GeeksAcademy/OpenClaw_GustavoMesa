---
name: nota-en-drive
description: Guarda una nota en Google Drive como documento editable.
metadata: { "openclaw": { "emoji": "📝" } }
---

# Skill: nota en Drive

## Cuándo usar
Cuando pidan guardar una nota, una idea o un resumen en Drive.

## Prerrequisitos
- El servidor MCP de Zapier conectado.
- Las convenciones de documentos de TOOLS.md.

## Procedimiento
1. Si no dieron el texto, preguntar qué guardar.
2. Armar el título con la fecha de hoy y un resumen corto del contenido, siguiendo el formato `AIE-DD-MM-YYYY — título`.
3. Crear el documento con `google_drive_create_file_from_text`, con `convert: true`.
4. Devolver el enlace del documento creado.

## Salida esperada
Existe un documento en Drive cuyo nombre empieza con AIE- y la fecha de hoy, y el agente devolvió su enlace.

## Notes
- Si el texto está vacío, no crear nada e informar al usuario.
- El servidor MCP de Zapier debe estar operativo antes de ejecutar la skill.
- El documento se crea en la carpeta raíz de Drive. Para moverlo a una carpeta específica, se requiere acción manual adicional.
