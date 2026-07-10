# multi-source-data-validator

Herramienta Python que cruza datos de tres fuentes independientes para detectar inconsistencias, errores tipográficos y discrepancias — eliminando la revisión manual de documentos.

## El problema

En entornos de ensayos regulados, los mismos datos del producto (cliente, marca, modelo, norma, ID de certificación) deben coincidir exactamente en tres documentos independientes:

- **TRF** — el informe interno de ensayo
- **Sistema** — el exportado del sistema de gestión interno
- **Certificadora** — el documento emitido por el organismo certificador externo

Comparar estas tres fuentes manualmente es lento, propenso a errores y debe hacerse en cada ensayo. Un solo error tipográfico o campo distinto puede generar problemas de cumplimiento normativo.

## Qué hace

Lee un único archivo `MASTER.txt` con los tres documentos exportados, parsea cada fuente de forma independiente y genera una tabla comparativa con los tres valores lado a lado — haciendo cualquier discrepancia inmediatamente visible.

Además ejecuta controles automáticos sobre el TRF:
- Consistencia de numeración de páginas
- Ocurrencia de la palabra "error"
- Detección de caracteres aislados anómalos (artefactos comunes de copy-paste)

Soporta 6 formatos de certificadora: **Intertek, Qetkra, TÜV Rheinland, NCC, IRAM, BVE, Lenor OCP**.

## Resultado

Un DataFrame de pandas comparando todos los campos entre las tres fuentes:

| | TRF | CERT | SIST |
|---|---|---|---|
| Cliente | Acme Corp | Acme Corp | Acme Corp. |
| Modelo | XR-200 | XR-200 | XR200 |
| Norma | IEC 60950 | IEC 60950 | IEC 60950-1 |

Las discrepancias quedan visibles de inmediato para revisión.

## Cómo ejecutarlo

**Requisitos**
```bash
pip install pandas
```

**Configuración**

Crear un archivo `MASTER.txt` con la siguiente estructura:
```
[texto del documento de certificadora]
fin cert
[exportado del sistema]
fin sist
[texto del TRF]
```

Usar `none cert`, `none sist` o `none trf` al inicio para omitir una fuente.

**Ejecución**
```bash
jupyter notebook Correccion_TRF_Laburo.ipynb
```

Ejecutar todas las celdas. La última llama a `comparacion()` y devuelve la tabla comparativa.

## Stack

Python · pandas · re · Jupyter Notebook

## Contexto

Desarrollado para automatizar un control de calidad diario en un laboratorio de ensayos de seguridad eléctrica regulado. Redujo el tiempo de verificación cruzada de ~15 minutos por informe a menos de 1 minuto.
