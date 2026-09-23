# Gotcha: timeout al escanear páginas

**Síntoma:** `use_figma` muere / time limit con `INCLUDE` que incluye `"components"` y `PAGE_NAMES = []`.

**Causa:** el script carga cada página (`setCurrentPageAsync`) y hace `findAllWithCriteria`. ~30+ páginas (USS One Mobile ~48, muchas staging) no caben en una call.

**Fix:**
1. `PAGE_NAMES = ["Buttons", "Cards", …]` — solo páginas de componentes.
2. Omitir Testing / `_` / Patterns / Templates / DOR / Annotations / Otros / “Comenzar”.
3. No convertir a N calls paralelas (eso es contrato ModUSS, no de este skill) salvo ADR.

Tokens/styles son file-scoped: `INCLUDE = ["tokens"]` o `["styles"]` **no** recorre páginas.
