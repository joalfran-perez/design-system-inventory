# 2026-09-29 — Fill de texto en canvas: Strong + interactive

Status: **aceptada**.

## Qué

En páginas de fundamentos (piloto: Ext Tipografía `1:89`), el **fill del nodo TEXT** (no el estilo TEXT, que no carga color) usa aliases `Color`:

- Default / accesibilidad: `Color Tokens/Text/Strong` — todo texto no interactivo.
- Interactivo (`Contenido/a…`, hyperlink, o fill Sandbox `Primary 70`): `Color Tokens/Text interactive/Default` (azul). No `Subtle - default` salvo pedido explícito.
- No primitivos (`Neutral 100`, `Primary 70`) como color de texto.

Typography (`Type/*`, `Font/*`) no se toca en esta operación.

## Por qué

El restore 1:1 desde Sandbox copiaba Neutral 100 (primitivo). Accesibilidad USS: Strong. Un pass que rebindeó “cualquier fill ≠ Subtle” a Subtle rompió Ext Tipografía; `textStyleId` no restaura color.

## Consecuencias

- Matcher interactivo: estilo `Contenido/a`, hyperlink, o mapa Sandbox Primary 70 (mismos node ids).
- Bind por nodo (`setBoundVariableForPaint` / `setRangeFills`). Skip INSTANCE.
- No aplicar fills a `TextStyle` (no hay `paints`). Otras páginas no heredan el color del estilo link.
