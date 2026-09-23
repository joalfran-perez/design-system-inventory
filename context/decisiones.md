# Índice de decisiones de diseño

ADRs en `decisions/`. Este archivo es el índice; no duplicar el razonamiento.

| Fecha | ADR | Status | Una línea |
|-------|-----|--------|-----------|
| 2026-08-31 | [one-call-plugin-inventory](../decisions/2026-08-31-one-call-plugin-inventory.md) | aceptada | Un `use_figma`; Plugin API; aliases `{path}`; sin Desktop Bridge / REST Enterprise |
| 2026-08-31 | [verbosity-and-page-scope](../decisions/2026-08-31-verbosity-and-page-scope.md) | aceptada | VERBOSITY + INCLUDE + filtros; loop de páginas con `PAGE_NAMES` (≠ fan-out ModUSS) |
| 2026-08-31 | [visualspec-shallow](../decisions/2026-08-31-visualspec-shallow.md) | aceptada | visualSpec = raíz + L1; hex CSS; no recursor |
| 2026-09-02 | [persistent-memory](../decisions/2026-09-02-persistent-memory.md) | aceptada | Memoria en repo; `logs/` y `output/` locales |
| 2026-09-02 | [community-skill-source](../decisions/2026-09-02-community-skill-source.md) | aceptada | Sin fork: southleft es canónico; overlay en `skills/` |
| 2026-09-23 | [install-skill-project-scope](../decisions/2026-09-23-install-skill-project-scope.md) | aceptada | Skill instalado vía `npx skills` en `.agents/skills/`, alcance proyecto, versionado |

## Criterios (no reabrir sin ADR)

1. Inventario unificado tokens+components+styles en **una** llamada, no N scripts ad hoc.
2. Payload se controla **antes** del run (constantes inlined), no “dump full y ya veremos”.
3. Profundidad visual acotada; skills hermanos para profundidad/CSS SM/export a disco.
4. Este repo no almacena el inventario USS resuelto (eso es ModUSS).
5. No vendor ad hoc del skill comunitario (copiar/pegar el script fuera del mecanismo de skills). Instalación oficial en `.agents/skills/` sí está permitida — ver `install-skill-project-scope`. Desviación USS = overlay + ADR, no copia de `inventory.js`.

Abiertas: ninguna de contrato. Pendiente operativo: `state/current.md`.
