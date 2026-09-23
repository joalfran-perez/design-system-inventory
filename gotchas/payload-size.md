# Gotcha: payload truncado o chat inflado

**Síntoma:** array cortado a mitad, sin error; o el kit `full` se pega al hilo y se come el context.

**Causa:** return de `use_figma` ~20KB cap (MCP). `VERBOSITY = "full"` en un DS real (100+ vars, sets con decenas de variantes) no cabe. Este skill no comprime por categoría (eso es ModUSS `decisions/001`); comprime por **válvulas**.

**Fix — en este orden:**
1. `inventory` → counts.
2. `summary` + `COMPONENT_NAME_FILTER` o `PAGE_NAMES`.
3. `INCLUDE` de una sección.
4. `full` solo en el recorte que sobrevivió.
5. Si hay que conservar: `output/<fileKey>-<verbosity>.json` (gitignored). Grep; no Read entero.

Nunca “probar full otra vez” en el mismo archivo sin acotar.

**Fundamentos core (`16PDlIOKg8kb176dMz0Ckg`):** `inventory` con `components` + página Iconos (~1192 comps) + keys de variables también trunca. Omitir Iconos, o `INCLUDE` tokens/styles solos, o devolver counts/prefijos.
