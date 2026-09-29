# 2026-09-28 — Kit externo = snapshot; copias internas evolutivas

Status: **aceptada**.

## Qué

El archivo **USS - Fundamentos de diseño** (`16PDlIOKg8kb176dMz0Ckg`) es un Kit UI de dueño externo:
sin edit de librería ni de archivo. Es la fuente de **convenciones** (nombres, modos, valores, aliases),
no un destino de escritura.

Las copias locales 1:1 viven en archivos que controla el equipo interno. Primero un **piloto** en
`(Sandbox) USS - Fundamentos de diseño (Copy)` (`lBG9RxtvvaBiSfx78Sd472`). Tras confirmación humana:
USS One Fundamentos (`w2FNtlyzRgJtrzkw7ywZHj`) y Ext. Library Fundamentos (`sDv64Fnh1bMxJXMOlTTZf8`).

Tipografía: el kit no tiene variables nativas (156 TEXT styles hardcoded). En los destinos se **crea**
colección `Typography` (modos Desktop/Mobile) y se bindean estilos. No es transferencia 1:1 de vars.

Estado final de colecciones en cada destino: `Space` + `Radius` + `Color` (1:1 kit) y `Typography`
(conversión). Tras `import-tokens-figma`, borrar **por ID** las colecciones extra: homónimas
(segunda `Color` / `Radius`) y leftovers `*-Copy-Originated`. Nunca `find(name)` + `remove()`
(borraría el snapshot: `find` es la primera homónima). Colecciones de producto con nombre único
(`Device`, `Elevacion`, …) no se tocan. Variables Figma-only **dentro** de la colección keep
(p.ej. `Radius-1000`) no se purgan.

Piloto 3 (2026-09-29, sandbox `lBG9RxtvvaBiSfx78Sd472`): borrados `Space-Copy-Originated` y
`Radius-Copy-Originated`. Quedan 4 colecciones: Space 19, Radius 6, Color 144, Typography 152.

## Por qué

Unificar tokens divergentes sin depender del dueño del kit, y poder extender la marca internamente.
Vincular la librería publicada dejaría tokens de solo lectura. Modelo: `export-tokens-figma` →
`import-tokens-figma` (aliases en dos pases). Bind PAINT/TEXT vía `figma-use` (no hay skill southleft).

## Consecuencias

- Alinea “no editar el core; additions en One / Ext. Library”.
- Cero writes a One/Ext hasta GATE del piloto **incluyendo** bind PAINT plano (ADR 2026-09-29)
  y delete de extras (piloto 3 sandbox).
- Producción: import → borrar extras por ID. Riesgo de unbind en IDs borradas: aceptado, sin
  rename previo ni rebind nodo a nodo.
- No usar JSON resuelto de ModUSS como input de import.
- Desktop/Mobile (archivos) fuera de esta migración; modos Desktop/Mobile son de Typography.
