## Estado actual
Repositorio publicado en GitHub con el CI en verde.
El 2026-11-10 se agregó GET /api/version, que devuelve la versión configurada en app/proyecto.py. La prueba del endpoint valida la respuesta y las 7 pruebas pasan.
Página de inicio, /salud, /api/version y /docs funcionando en https://plataforma-ludica-p33k.onrender.com

.
## Decisiones
| Fecha | Decisión | Motivo |
| --- | --- | --- |
| 2026-11-10 | Exponer la versión mediante GET /api/version con respuesta tipada {"version": ...} | Permite consultar la versión por separado y mantener el contrato validado por FastAPI. |
| 2026-11-10 | Leer la versión desde proyecto.VERSION | app/proyecto.py es la única fuente de versión del proyecto; se evita duplicar el valor. |
## Siguiente paso
Priorizar y definir la primera historia funcional del dominio con criterios de aceptación; specs/tasks.md y specs/plan.md siguen siendo plantillas y no hay una siguiente funcionalidad acordada.