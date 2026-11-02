# CHANGELOG — SnackCheck

Formato inspirado en Keep a Changelog. Versión SemVer propuesta para entrega académica, no basada en etiquetas Git verificadas.

## [1.0.0] — 2026-11-02

### Added
- Webhook POST `/nutrition-check`, validación de body y formato del código.
- Consulta a Open Food Facts, extracción de nutrientes, tratamiento de datos faltantes.
- Semáforos, puntuación de preocupación y decisión de veredicto basada en reglas.
- Dos rutas de explicación IA mediante Groq y respuesta HTTP en texto UTF-8.
- Documentación técnica, registro de pruebas y diagrama editable.

### Changed
- Nutri-Score no disponible se muestra sin atribuir una letra inexistente.
- Ajuste de semáforo para categoría `medio` (amarillo).
- Revisión de prompts para evitar extrapolaciones nutricionales.
- Ambas rutas Groq utilizan `openai/gpt-oss-20b`.
- Se configuró HTTP 404 de forma explícita en producto no encontrado.

### Fixed
- Pruebas del workflow completo ejecutadas desde el canvas, en lugar de solo escuchar el nodo Webhook.
- Eliminados los datos simulados fijados tras las pruebas internas.

### Known limitations
- Falta tratamiento especializado de fallos temporales de proveedores externos.
- No se incluyen límites de uso o autenticación del Webhook.
- Algunas verificaciones de estado HTTP no tienen captura adjunta todavía.


## Verificación académica del 2026-11-02 (sin cambio de versión)
- Cinco pruebas consecutivas exitosas; duración media 1.9702 s.
- Informe de Nutella obtenido mediante URL de producción.
- Se documentó el canvas con 23 notas de nodos y tres Sticky Notes, confirmados en la exportación final. Se normalizaron los nombres de Route y Merge y se añadió una imagen PNG del diagrama.
## Auditoría final de evidencias y documentación — 2026-11-02 (sin cambio de versión)
- Se confirmaron en Postman las respuestas HTTP 200, 400 y 404 con entradas específicas; se añadieron capturas depuradas.
- Se confirmó con una llamada real el caso Nutri-Score A (`8410505192216`), con HTTP 200 y veredicto SALUDABLE.
- Se actualizó el diagrama Excalidraw con cinco decisiones en rombos, ramas y respuestas claramente identificadas.
- Se estandarizaron los nombres de los dos nodos de modelo Groq en el JSON público, sin cambiar la lógica.
- Se actualizó el registro de pruebas manteniendo como simulada la comprobación de nutrientes ausentes.
