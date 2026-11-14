# Mini-aplicación de práctica: Trivia de cultura general

## Versión A
**Objetivo:** construir la mini-aplicación con un prompt de una línea.
**Herramienta y modelo:** Copilot con el modelo gpt-6-luna.
**Prompt:**
```text
Haz una app web de trivia con FastAPI.
```
**Resultado:** Produjo una aplicación web funcional de trivia con tres categorías, separando backend y frontend. Sin embargo, el agente tomó todas las decisiones de diseño e implementación por su cuenta al no tener un contrato de pruebas. Pruebas aprobadas: N/A (sin tests formales iniciales). Errores de Ruff: No reportados.

## Versión B
**Objetivo:** construir la misma aplicación con un prompt estructurado, AGENTS.md y pruebas de aceptación.
**Herramienta y modelo:** Copilot con el modelo gpt-6-luna.
**Prompt:**
```markdown
## Rol
Actúa como desarrollador backend con experiencia en FastAPI, Jinja2 y pruebas con pytest.

## Objetivo
Construir la mini-aplicación web de Trivia de cultura general con estos requisitos:
1. GET / muestra una página que presenta las preguntas una por una, con cuatro botones de opción, indica si la respuesta fue correcta y muestra el puntaje acumulado.
2. GET /api/preguntas devuelve una lista de 5 preguntas de cultura general. Cada pregunta tiene los campos id (entero), texto (texto) y opciones (lista de 4 textos). La respuesta correcta no se incluye.
3. POST /api/responder recibe {"id": 1, "opcion": 2}, donde opcion es un índice de 0 a 3, y devuelve {"correcta": true o false, "respuesta_correcta": índice de 0 a 3}.
4. Si el id no existe, responde con el código 404. Si la opción no está entre 0 y 3, responde con el código 422.

## Contexto
Lee AGENTS.md. El contrato de la API está en tests/test_aceptacion.py: las rutas, los nombres de los campos y los códigos de estado deben coincidir con esas pruebas.

## Restricciones
Usa solo las dependencias de requirements.txt.
Define las rutas en app/main.py y la lógica en app/servicios.py, sin importar FastAPI en servicios.
Guarda los datos en memoria, sin base de datos ni archivos.
No modifiques tests/test_aceptacion.py.
La página usa HTML, CSS y JavaScript sin librerías externas.

## Formato de salida
Antes de escribir código, muestra el plan con las rutas completas de los archivos que vas a crear y espera mi aprobación. Al terminar, resume en una línea qué hace cada archivo.

## Ejemplo
GET /api/preguntas devuelve:
[{"id": 1, "texto": "¿Cuál es el planeta más grande del sistema solar?", "opciones": ["Marte", "Júpiter", "Saturno", "Venus"] }, ...]

POST /api/responder con {"id": 1, "opcion": 1} devuelve:
{"correcta": true, "respuesta_correcta": 1}

## Criterio de éxito
python -m pytest -v pasa todas las pruebas y ruff check . no reporta errores. Si después de 3 intentos las pruebas no pasan, detente y explica el problema.
```
**Resultado:** La aplicación arrancó correctamente al primer intento. Pruebas aprobadas: 6 de 6. Requisitos cumplidos: 6 de 6. Errores de Ruff: 0. Separó adecuadamente rutas y lógica sin tomar decisiones por su cuenta fuera de lo especificado.

## Mejora
**Objetivo:** Modificar el endpoint GET /api/preguntas para que acepte el parámetro opcional cantidad (entre 1 y 5), devolviendo esa cantidad de preguntas (por defecto 5).
**Herramienta y modelo:** Copilot con el modelo gpt-6-luna.
**Prompt:**
```markdown
## Rol
Actúa como desarrollador backend con experiencia en FastAPI.

## Objetivo
Modificar el endpoint GET /api/preguntas para que acepte el parámetro opcional `cantidad`, entre 1 y 5, y devuelva solo esa cantidad de preguntas. Sin el parámetro devuelve 5, y con un valor fuera del rango responde con el código 422. La página principal también debe permitir elegir cuántas preguntas se juegan. Además, crea una prueba nueva en tests/test_mejora.py que compruebe esta mejora.

## Contexto
Lee AGENTS.md, app/main.py, app/servicios.py y app/templates/index.html.

## Restricciones
No modifiques tests/test_aceptacion.py.

## Formato de salida
Antes de escribir código, muestra el plan con las rutas completas de los archivos y espera mi aprobación.

## Ejemplo
GET /api/preguntas?cantidad=3 devuelve 3 preguntas.
GET /api/preguntas?cantidad=8 devuelve 422.

## Criterio de éxito
`python -m pytest -v` pasa todas las pruebas (las de aceptación y las de la mejora). Si después de 3 intentos no se cumple, detente y explica el problema.
```
**Resultado:** Se implementó exitosamente la lógica del parámetro opcional y se actualizó la interfaz web. Pruebas aprobadas: 9 de 9. No hubo necesidad de hacer correcciones adicionales; todo funcionó y pasó los chequeos de Ruff sin errores.

## Comparación
**Versión A:** Pruebas aprobadas N/A (no había contrato inicial). Requisitos cumplidos: parcial/asumidos. Errores de Ruff: no reportados.

**Versión B:** Pruebas aprobadas 6 de 6. Requisitos cumplidos: 6 de 6. Errores de Ruff: 0.

**Mejora:** Pruebas aprobadas 9 de 9. Errores de Ruff: 0.

**Conclusión:** El uso de un prompt estructurado (Versión B) junto con archivos de contexto (AGENTS.md) y un conjunto estricto de pruebas de aceptación predefinidas reduce drásticamente la ambigüedad. Mientras que en la Versión A el modelo toma decisiones autónomas sobre la estructura y validaciones, la Versión B garantiza alineación total con los requerimientos técnicos, asegurando robustez, cumplimiento de contratos de API y cero margen para desviaciones no deseadas.
