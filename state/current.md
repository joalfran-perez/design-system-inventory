# Estado actual

Actualizado: 2026-09-23.

## Hecho

- Memoria persistente + overlay USS: `skills/design-system-inventory-figma.md`.
- Fork local eliminado (`SKILL.md`, `scripts/inventory.js`, `references/visual-spec.md` = copia southleft).
- Contrato: una llamada Plugin API; verbosidad/páginas/filtros; visualSpec raíz+L1; aliases `{path}`.
- Git: remote `origin` → [github.com/joalfran-perez/design-system-inventory](https://github.com/joalfran-perez/design-system-inventory) (público). Commit inicial `ebcc0ec`, rama `master`.
- Primer run documentado: USS core Fundamentos `16PDlIOKg8kb176dMz0Ckg` (edit OK). Nodo `1:89` = página Tipografía.
- Dump compacto: `output/16PDlIOKg8kb176dMz0Ckg-inventory.json` (gitignored). Log: `logs/2026-09-02-16PDlIOKg8kb176dMz0Ckg.md`.
- Pass `summary` tokens (169, aliases `{path}`) + TEXT styles resueltos (156, sin variables Figma ligadas). Dumps: `output/16PDlIOKg8kb176dMz0Ckg-tokens-summary.json`, `output/16PDlIOKg8kb176dMz0Ckg-text-styles.json`.
- **4 skills comunitarias instaladas** (2026-09-23, `npx skills`, alcance proyecto, mismo repo `southleft/skills-for-figma`): `design-system-inventory-figma`, `deep-component-figma`, `analyze-component-set-figma`, `export-tokens-figma` → `.agents/skills/<nombre>/` con `SKILL.md` + `scripts/` reales, versionados. `skills-lock.json` con las 4 entradas. ADR: `decisions/2026-09-23-install-skill-project-scope.md`. Logs: `logs/2026-09-23-install-design-system-inventory-figma-skill.md`, `logs/2026-09-23-install-sibling-skills-and-prompting-guide.md`.
- **Guía de prompting** `skills/prompting-figma-skills.md`: cómo Cursor invoca las 4 skills (match automático por `description` o `/skill-name` explícito — **no** por `skillNames` de `use_figma`, que es solo log), frases-gatillo verbatim y tabla de disambiguación entre skills con vocabulario solapado.

## Pendiente

1. Cuando el archivo esté en **edit** en MCP: aplicar `output/16PDlIOKg8kb176dMz0Ckg-style-variable-bindings.json` (205 binds + rebind Dark BG3). Tipografía: crear colección nativa **después** de aprobación.
2. (Opcional) puente a ModUSS: post-procesar return → `tokens/*.json` resuelto. No implementar sin pedido.
3. Validar extremo a extremo las 4 skills instaladas: probar el match automático (mensaje natural) y el explícito (`/skill-name`) contra un fileKey real, confirmar que no hace falta inyectar JS manual.

## Blockers

MCP `use_figma` ~20KB: un shot `inventory` con Iconos (~1192 comps) + keys de variables **trunca**. No reintentar `full` en este file sin omitir Iconos.

**MCP write en Fundamentos core (`16PDlIOKg8kb176dMz0Ckg`): read-only.** Estilos locales (`remote: false`) pero `set_paints` / `set_name` fallan. Bindings estilo↔variable no se pueden aplicar hasta abrir el archivo con **edit** en la sesión Figma MCP. Mapa listo: `output/16PDlIOKg8kb176dMz0Ckg-style-variable-bindings.json`.

## Fuera de cola

- Consolidación USS One ↔ Extension Library (ModUSS).
- Binding Color / `tokens.json` (`uss-kit-digital`).
- Parchear defaults de southleft (`VERBOSITY = "full"`). Acotar en el call.

## Siguiente sesión

Reabrir Fundamentos core con **edit** en Figma MCP y aplicar bindings; o primer pass USS One Fundamentos (`w2FNtlyzRgJtrzkw7ywZHj`).
