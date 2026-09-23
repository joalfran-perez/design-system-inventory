# Skill: design-system-inventory-figma (overlay USS)

Instalado (2026-09-23, `npx skills`, alcance proyecto): `.agents/skills/design-system-inventory-figma/`
— `SKILL.md`, `scripts/inventory.js`, `references/visual-spec.md`. Cursor lo descubre y resuelve solo
por `skillNames`. Fuente: [southleft/skills-for-figma](https://github.com/southleft/skills-for-figma/blob/main/skills/design-system-inventory-figma/SKILL.md).
ADR: `decisions/2026-09-23-install-skill-project-scope.md`.

Este archivo es el procedimiento **de este repo**. No editar el `inventory.js` instalado a mano — se
actualiza reinstalando el skill (mismo comando), no parcheando el archivo.

Usar para extraer tokens/components/styles de **un** archivo Figma. Runtime: `figma-use` **primero**.

No usar para árbol profundo (`deep-component-figma`), CSS SM (`analyze-component-set-figma`), ni tokens a disco (`export-tokens-figma`) — mismos nombres en el repo comunitario, también instaladas en `.agents/skills/`. Cómo promptear cada una: `skills/prompting-figma-skills.md`.

## Cargar

1. `AGENTS.md` + `state/current.md` + este archivo
2. Skill `figma-use`
3. **Fallback únicamente**: si `skillNames: "design-system-inventory-figma"` no resuelve (skill desinstalado/roto), fetch puntual a `inventory.js` upstream — no Read al prompt entero si se puede inyectar al tool. No es el camino principal.
4. Si falla MCP: `gotchas/` por síntoma
5. Forma `visualSpec`: `context/design.md` (contrato). Tabla CSS completa → reference upstream, no local.

## Invocación

`use_figma` + `fileKey` + `skillNames: "design-system-inventory-figma"`.

Constantes (tope del script upstream; inlined `const`, no IIFE):

| Const | Default upstream | En este repo |
|-------|------------------|--------------|
| `INCLUDE` | `["tokens","components","styles"]` | Solo tokens → `["tokens"]` (no carga páginas) |
| `VERBOSITY` | `"full"` (peligroso) | Primer pass: `"inventory"` o `"summary"` |
| `COMPONENT_NAME_FILTER` | `null` | substring p.ej. `"Button"` + `full` |
| `PAGE_NAMES` | `[]` = todas | Páginas de componentes. USS One Mobile ~48 págs. |

## Procedimiento

1. `fileKey` de la URL o tabla en `context/design.md`.
2. View-only → no `use_figma`. `gotchas/read-only-access.md`.
3. Setear constantes **en el call**, no en un archivo de este repo.
4. Si truncado / timeout: no reintentar `full`. Bajar verbosidad / `INCLUDE` / `PAGE_NAMES`. `gotchas/payload-size.md`, `gotchas/page-scan-timeout.md`.
5. Consumir: `tokens` → valores; `components[].properties` → API; `variants[].visualSpec` → apariencia; `styles` → no-variable. Referenciar, no pegar.
6. Opcional: `output/<fileKey>-<verbosity>.json` (gitignored).
7. Cierre: `state/current.md` + `logs/YYYY-MM-DD-<fileKey>.md` ≤40 líneas.

## Paralelo / no hacer

Varios archivos = N `use_figma` en el mismo mensaje. No fan-out de páginas (eso es ModUSS). No `loadAllPagesAsync`. No pegar kit `full` al chat. No copiar `SKILL.md` / `inventory.js` / `visual-spec.md` sueltos fuera de `.agents/skills/` (esa ruta es la instalación oficial, ya versionada; cualquier otra copia es vendor no autorizado).
