# Estado actual

Actualizado: 2026-09-23.

## Hecho

- Memoria persistente + overlay USS: `skills/design-system-inventory-figma.md`.
- Fork local eliminado (`SKILL.md`, `scripts/inventory.js`, `references/visual-spec.md` = copia southleft).
- Contrato: una llamada Plugin API; verbosidad/páginas/filtros; visualSpec raíz+L1; aliases `{path}`.
- Git local (`master`); sin remote.
- Primer run documentado: USS core Fundamentos `16PDlIOKg8kb176dMz0Ckg` (edit OK). Nodo `1:89` = página Tipografía.
- Dump compacto: `output/16PDlIOKg8kb176dMz0Ckg-inventory.json` (gitignored). Log: `logs/2026-09-02-16PDlIOKg8kb176dMz0Ckg.md`.
- Pass `summary` tokens (169, aliases `{path}`) + TEXT styles resueltos (156, sin variables Figma ligadas). Dumps: `output/16PDlIOKg8kb176dMz0Ckg-tokens-summary.json`, `output/16PDlIOKg8kb176dMz0Ckg-text-styles.json`.
- **Skill comunitario instalado** (2026-09-23): `npx skills add southleft/skills-for-figma --skill design-system-inventory-figma -a cursor -y` → `.agents/skills/design-system-inventory-figma/` (`SKILL.md` + `scripts/inventory.js` + `references/visual-spec.md`, copia real, versionada). `use_figma` + `skillNames: "design-system-inventory-figma"` ya no requiere inyectar el JS a mano. ADR: `decisions/2026-09-23-install-skill-project-scope.md`. Log: `logs/2026-09-23-install-design-system-inventory-figma-skill.md`.

## Pendiente

1. Cuando el archivo esté en **edit** en MCP: aplicar `output/16PDlIOKg8kb176dMz0Ckg-style-variable-bindings.json` (205 binds + rebind Dark BG3). Tipografía: crear colección nativa **después** de aprobación.
2. Skills hermanos (southleft, no aquí): `deep-component-figma`, `analyze-component-set-figma`, `export-tokens-figma`.
3. (Opcional) puente a ModUSS: post-procesar return → `tokens/*.json` resuelto. No implementar sin pedido.
4. Validar extremo a extremo el skill instalado: correr `use_figma` con `skillNames: "design-system-inventory-figma"` contra un fileKey real y confirmar que resuelve sin inyectar JS manual.

## Blockers

MCP `use_figma` ~20KB: un shot `inventory` con Iconos (~1192 comps) + keys de variables **trunca**. No reintentar `full` en este file sin omitir Iconos.

**MCP write en Fundamentos core (`16PDlIOKg8kb176dMz0Ckg`): read-only.** Estilos locales (`remote: false`) pero `set_paints` / `set_name` fallan. Bindings estilo↔variable no se pueden aplicar hasta abrir el archivo con **edit** en la sesión Figma MCP. Mapa listo: `output/16PDlIOKg8kb176dMz0Ckg-style-variable-bindings.json`.

## Fuera de cola

- Consolidación USS One ↔ Extension Library (ModUSS).
- Binding Color / `tokens.json` (`uss-kit-digital`).
- Parchear defaults de southleft (`VERBOSITY = "full"`). Acotar en el call.

## Siguiente sesión

Reabrir Fundamentos core con **edit** en Figma MCP y aplicar bindings; o primer pass USS One Fundamentos (`w2FNtlyzRgJtrzkw7ywZHj`).
