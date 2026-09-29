# Estado actual

Actualizado: 2026-09-29 (cierre memoria).

## Tres Fundamentos (locales)

| File | fileKey | Colecciones | Vars |
|------|---------|-------------|------|
| Sandbox | `lBG9RxtvvaBiSfx78Sd472` | Radius 6, Space 19, Color 144, Typography 152 | 321 |
| Ext | `sDv64Fnh1bMxJXMOlTTZf8` | Space 19, Radius 6, Typography 152, Color 144 | 321 |
| One | `w2FNtlyzRgJtrzkw7ywZHj` | Color 144, Radius 6, Device 1, Typography 152, Space 19 (`10488:1798`) | 322 |

Core kit (`16PDlIOKg8kb176dMz0Ckg`) MCP read-only; **desvinculado** en los tres (humano). Self-library only.

`output/three-libs-post-manual.json` **obsoleto** (One sin Space). Space One keys son nuevas, no las publicadas del kit.

## Hecho (esta línea)

- Pipelines + purge GATE; TSV One unbind+delete; Device intacto.
- Space One: humano export Core → colección local; agente rebind `Espaciado/*` **1673**.
- Bind stale-mismo-nombre (nodos, skip INSTANCE): Sandbox **428**, Ext **411**, One **18329**. Estilos PAINT/TEXT ya correctos (0 stale).
- Ext Tipografía `1:89` fills canvas (ADR `2026-09-29-canvas-text-fill-strong-interactive`): **3601** `Text/Strong` + **81** `Text interactive/Default`. Estilos TEXT `Contenido/a` no tienen fill; 62 nodos link cubiertos en el 81.

## Pendiente

1. 2 TEXT mixed fills Subtle en Tipografía Sandbox/One.
2. (Opcional) puente ModUSS.
3. INSTANCE internals (heredan main).
4. Otras páginas Ext: fills de `…/a` no recorridas (estilos no cargan color).

## Blockers

Ninguno operativo.

## Fuera de cola

Desktop/Mobile kit→USS; EFFECT; commit.

## Siguiente sesión

Device no se toca. No reimport Color. No pass de fills “cualquier paint ≠ X”.
