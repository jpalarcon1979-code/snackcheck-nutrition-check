# TEST REPORT — SnackCheck

**Fecha de consolidación:** 2026-11-02  
**Alcance:** pruebas manuales relatadas y respuestas compartidas durante la tutoría. Se incorporaron capturas HTTP verificables de cuatro rutas y Nutri-Score A. Se sigue diferenciando lo probado externamente de simulaciones internas y de códigos HTTP no confirmados.

| ID | Caso | Método | Resultado observado | Estado |
|---|---|---|---|---|
| TC-001 | Nutella `3017620422003` | Postman, extremo a extremo | Informe completo; NO SALUDABLE, score 2/3, Nutri-Score no disponible | Aprobado funcionalmente |
| TC-002 | Coca-Cola `5449000000996` | Postman, extremo a extremo | Informe completo; NO SALUDABLE, Nutri-Score E, score 0/3, azúcar amarilla tras corrección | Aprobado funcionalmente |
| TC-003 | Body vacío `{}` | Postman, producción, 2026-11-02 | JSON de instrucciones de uso, **HTTP 200** confirmado | **Aprobado** — `evidencias/TC-003_body_vacio_HTTP200.png` |
| TC-004 | Código `abc123` | Postman, producción, 2026-11-02 | `invalid_barcode`, **HTTP 400** confirmado | **Aprobado** — `evidencias/TC-004_codigo_invalido_HTTP400.png` |
| TC-005 | `98765432109876` | Postman, producción, 2026-11-02 | `product_not_found`, **HTTP 404** confirmado nuevamente | **Aprobado** — `evidencias/TC-005_producto_no_encontrado_HTTP404.png` |
| TC-006 | Sin `barcode` (`{"codigo":"123"}`) | Postman, producción, 2026-11-02 | `invalid_barcode`, **HTTP 400** confirmado | **Aprobado** — `evidencias/TC-006_campo_ausente_HTTP400.png` |
| TC-007 | Código >14 dígitos | Postman | Se reportó `invalid_barcode`; valor de entrada no quedó registrado en el chat | Validación parcial; conservar evidencia de entrada |
| TC-008 | Nutrientes `null` | Datos simulados/Pin Data en n8n | `Check - Nutrient Data` tomó rama `false`, nodo de respuesta configurado `insufficient_data` | Prueba interna aprobada; no E2E HTTP |
| TC-009 | Nutri-Score A | Datos simulados/Pin Data en n8n | `concernScore=0`, `nutriReading=saludable`, `verdictLevel=saludable` | Prueba interna aprobada; no E2E HTTP |
| REG-001 | Prompts revisados, Nutella | Postman | Informe generado manteniendo veredicto y nutrientes; posterior ajuste de prompt cauteloso añadido | Regresión parcial: ajuste último no reprobado |
| REG-002 | Modelo Groq actualizado, BACKBOX `1234567890123` | Postman | Informe SALUDABLE, Nutri-Score B, score 0/3, explicación sin recomendaciones excesivas | Aprobado funcionalmente |

| PERF-001 | Cinco ejecuciones consecutivas, Nutella | n8n Executions, 2026-11-02 | 3.167 s; 1.219 s; 1.504 s; 1.515 s; 2.446 s; **promedio 1.9702 s**, mínimo 1.219 s, máximo 3.167 s; las cinco exitosas | Aprobado en entorno de pruebas; desglose API/IA no disponible |
| PROD-001 | Nutella mediante URL de producción | Postman hacia endpoint `/webhook/nutrition-check`, sin ejecutar manualmente el canvas | Informe completo NO SALUDABLE, preocupación 2/3, cifras coherentes; respuesta recibida | Aprobado funcionalmente; HTTP 200 pendiente de confirmación explícita |
| TC-010 | Nutri-Score A real, `8410505192216` | Postman, producción, 2026-11-02 | **HTTP 200**, SALUDABLE, Nutri-Score A, preocupación 0/3; Luengo, nombre de producto no disponible | **Aprobado** — `evidencias/TC-010_nutriscore_A_HTTP200.png` |
| PROD-002 | Ejecución n8n de TC-010 | n8n Executions, 2026-11-02 17:35:33 | Estado `Succeeded`, duración **3.131 s** | **Confirmado** — `evidencias/PROD-002_ejecucion_n8n_exitosa.png` |

