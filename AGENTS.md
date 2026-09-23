# Design system inventory — AGENTS.md

Overlay USS sobre el skill comunitario `design-system-inventory-figma` (southleft). Extrae **tokens + components + styles** en un `use_figma`. No es ModUSS ni `tokens.json`. No es un fork del skill.

Fuente: https://github.com/southleft/skills-for-figma/blob/main/skills/design-system-inventory-figma/SKILL.md

## Identidad

Agente = orquestador USS del inventario (fileKeys, access, verbosidad, persistencia). No mantiene el JS de southleft. Prioridad: payload controlado, Plugin API, `visualSpec` compacto, cero dumps al chat, cero copia local del skill.

Consumidor típico de la salida: ModUSS (`C:\Users\Genesys\ModUSS`) post-procesa a JSON resuelto. El skill comunitario **conserva aliases** `{Token.Name}`; ModUSS resuelve a hex. No mezclar contratos.

## Reglas de oro (contexto)

- El context window es caro y volátil. La memoria real vive en archivos.
- Nunca cargar todo el historial ni todos los archivos del proyecto.
- Cargar solo lo estrictamente necesario para la tarea actual.
- Al final de cada sesión importante: actualizar `state/`, registrar decisiones y comprimir lo valioso en `logs/`.
- Preferir referenciar archivos antes que copiar contenido largo al prompt.
- Convertir procedimientos repetitivos en skills reutilizables.
- Mantener este `AGENTS.md` conciso (máx. ~300 líneas) y de alta densidad.

## Invariantes

1. Antes de `use_figma`: cargar skill oficial **`figma-use`**. JS plano, top-level `await` + `return`. Sin IIFE, sin `figma.closePlugin()`, sin `console.log` (no se retorna).
2. Errores atómicos: un script fallido no aplica nada — leer error, corregir, reintentar.
3. **No** `figma.loadAllPagesAsync()` (no existe en este runtime). Páginas: `setCurrentPageAsync`. Restaurar página original al final.
4. `VERBOSITY` / `INCLUDE` / `COMPONENT_NAME_FILTER` / `PAGE_NAMES` se inlinan **en el call** (script upstream). Default upstream inseguro: `full` + `PAGE_NAMES = []`. No editar el `inventory.js` instalado en `.agents/skills/` (es upstream, se actualiza reinstalando el skill, no a mano).
5. `visualSpec` = raíz + hijos directos (`childSpecs`). Árbol profundo / CSS SM / export disco → skills hermanos en **southleft**, no aquí.
6. Colores: hex mayúsculas, alpha drop. Aliases: `{path.con.puntos}`, no `VariableID` crudo.
7. No commitear dumps (`output/`), `logs/`, ni secretos. No commit/push salvo pedido explícito.
8. Archivos Figma view-only: `use_figma` falla aunque el script solo lea. Fallback MCP remoto — ver `gotchas/read-only-access.md`.
9. Nunca pegar un inventario `full` en el chat. Bajar verbosidad o escribir `output/` (gitignored).
10. No re-inventariar un file-set USS “por si acaso”. Chequear `state/current.md` y, si el destino es ModUSS, `ModUSS/state/inventories.md`.

## No cargar por defecto

| Recurso | Por qué | Cuándo sí |
|---------|---------|-----------|
| `inventory.js` instalado (`.agents/skills/design-system-inventory-figma/scripts/`) | Extractor estable de southleft, ya local | Cursor lo carga solo vía `skillNames`; no hace falta Read manual |
| `visual-spec.md` instalado (misma ruta, `references/`) | Tabla CSS del spec | Al mapear visualSpec → código; contrato local ya está en `context/design.md` |
| SKILL.md comunitario | Duplica este overlay | Instalado en `.agents/skills/design-system-inventory-figma/`; no leer el archivo completo a mano, Cursor lo resuelve por `skillNames` |
| Transcripts / canvas ModUSS | Historial crudo | Nunca. Usar `logs/` y `context/` |
| Inventarios ModUSS (`tokens/*.json`) | Datos de otro repo | Solo si el usuario pide cruzar extracción ↔ auditoría |
| `output/` | Dumps grandes | Grep/Read por path; nunca el archivo entero al prompt |

## Orden de lectura

Mínimo (toda sesión): este archivo + `state/current.md`.

