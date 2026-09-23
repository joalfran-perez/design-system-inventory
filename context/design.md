# Design — inventario Figma (este repo)

Skill de **extracción**, no de datos. Un archivo Figma → un kit estructurado. La auditoría USS persistida vive en ModUSS.

Fuente canónica (southleft, no fork):
https://github.com/southleft/skills-for-figma/blob/main/skills/design-system-inventory-figma/SKILL.md

| Pieza | Dónde |
|-------|--------|
| Skill + `inventory.js` + visualSpec | Repo comunitario. Invocar `skillNames: "design-system-inventory-figma"` |
| Overlay USS (verbosidad, fileKeys, output) | `skills/design-system-inventory-figma.md` |

Equivalente abierto de `figma_get_design_system_kit`, idioma `use_figma` (sin Desktop Bridge). Variables = Plugin API, cualquier plan.

## Contrato de salida

```
{ fileName, generatedAt, verbosity, include, pagesScanned, totalPagesInFile,
  tokens:    { collections[], summary },
  components:{ items[], summary },
  styles:    { items[], summary },
  ai_instruction }
```

- **tokens:** colecciones locales + modos. Alias → `{Name.con.puntos}`. Targets de alias fuera de colecciones “local” se resuelven por id (no filtrar `getLocalVariableCollectionsAsync` como universo).
- **components:** `COMPONENT_SET` (variants) + standalone (no hijos de set). `properties` = `componentPropertyDefinitions`. `full` añade `visualSpec` + `childSpecs` (L1).
- **styles:** PAINT / TEXT / EFFECT. `resolvedValue` solo en `full`.

Colores hex `#RRGGBB` mayúsculas. Spacing/size en px.

## Verbosidad (control de payload)

| `VERBOSITY` | Qué incluye | Cuándo |
|-------------|-------------|--------|
| `inventory` | nombres, ids, counts | primer pass / archivo enorme |
| `summary` | + properties / metadata; sets → `variantCount`; sin visualSpec | API del componente |
| `full` | + visualSpec por variante + resolved styles | un componente o páginas ya acotadas |

Válvulas: `INCLUDE` (`tokens`\|`components`\|`styles`), `COMPONENT_NAME_FILTER` (substring), `PAGE_NAMES` (`[]` = todas).

## Profundidad visual

Raíz + hijos directos. No reactions, no instancias anidadas, no árbol ilimitado → `deep-component-figma`. Variant set → CSS SM: `analyze-component-set-figma`. Tokens a disco: `export-tokens-figma`. Los tres son skills hermanos del mismo repo comunitario (`southleft/skills-for-figma`), instalados localmente en `.agents/skills/` igual que este. Cómo promptear cada una: `skills/prompting-figma-skills.md`.

Tabla campo→CSS: [references/visual-spec.md](https://github.com/southleft/skills-for-figma/blob/main/skills/design-system-inventory-figma/references/visual-spec.md) upstream. No duplicar.

## vs ModUSS (no confundir)

| | Este skill | ModUSS |
|--|------------|--------|
| Qué es | Extractor one-shot | Repo de datos / diffs / consolidación |
| Aliases | se conservan `{path}` | se resuelven a hex |
| Páginas | loop `setCurrentPageAsync` en **un** script + `PAGE_NAMES` | 1 `use_figma` **por página**, en paralelo (prohíbe el loop) |
| Compresión | VERBOSITY / INCLUDE / filtros | counts por categoría + samples; truncación ~20KB |
| Destino | return del plugin (o `output/` local) | `USS* Design System Inventory/tokens/*.json` |

Al extraer para ModUSS: este skill puede alimentar, pero ModUSS exige JSON resuelto + shape fijo (`decisions/002` allá). No escribir inventarios USS aquí.

Keys Figma USS (copiar; no re-parsear URLs): ver tabla abajo. Access = última nota 2026-08; revalidar si `use_figma` falla.

## Archivos Figma USS (9)

Main = view-only en Desktop/Mobile. No proponer edits al core. Additions → USS One / Extension Library (regla ModUSS `decisions/012`).

| Sistema | Archivo | fileKey | Access (ago 2026) |
|---------|---------|---------|-------------------|
| USS (core) | Fundamentos | `16PDlIOKg8kb176dMz0Ckg` | MCP read-only (2026-09-02; estilos locales) |
| USS (core) | Desktop | `nCGtIjrJLW6v4ZMvzTsOAd` | view |
| USS (core) | Mobile | `uH4MBdFSPYvfxwXcrdFic9` | view (solo Comenzar) |
| USS One | Fundamentos | `w2FNtlyzRgJtrzkw7ywZHj` | edit |
| USS One | Desktop | `5XVuReA8as6xhPa0jUzVOg` | edit |
| USS One | Mobile | `joO68kJ8XUJ8NepohniIRC` | edit (~48 págs → acotar `PAGE_NAMES`) |
| Ext. Library | Fundamentos | `sDv64Fnh1bMxJXMOlTTZf8` | edit |
| Ext. Library | Desktop | `DSOeWAXEvG2O18rQLMSqAf` | edit |
| Ext. Library | Mobile | `mVPBqzhfGqQkBJSrpLwEGV` | edit |

Arquitectura de tokens / diffs entre sistemas: `ModUSS/context/design.md`. No duplicar aquí.

## Runtime `use_figma` (resumen)

Plain JS; `return` es el resultado. `figma.currentPage` arranca en la primera página de cada call. Setter síncrono `figma.currentPage =` no existe. Detalle: `gotchas/`.
