---
name: autenticar-token
description: "Verifica si el token de estudiante de 4Geeks almacenado es válido y la sesión está activa."
metadata: { "openclaw": { "emoji": "🔑" } }
---

# Autenticar token

Usar cuando pregunten si el token funciona, si la sesión está activa o si las credenciales de 4Geeks son válidas.

Necesita el token guardado en el almacén de secretos (`FOURGEEKS_TOKEN`) y la dirección base de la API.

## Procedimiento

1. Hacer `GET /v1/admissions/user/me` con el header `Authorization: Token {token}`.
2. Comprobar que la respuesta tenga un estado de respuesta exitoso (HTTP 200).
3. Extraer el estado del token y el correo del usuario asignado.
4. Devolver la confirmación de la validez de la sesión.

## Salida esperada

Mensaje de confirmación indicando si el token está activo, junto con el usuario asociado o el error detectado en caso de fallar.

## Notes

- Si la API responde con un error 401 o 403, avisar que el token expiró o es inválido.
- Si la API no responde, decir en qué paso se cortó.