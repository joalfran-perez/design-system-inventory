# 2026-09-02 — Fuente = skill comunitario; sin fork local

Status: **aceptada**.

## Qué

`SKILL.md`, `scripts/inventory.js` y `references/visual-spec.md` en la raíz eran copia 1:1 de
[southleft/skills-for-figma `design-system-inventory-figma`](https://github.com/southleft/skills-for-figma/blob/main/skills/design-system-inventory-figma/SKILL.md).
Se eliminan. Overlay USS: `skills/design-system-inventory-figma.md`.

Invocación: `use_figma` + `skillNames: "design-system-inventory-figma"`. Constantes se inlinan en el call.

## Por qué

Mantener el JS aquí diverge de upstream y duplica `AGENTS.md` / `context/design.md` / gotchas. La extracción USS usará el skill de la comunidad; este repo guarda fileKeys, access, verbosidad y vs ModUSS.

## Consecuencias

- No vendor. Si `skillNames` no resuelve: fetch puntual a GitHub, no commit.
- Default `"full"` es de **upstream**. Aquí se acota en cada run; no parchear el script.
- `visualSpec` canónico = reference upstream. Contrato local = `context/design.md`.
