# 2026-08-31 — visualSpec superficial (raíz + L1)

Status: **aceptada**.

## Qué

Cada componente/variante lleva `visualSpec` opcional (fills, strokes, effects, radius, opacity, layout, typography) más `childSpecs` de hijos **directos**. Campos omitidos si son default. Hex mayúsculas; alpha drop; px.

Forma canónica: [visual-spec.md](https://github.com/southleft/skills-for-figma/blob/main/skills/design-system-inventory-figma/references/visual-spec.md) (upstream). Contrato resumido: `context/design.md`.

## Por qué

Espejo de `figma_get_design_system_kit` (root + first-level). Un árbol completo por cada variante de un set grande no cabe en una llamada. Profundidad ilimitada, reactions e instancias = otro skill.

## Consecuencias

- No añadir recursor en `inventory.js` sin ADR.
- No afirmar que el inventario “documenta el componente entero”.
- Mapear a CSS con la tabla de `visual-spec.md`; no inventar campos.
