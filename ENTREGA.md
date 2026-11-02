# Entrega final de SnackCheck — 4Geeks

## Archivos principales
1. `SnackCheck_workflow_publicable.json`: workflow importable en n8n.
2. `SnackCheck_diagrama.excalidraw`: diagrama editable, con cinco rombos y rutas etiquetadas.
3. `README.md`: descripción, arquitectura, configuración y limitaciones.
4. `TEST-REPORT.md`: registro de casos, resultados y evidencias.
5. `CHANGELOG.md`: historial SemVer v1.0.0 y cambios documentales.

## Complementos
- `SnackCheck_arquitectura.png` / `.svg`: vista del diagrama.
- `evidencias/`: capturas recortadas de las pruebas de Postman y n8n, sin las barras del navegador.

## Estado y cautelas
- Se preserva el comportamiento del flujo y se actualizan únicamente los dos nombres de los modelos Groq en la copia publicable.
- Las pruebas faltantes quedan identificadas en `TEST-REPORT.md`: datos insuficientes solo por simulación y TC-007 sin captura completa.
- El flujo público se importa sin credenciales y como inactivo: añade tu propia credencial Groq y reemplaza el correo de ejemplo en `User-Agent` antes de usarlo.
- No agregues claves, tokens ni datos privados a GitHub.
- La guía de entrega permite que el instructor indique el canal final; un repositorio GitHub puede servir de referencia y como portafolio.
