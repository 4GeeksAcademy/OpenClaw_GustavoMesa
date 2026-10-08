# SKILL_LOG.md - Registro de Integración OpenClaw & BreatheCode API

## 1. Conversación de Descubrimiento

### Skill: autenticar-token

**Fecha:** 7 de octubre de 2026

**Proceso:**

1. Gustavo pidió crear la skill `autenticar-token` con la estructura de SKILL.md proporcionada.
2. Se creó el archivo `skills/autenticar-token/SKILL.md` con la descripción, procedimiento y notas.
3. Se intentó guardar el token de 4Geeks (`FOURGEEKS_TOKEN`) de forma segura usando `openclaw secrets store set` — el token se guardó correctamente como secreto.
4. Se recargaron los secretos con `openclaw secrets reload`.
5. Se probó el endpoint `GET /v1/auth/subscribe/token_info` contra `https://breathecode.herokuapp.com` con el token autenticado vía `Authorization: Bearer ***`.
6. **Resultado:** HTTP 404 — el endpoint no se encuentra.
7. Se probaron múltiples variantes de ruta y dominio:
   - `/v1/auth/token-info`, `/v1/auth/token_info`, `/v1/auth/me`, `/v1/auth/user`
   - `/v1/mentor/me`, `/v1/mentor/assignments`, `/v1/mentor/projects`
   - `/auth/subscribe/token_info`, `/subscribe/token_info`
   - Con y sin trailing slash, token en query string, diferentes formatos de auth header
   - Dominios: `breathecode.herokuapp.com`, `api.4geeksacademy.com`, `auth.4geeksacademy.com`
8. **Todas las variantes devolvieron HTTP 404 o 301.**
9. **Pendiente:** Gustavo debe confirmar la URL base correcta o la ruta exacta del endpoint.

**Problemas detectados:**
- La API en `breathecode.herokuapp.com` responde pero ninguna ruta de auth parece existir.
- Es posible que el endpoint haya cambiado de dominio o que el token sea para otro entorno.

---

## 2. Registro de Skills Principales

### Skill 1: Autenticar token
- **Prompt:** "Verifica si el token de estudiante de 4Geeks almacenado es válido y la sesión está activa."
- **Descripción:** Consulta el endpoint de autenticación para validar que el token siga activo y devolver el usuario asociado.
- **Endpoint:** `GET /v1/admissions/user/me` con header `Authorization: Token <token>`
- **Estado de la prueba:** ✅ HTTP 200 — token válido para gustavoamesa17@gmail.com, cohorte activa spain-aie-devs-pt-1 en 4Geeks Madrid.
- **Archivo de skill:** `skills/autenticar-token/SKILL.md`

### Skill 2: Obtener proyectos
- **Prompt:** "Obtiene la lista de todos los proyectos asignados en 4Geeks junto con su estado actual."
- **Descripción:** Consulta los proyectos del usuario filtrando por task_type=PROJECT para listar títulos, estados y revisión.
- **Endpoint:** `GET /v1/assignment/user/me/task?task_type=PROJECT` con header `Authorization: Token <token>`
- **Resultado de la prueba (8 oct 2026):** ✅ HTTP 200 — se obtuvieron 39 proyectos. Ejemplos:
   - Build Your IT Resume — PENDING / PENDING
   - Create a HTML5 form — DONE / APPROVED
   - Setting Up Your Personal AI Agent with OpenClaw — DONE / APPROVED
   - My 4Geeks Assistant — PENDING / PENDING
- **Archivo de skill:** `skills/obtener-proyectos/SKILL.md`

### Skill 3: Obtener trabajo pendiente
- **Prompt:** "Obtiene la lista de tareas y proyectos que el estudiante tiene pendientes por entregar."
- **Descripción:** Consulta las tareas del usuario filtrando por task_status=PENDING para listar trabajo inconcluso.
- **Endpoint:** `GET /v1/assignment/user/me/task?task_status=PENDING` con header `Authorization: Token <token>`
- **Resultado de la prueba (8 oct 2026):** ✅ HTTP 200 — 34 tareas pendientes. Incluye:
   - 2 Proyectos: Build Your IT Resume, Choosing Your Vibe Coding Tools Wisely, etc.
   - 20+ Ejercicios: Connecting OpenClaw with Telegram, Managing Secrets, etc.
   - 4 Lecciones: First Steps for your resume, Interview Preparation, etc.
   - 2 Quizzes: AI Fundamentals, Prompt Engineering
   - Distribuidos en cohortes: Building Your Tech Profile, Full Stack with AI, Working with AI coding agents, etc.
- **Archivo de skill:** `skills/obtener-trabajo-pendiente/SKILL.md`

### Skill 4: Resumen de progreso
- **Prompt:** "Muestra un resumen global del progreso del estudiante, incluyendo tareas completadas y pendientes."
- **Descripción:** Consulta todas las tareas del usuario (sin filtro) y calcula porcentaje de avance, desglose por estado y tipo.
- **Endpoint:** `GET /v1/assignment/user/me/task` con header `Authorization: Token <token>`
- **Resultado de la prueba (8 oct 2026):** ✅ HTTP 200 — 161 tareas totales:
   - Completadas (DONE): 127 (79%)
   - Pendientes (PENDING): 34
   - Aprobadas (APPROVED): 36
   - Por tipo: 39 proyectos, 43 ejercicios, 72 lecciones, 7 quizzes
   - Distribuidas en 8 cohortes
- **Archivo de skill:** `skills/resumen-de-progreso/SKILL.md`

---

## 3. Registro de Skills Extendidas

### Skill 5: Listar cohortes
- **Prompt:** "Lista los cohortes del estudiante en 4Geeks con su información detallada."
- **Descripción:** Consulta los cohortes del estudiante usando el endpoint de admissions con header Academy.
- **Endpoint:** `GET /v1/admissions/academy/cohort/me` con `Authorization: Token <token>` y `Academy: 6` (4Geeks Madrid)
- **Resultado de la prueba (8 oct 2026):** ✅ HTTP 200 — 4 cohortes obtenidos:
   - spain-fs-pt-123 — Full-Stack Developer (ENDED)
   - Land a Job in Tech Spain — Land a Job in Tech (STARTED)
   - Building Your Tech Profile Spain — Building Your Tech Profile (STARTED)
   - Madrid Prework — Coding Introduction (STARTED)
- **Archivo de skill:** `skills/listar-cohortes/SKILL.md`

### Skill 6: Buscar recursos educativos
- **Prompt:** "Busca lecciones, ejercicios o proyectos en el catálogo de recursos de 4Geeks por tipo y tecnología."
- **Descripción:** Consulta el catálogo de assets educativos para buscar materiales de estudio por tipo y tecnología.
- **Endpoint:** `GET /v1/registry/asset?asset_type=EXERCISE&technologies=python` (sin token necesario)
- **Resultado de la prueba (8 oct 2026):** ✅ HTTP 200 — se obtuvieron recursos. Ejemplos: Exploring Random Forest, Exploring Boosting Algorithm, Explorando Random Forest, Learn how to build HTTP requests with Python. El endpoint no necesita autenticación.
- **Archivo de skill:** `skills/buscar-recursos-educativos/SKILL.md`