# Gotcha: segunda Color / Radius / Space no se borra en Figma

**Síntoma:** panel Variables de USS One Fundamentos muestra Color 144 y Color 143, Radius 6 dos veces, Space 19 extra. El trash de las filas de abajo está gris o no existe.

**Causa:** no son colecciones locales duplicadas. Son variables de **librerías habilitadas**:

- `USS - Fundamentos de diseño` (kit core, dueño externo)
- y/o `USS One - Fundamentos de diseño` (el mismo archivo suscrito a su publicación)

Plugin API local sigue siendo 5 colecciones (Color 144, Device, Radius 6, Typography 152, Space 19). `collection.remove()` no aplica a las filas de librería.

**Fix (en el archivo, no por ID):** Assets → Libraries → **desvincular** `USS - Fundamentos de diseño` (kit core). Esa librería estaba asociada al archivo sin posibilidad de edición; no se puede `remove()` de las colecciones remotas. No unpublish el kit. Color 144 vs 143 = local con `Branding/Dorado` vs snapshot publicado del core.

Resuelto en One **manualmente en la librería del archivo** (2026-09-29): se desvinculó el Core. Plan *One library dupes report*.