# 2026-09-29 — Bind PAINT de primera clase (como TEXT)

Status: **aceptada**.

## Qué

Tras copiar la colección `Color`, los estilos PAINT locales se bindean a variables
**por nombre**, con el mismo rigor que TEXT → `Typography`:

1. `Light mode/{path}` y `☾ Dark mode/{path}` → `Color Tokens/{path}` (semántico).
2. `_Base/{palette}/{swatch}` → `_Base palettes/{palette}/{swatch}`.
3. Paleta plana `{Palette}/{step}` → `_Base palettes/{PaletteKit}/{token} {step}`
   (`Exito`→Success, `Alerta`→Warning, `Secondary`→`Secondary - Verde`,
   Neutral 10 → `Neutral 10 (blanco)`).

Sin variable en el kit (`Terciary/*`, `bg/on-light/*`, …): gap, no crear tokens.

Capas Color (kit):

- **Primitivos (hex):** `_Base palettes/…`, `Branding/…`. Secondary 100 y 10 = únicos primitivos
  con hex distinto Light/Dark.
- **`Color Tokens/…`:** aliases a primitivos, salvo Background 3/4, Surface ghost default
  `#FFFFFF00`, Elevation 1/2 Light `#FFFFFF00`.
- Bind: Light/Dark → `Color Tokens`; `_Base/` y `{Palette}/{step}` → primitivos.

Piloto sandbox 2026-09-29: +73 binds planos; 12 unmatched; 322/334 PAINT bindeados.

## Por qué

El plan 1:1 de variables dejó paletas planas hardcoded. El equipo las trata como
los TEXT styles: el estilo debe apuntar a “su” variable. El matcher se prueba en
sandbox **antes** de One / Ext. Library.

## Consecuencias

- Pipeline destino (aún GATE): import Color → bind PAINT (3 reglas) → Typography → bind TEXT.
- Cero writes a `w2FNtlyzRgJtrzkw7ywZHj` / `sDv64Fnh1bMxJXMOlTTZf8` hasta OK del sandbox con este bind
  **y** extras de colección borrados (piloto 3, ADR 2026-09-28).
- EFFECT y paletas sin kit var siguen fuera.