Según tarea, **uno** de:

| Tarea | Leer |
|-------|------|
| Correr inventario | `skills/design-system-inventory-figma.md` + `figma-use` |
| Forma de `visualSpec` | `context/design.md`; tabla CSS → reference upstream |
| Arquitectura / archivos USS / vs ModUSS | `context/design.md` |
| Decisión / por qué | `context/decisiones.md` → ADR en `decisions/` |
| Error MCP / Figma | `gotchas/` (síntoma). No reintentar a ciegas |
| Continuar trabajo | `state/current.md` → último `logs/` |
| Cambiar contrato del extractor | No parchear southleft aquí. ADR + overlay; wait upstream o documentar desviación |

## Routing de skills

| Trigger | Usar |
|---------|------|
| Inventario entero de un archivo Figma | `use_figma` + `skillNames: "design-system-inventory-figma"` (skill instalado en `.agents/skills/`, Cursor lo resuelve solo) + `skills/design-system-inventory-figma.md` |
| Un componente, árbol ilimitado, reactions | `deep-component-figma` (southleft; no vendor) |
| Un variant set como state machine CSS | `analyze-component-set-figma` (southleft; no vendor) |
| Tokens a disco (Tokens Studio / JSON) | `export-tokens-figma` (southleft) o repo `uss-kit-digital` |
| Write Figma / variables / librería | skills Figma (`figma-use`, `figma-generate-library`, …) **después** de cargar el skill Figma |
| Auditoría USS ya extraída / diffs entre sistemas | Repo ModUSS + su `AGENTS.md`. No re-extraer desde aquí |
| Crear/editar skills Cursor | skill `create-skill` |
| Git commit / PR | reglas de usuario |

Si un procedimiento se usa ≥2 veces → archivo en `skills/` y puntero aquí.

## Punteros de memoria

| Path | Qué hay |
|------|---------|
| `context/design.md` | Contrato del skill, vs ModUSS, keys Figma USS |
| `context/decisiones.md` | Índice de ADRs |
| `decisions/` | ADRs fechados (qué / por qué / status) |
| `state/current.md` | Hecho / pendiente / blockers |
| `skills/` | Overlay USS (`design-system-inventory-figma.md`). Skill canónico = GitHub southleft |
| `.agents/skills/design-system-inventory-figma/` | Skill comunitario instalado (Cursor CLI, alcance proyecto). `SKILL.md` + `scripts/inventory.js` + `references/visual-spec.md` reales, versionados |
| `skills-lock.json` | Lockfile del CLI `npx skills` (fuente + hash del skill instalado). Reinstalar con el mismo comando actualiza hash; no editar a mano |
| `gotchas/` | Fallos conocidos + fix |
| `logs/` | Resúmenes de sesión **locales** (gitignored) |

## Comportamiento

1. Abrir sesión: `AGENTS.md` + `state/current.md`. No transcripts.
2. Scope: una tarea (correr inventario **o** cambiar extractor **o** documentar). No ambas sin pedido.
3. Antes de `use_figma` en archivo grande: bajar a `summary`/`inventory` o setear `PAGE_NAMES` / filtro. Ver `gotchas/page-scan-timeout.md`.
4. Resultado grande → no reintentar `full`. Bajar verbosidad o recortar `INCLUDE`.
5. Cierre de sesión importante:
   - `state/current.md`
   - ADR nuevo si cambió una regla
   - `logs/YYYY-MM-DD-<tema>.md` (local, ≤40 líneas)
6. Constantes van en el call. No dejar un fork de `inventory.js` en el working tree.

## Definition of Done

- [ ] Cambio acotado (extractor **o** memoria **o** un run)
- [ ] `use_figma`: `figma-use` cargado; script retorna objeto; sin IIFE / `closePlugin`
- [ ] Archivo grande: verbosidad/páginas acotadas; payload no volcado al chat
- [ ] Aliases sin `VariableID` crudo; hex en primitivos
- [ ] `visualSpec` sin recursor extra (salvo skill profundo)
- [ ] `state/` actualizado; ADR si cambió contrato
- [ ] Páginas restauradas (`setCurrentPageAsync` al original)

## Arranque de sesión (usuario)

Pegar: `Lee AGENTS.md y state/current.md. Continúa el pendiente.` + la tarea concreta (fileKey / URL Figma si aplica).
