# Gotcha: `use_figma` en archivo view-only

**Síntoma:** `"Operation attempted to modify the file while in read-only mode"` aunque el script solo lea; o `"Can't set \"paints\" in read-only mode"` / `"Can't set \"name\" in read-only mode"` al mutar.

**Causa:** la sesión Figma MCP no tiene **edit** en ese file. Core USS Desktop/Mobile = view (ago 2026). Fundamentos core (`16PDlIOKg8kb176dMz0Ckg`) = **MCP read-only el 2026-09-02** aunque los estilos son locales (`remote: false`) y `context/design.md` decía edit (ago 2026). Revalidar USS One / Ext. Library.

**Fix:**
- Cambiar de archivo (preferir librería local con edit) **o**
- Fallback MCP remoto (no este skill): `get_metadata` (páginas; sin nodeId), `get_variable_defs` (nodo/frame, **no** page id — “nothing selected”), `get_design_context` (una selección).

`get_variable_defs` es por subárbol, no inventario de archivo. No fingir cobertura completa.

Probar access con un `return { name: figma.root.name, pages: figma.root.children.length }` mínimo antes de `full`.
