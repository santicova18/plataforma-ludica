# AGENTS.md

Contexto del proyecto para los agentes de IA. Léalo antes de cualquier cambio.

## Proyecto

- Plataforma web lúdica de cursos para niños. Curso: Lectura. Mecánica: Misiones e insignias. Roles: docente y niño; el niño entra con código de clase y avatar, sin correo.
- Estado real: plantilla base con página de inicio, `/salud`, `/api/proyecto` y PWA; `app/modelos/` y `app/servicios/` están vacíos y el dominio no está implementado. No invente tablas, servicios ni endpoints que no existan.
- `app/proyecto.py` es la única fuente de `APRENDIZ`, `CURSO`, `MECANICA`, `URL_PUBLICA` y `VERSION`: la usan la página, la API y `entregar.py`. Cambie ahí, no en las plantillas.
- `specs/`, `STATE.md` y `POLITICA_IA.md` siguen siendo plantillas con `(complete)`. El contrato vigente es el código y `/docs`, no `specs/plan.md`.

## Stack

- Python >= 3.11 (el CI usa 3.13), FastAPI, Jinja2, SQLModel; SQLite en desarrollo y PostgreSQL al publicar.
- pytest + TestClient, Ruff, pre-commit, GitHub Actions y Render.

## Comandos

- Entorno existente en `venv/`: `.\venv\Scripts\python.exe -m ...` o `venv\Scripts\activate`. Instalar desde cero: `python -m venv venv` y `pip install -r requirements-dev.txt`.
- Ejecutar `uvicorn app.main:app --reload` (http://127.0.0.1:8000, API en `/docs`); pruebas `pytest -v` (una sola: `pytest tests/test_inicio.py::test_salud_responde_ok -v`).
- Estilo `ruff check .` (línea 100, reglas E, F, B, I; target py311). Antes de terminar ejecute `ruff check .` y luego `pytest -v` (igual que CI; `pre-commit install` corre ambos).
- `pyproject.toml` fija `testpaths = ["tests"]` y `pythonpath = ["."]`: ejecute desde la raíz.

## Estructura

- `app/main.py` monta `/static` e incluye los routers; `app/rutas/` recibe la petición y llama a los servicios, sin regla de negocio; `paginas.py` sirve HTML (fuera de `/docs`) y `api.py` la API documentada.
- `app/servicios/`: regla de negocio con pruebas propias; `app/modelos/`: tablas SQLModel; `app/templates/` y `app/static/`: interfaz y PWA.
- `tests/`: pruebas; `tests/aceptacion/`: criterios Dado/Cuando/Entonces de `specs/spec.md` en `test_<historia>.py`.
- `specs/`: especificación, plan, tareas, flujo y ADR en `specs/adr/`; `entregas/`: respuestas semanales.

## PWA

- El service worker se sirve en `/sw.js` desde una ruta de `paginas.py`, no desde `/static`, para controlar todo el sitio; `app.js` lo registra en `/sw.js`. Moverlo rompe `test_service_worker_se_sirve_desde_la_raiz` y el alcance del SW.
- El manifest está en `/static/manifest.json`; las pruebas verifican que cada icono declarado responda 200.

## Entregas

- `python entregar.py N` genera `entregas/semana-N.pdf` desde `entregas/semana-N.md` (plantilla en `entregas/plantilla.md`): requiere `fpdf2`, usa la etiqueta git `semana-N` si existe y hace una petición real a `URL_PUBLICA` (timeout 90 s). El PDF incluye `## Lecciones aprendidas` de este archivo: mantenga ese título exacto.
- Los PDF no se versionan (`.gitignore`). Guarde los prompts en `prompts/` como `semana-N-tema.md` y declare el uso de IA en cada entrega (`POLITICA_IA.md`).

## Configuración y despliegue

- `.env` no se carga automáticamente (no hay python-dotenv): `SECRET_KEY` y `DATABASE_URL` de `.env.example` aún no los lee el código; no asuma que existen.
- Render construye solo con `requirements.txt` y arranca con `uvicorn app.main:app --host 0.0.0.0 --port $PORT`; health check en `/salud`; despliega cuando pasa CI. CI (`.github/workflows/ci.yml`) corre Ruff, pytest y un arranque real de GET `/salud` en push a `main` y en PR.

## Convenciones

- Al iniciar cada sesión, leer STATE.md.
- Nombres en español, claros y completos; cambios pequeños, una tarea por rama y prueba antes del código.
- Starlette 1.x: `TemplateResponse` recibe `request` como primer argumento (como en `paginas.py`); no use la firma antigua con el nombre de la plantilla primero.
- `requirements-dev.txt` instala `httpx2`, no `httpx`: es lo que exige el `TestClient` de Starlette 1.x. No lo "corrija" a `httpx`.
- Lea `STATE.md` al iniciar cada sesión y actualice estado, decisiones y siguiente paso al cerrar.

## Qué no hacer

- No escribir claves ni contraseñas en el código (se leen de variables de entorno) ni instalar dependencias sin verificar que existen en pypi.org.
- No modificar ni borrar pruebas para que pasen.
- No usar datos personales reales en pruebas ni en datos de ejemplo.
- No hacer commit ni push: los hace el
desarrollador.

## Lecciones aprendidas

Cada vez que el agente se equivoque, escriba aquí la regla que evita repetir el error.
