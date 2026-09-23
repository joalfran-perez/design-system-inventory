# Gotcha: runtime `use_figma`

**Síntoma:** script “corre” pero el tool no devuelve el kit; o error de API oscuro.

**Reglas del runtime (skill `figma-use`):**
- JS plano, `await` top-level, **`return` del objeto**. IIFE / `figma.closePlugin()` / `console.log` = resultado vacío.
- Error = nada aplicado. Leer, corregir, reintentar (no “un poquito más de script”).
- `figma.loadAllPagesAsync()` **no existe** aquí. `root.findAllWithCriteria` exige páginas cargadas.
- `figma.currentPage = page` lanza. Solo `await figma.setCurrentPageAsync(page)`.
- Cada call arranca en la primera página. Este extractor restaura `_origPage` al final; no lo quites.
- Colores Plugin API = 0–1 RGBA; este script emite hex. Fuentes: cargar antes de ops de texto (este inventario no crea texto).

**Fix:** no reescribir el extractor al estilo node/plugin de escritorio. Seguir `figma-use` + overlay `skills/design-system-inventory-figma.md`. Script = upstream, no local.
