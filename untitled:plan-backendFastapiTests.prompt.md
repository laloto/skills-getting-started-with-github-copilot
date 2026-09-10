## Plan: Pruebas Backend FastAPI

Añadir una suite independiente en `tests/` usando `pytest` y `TestClient`, aislando el estado global mutable de la aplicación. Estructurar cada prueba con el patrón AAA: `Arrange` para preparar el estado y los datos, `Act` para ejecutar una única acción HTTP y `Assert` para verificar la respuesta y sus efectos.

**Pasos**

1. Añadir `pytest` a [requirements.txt](requirements.txt).
2. Crear [tests/test_app.py](tests/test_app.py) con:
   - Fixture para restaurar `activities` antes y después de cada prueba.
   - Pruebas de `GET /activities`, separando claramente `Arrange`, `Act` y `Assert`.
   - Inscripción exitosa y persistencia del participante.
   - Rechazo de inscripción duplicada (`400`).
   - Baja exitosa y participante inexistente (`404`).
   - Actividad inexistente en inscripción y baja (`404`).
   - Redirección de `GET /`.
   - Parámetro `email` ausente (`422`).
   - Respuesta de `/static/index.html`.
3. Mantener las pruebas contra el contrato actual, sin probar reglas que aún no existen, como límite de participantes o validación de formato de email.
4. Aplicar AAA de forma consistente: mantener la preparación antes de la llamada, realizar la acción HTTP en una línea o bloque identificable y colocar las aserciones después de la acción; evitar mezclar mutaciones y aserciones dentro de la preparación.
5. Ejecutar:
   - `python -m pytest -q`
   - `python -m pytest -q tests/test_app.py`
   - `python -m compileall src tests`

**Archivos relevantes**

- [src/app.py](src/app.py): endpoints y estado global que se probarán.
- [requirements.txt](requirements.txt): dependencia `pytest`.
- [pytest.ini](pytest.ini): configuración existente de descubrimiento/importación.
- [tests/test_app.py](tests/test_app.py): nueva suite de pruebas.

**Decisiones**

- Usar `TestClient`; no arrancar un servidor durante las pruebas.
- Mantener `tests/` en la raíz del repositorio.
- Restaurar `activities` mediante copia profunda para evitar contaminación entre casos.
- No modificar la lógica de producción ni el workflow de CI en esta tarea.
