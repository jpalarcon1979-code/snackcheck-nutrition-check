# TEST REPORT — SnackCheck

**Fecha de consolidación:** 2026-11-02  
**Alcance:** pruebas manuales relatadas y respuestas compartidas durante la tutoría. Se incorporaron capturas HTTP verificables de cuatro rutas y Nutri-Score A. Se sigue diferenciando lo probado externamente de simulaciones internas y de códigos HTTP no confirmados.

| ID | Caso | Método | Resultado observado | Estado |
|---|---|---|---|---|
| TC-001 | Nutella `3017620422003` | Postman, extremo a extremo | Informe completo; NO SALUDABLE, score 2/3, Nutri-Score no disponible | Aprobado funcionalmente |
| TC-002 | Coca-Cola `5449000000996` | Postman, extremo a extremo | Informe completo; NO SALUDABLE, Nutri-Score E, score 0/3, azúcar amarilla tras corrección | Aprobado funcionalmente |
| TC-003 | Body vacío `{}` | Postman, producción, 2026-11-02 | JSON de instrucciones de uso, **HTTP 200** confirmado | **Aprobado** — `EVIDENCIAS_SnackCheck.pdf`, página 2 |
| TC-004 | Código `abc123` | Postman, producción, 2026-11-02 | `invalid_barcode`, **HTTP 400** confirmado | **Aprobado** — `EVIDENCIAS_SnackCheck.pdf`, página 3 |
| TC-005 | `98765432109876` | Postman, producción, 2026-11-02 | `product_not_found`, **HTTP 404** confirmado nuevamente | **Aprobado** — `EVIDENCIAS_SnackCheck.pdf`, página 5 |
| TC-006 | Sin `barcode` (`{"codigo":"123"}`) | Postman, producción, 2026-11-02 | `invalid_barcode`, **HTTP 400** confirmado | **Aprobado** — `EVIDENCIAS_SnackCheck.pdf`, página 4 |
| TC-007 | Código >14 dígitos | Postman | Se reportó `invalid_barcode`; valor de entrada no quedó registrado en el chat | Validación parcial; conservar evidencia de entrada |
| TC-008 | Nutrientes `null` | Datos simulados/Pin Data en n8n | `Check - Nutrient Data` tomó rama `false`, nodo de respuesta configurado `insufficient_data` | Prueba interna aprobada; no E2E HTTP |
| TC-009 | Nutri-Score A | Datos simulados/Pin Data en n8n | `concernScore=0`, `nutriReading=saludable`, `verdictLevel=saludable` | Prueba interna aprobada; no E2E HTTP |
| REG-001 | Prompts revisados, Nutella | Postman | Informe generado manteniendo veredicto y nutrientes; posterior ajuste de prompt cauteloso añadido | Regresión parcial: ajuste último no reprobado |
| REG-002 | Modelo Groq actualizado, BACKBOX `1234567890123` | Postman | Informe SALUDABLE, Nutri-Score B, score 0/3, explicación sin recomendaciones excesivas | Aprobado funcionalmente |

| PERF-001 | Cinco ejecuciones consecutivas, Nutella | n8n Executions, 2026-11-02 | 3.167 s; 1.219 s; 1.504 s; 1.515 s; 2.446 s; **promedio 1.9702 s**, mínimo 1.219 s, máximo 3.167 s; las cinco exitosas | Aprobado en entorno de pruebas; desglose API/IA no disponible |
| PROD-001 | Nutella mediante URL de producción | Postman hacia endpoint `/webhook/nutrition-check`, sin ejecutar manualmente el canvas | Informe completo NO SALUDABLE, preocupación 2/3, cifras coherentes; respuesta recibida | Aprobado funcionalmente; HTTP 200 pendiente de confirmación explícita |
| TC-010 | Nutri-Score A real, `8410505192216` | Postman, producción, 2026-11-02 | **HTTP 200**, SALUDABLE, Nutri-Score A, preocupación 0/3; Luengo, nombre de producto no disponible | **Aprobado** — `EVIDENCIAS_SnackCheck.pdf`, página 6 |
| PROD-002 | Ejecución n8n de TC-010 | n8n Executions, 2026-11-02 17:35:33 | Estado `Succeeded`, duración **3.131 s** | **Confirmado** — `EVIDENCIAS_SnackCheck.pdf`, página 7 |

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

## Evidencia visual consolidada (02-11-2026)

Las seis capturas verificables de Postman y n8n se encuentran reunidas en [`EVIDENCIAS_SnackCheck.pdf`](EVIDENCIAS_SnackCheck.pdf), para facilitar su consulta y entrega en GitHub.

| Caso | Página | Evidencia |
|---|---:|---|
| TC-003 | 2 | Autodocumentación: HTTP 200 |
| TC-004 | 3 | Código inválido: HTTP 400 |
| TC-006 | 4 | Falta barcode: HTTP 400 |
| TC-005 | 5 | Producto no encontrado: HTTP 404 |
| TC-010 | 6 | Luengo, Nutri-Score A y veredicto saludable: HTTP 200 |
| PROD-002 | 7 | n8n: ejecución exitosa (3.131 s) |

**Límites:** TC-008 (sin nutrientes) fue probado con datos simulados, no mediante HTTP real. TC-007 no tiene captura de la entrada exacta; PROD-001 no cuenta con confirmación visual explícita de código HTTP 200. No se declara validación completa de esos casos.
