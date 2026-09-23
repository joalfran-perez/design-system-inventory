# 2026-08-31 — Verbosidad y alcance por páginas

Status: **aceptada**.

## Qué

Cuatro válvulas en el script **upstream** `inventory.js`: `INCLUDE`, `VERBOSITY` (`full`\|`summary`\|`inventory`), `COMPONENT_NAME_FILTER`, `PAGE_NAMES`. En este repo se inlinan en el call; no hay copia local.

Páginas: el runtime **no** soporta `loadAllPagesAsync`; `findAllWithCriteria` exige página cargada. El script hace loop `setCurrentPageAsync` y restaura la página original.

`PAGE_NAMES = []` = todas. En archivos ~30+ páginas el loop puede exceder el time limit de `use_figma`.

## Por qué

Un dump `full` de un DS real (150–575+ items) revienta el return (~20KB de truncación silenciosa en MCP) y el context window. USS One Mobile ~48 páginas: no escanear staging.

**Diferencia con ModUSS:** allá se prohíbe el loop (1 call/página, paralelo). Aquí el contrato del skill es **una** llamada; la válvula es `PAGE_NAMES`, no fan-out. No copiar la regla de ModUSS dentro de este extractor.

## Consecuencias

- Primer pass en archivo desconocido: `inventory` o `summary`, páginas de componentes explícitas.
- `full` solo con filtro de nombre o pocas páginas.
- Si timeout: acotar `PAGE_NAMES`, no reescribir a N calls salvo ADR nuevo.