## Evidencias recomendadas para la entrega

Guardar capturas de: un ejemplo de cada rama, códigos 400 y 404 en Postman, respuesta de datos insuficientes en el IF, resultado Nutri-Score A simulado, y canvas completo. Anotar fecha, entrada, estado HTTP y resultado esperado/real.

## Hallazgos

1. Dos códigos aparentemente ficticios (`9999999999999` y `1234567890123`) sí correspondían a productos en Open Food Facts; se sustituyó el primero por un código que produjo 404.
2. En Nutella, los campos `nutriscore_grade` y `nutrition_grades` devolvieron `unknown`; se presenta `No disponible` sin inventar nota.
3. Se corrigió el semáforo de azúcar media en Coca-Cola.
4. Postman visualizó bien emojis y caracteres que Windows PowerShell mostró incorrectamente.
5. Se fijaron datos simulados para TC-008 y TC-009, luego se retiraron; la exportación final tiene `pinData: {}`.
6. Se configuró explícitamente HTTP 404 en la rama de producto no encontrado.

## Pendientes no bloqueantes para entrega académica

- Capturas HTTP de TC-003/004/005/006: **incorporadas**. La prueba TC-007 (más de 14 dígitos) sigue sin entrada exacta y captura conservadas.
- Probar `insufficient_data` con un producto real mediante HTTP, si se dispone de un registro apropiado.
- Nutri-Score A real: **completado en TC-010** (HTTP 200, `8410505192216`).
- Validar manejo de caída de Open Food Facts y Groq antes de producción real.

## Auditoría de exportación final (2026-11-02)
- Exportación recibida con **26 elementos: 23 nodos funcionales y 3 Sticky Notes**.
- Todos los 23 nodos funcionales tienen texto de nota; 22 tenían `Display Note in Flow` activado en el origen. La versión publicable activa también la nota restante, de Open Food Facts.
- Nombres `Route - Verdict Tone` y `Merge - Combine AI Branches` verificados.
- `pinData` vacío; `Respond - Product Not Found` tiene HTTP 404; ambos Groq usan `openai/gpt-oss-20b`.
- Se verificó `POST` y la respuesta Nutella desde la URL de producción (cuerpo correcto). La captura del código HTTP de producción no se proporcionó aquí.
- Las cinco ejecuciones exitosas incluyen el tiempo total; la vista mostrada no dio desglose API/IA.
## Capturas recibidas en la auditoría del 2026-11-02

| Archivo de evidencia | Observación verificable |
| --- | --- |
| `evidencias/TC-003_body_vacio_HTTP200.png` | Cuerpo vacío / autodocumentación HTTP 200 |
| `evidencias/TC-004_codigo_invalido_HTTP400.png` | Código con letras HTTP 400 |
| `evidencias/TC-006_campo_ausente_HTTP400.png` | Campo barcode ausente HTTP 400 |
| `evidencias/TC-005_producto_no_encontrado_HTTP404.png` | Código desconocido HTTP 404 |
| `evidencias/TC-010_nutriscore_A_HTTP200.png` | Producto Luengo Nutri-Score A HTTP 200 |
| `evidencias/PROD-002_ejecucion_n8n_exitosa.png` | Ejecución n8n con éxito |

**Nota de límites:** TC-008 (sin nutrientes) solo tiene prueba interna con Pin Data, no prueba real por HTTP. No debe declararse validado de extremo a extremo. TC-007 carece aún de captura identificable. El HTTP 200 de Nutella en PROD-001 sigue pendiente de evidencia explícita, aunque se obtuvo una respuesta válida.
