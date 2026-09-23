# 2026-08-31 — Inventario unificado vía Plugin API (una llamada)

Status: **aceptada**.

## Qué

Un `use_figma` con el script del skill comunitario `design-system-inventory-figma` (southleft) devuelve tokens + components + styles. Sustituye N llamadas MCP (`get_design_context` = una selección; `get_metadata` = árbol plano). Sin Desktop Bridge y **sin REST Enterprise**.

Aliases se emiten como `{Name.split("/").join(".")}`, no como hex resuelto ni como `VariableID`.

## Por qué

La sesión de extracción USS (3 archivos Fundamentos/Desktop/Mobile) mostró: `get_variable_defs` exige nodo (no page); view-only bloquea `use_figma`; dumps `full` se comen el contexto. Hacía falta un front door único y portable.

## Consecuencias

- Siempre cargar `figma-use` antes de correr.
- No tratar este return como el JSON de ModUSS (allá se resuelve a hex).
- Targets de alias pueden vivir fuera de `getLocalVariableCollectionsAsync()` — el script los carga por id.
