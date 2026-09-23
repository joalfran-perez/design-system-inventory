# 2026-09-23 — Instalar el skill comunitario como skill real de Cursor (alcance proyecto)

Status: **aceptada**.

## Qué

`design-system-inventory-figma` (southleft/skills-for-figma) ya no se inyecta como script upstream en
cada `use_figma`. Se instaló como skill de Cursor con alcance de **proyecto**, vía el CLI oficial:

```
npx skills add southleft/skills-for-figma --skill design-system-inventory-figma -a cursor -y
```

El CLI clonó el repo, copió el skill (no symlink; decisión propia del CLI en este entorno Windows, no
fue necesario el fallback `--copy`) a `.agents/skills/design-system-inventory-figma/`, con los 3
archivos reales: `SKILL.md`, `scripts/inventory.js`, `references/visual-spec.md`. Cursor descubre esa
ruta de forma nativa (confirmado en [cursor.com/docs/context/skills](https://cursor.com/docs/context/skills));
queda versionada en este repo (no se agrega a `.gitignore`). El CLI también generó `skills-lock.json`
en la raíz (fuente + hash del skill instalado) — lockfile propio de la herramienta, se usa para
reinstalar/actualizar; no se edita a mano.

## Por qué

- Antes de esto, el skill comunitario no estaba resuelto por `skillNames` en `use_figma` (ese parámetro
  es solo un log; no auto-inyecta el script) — la única forma de correr el inventario era pegar el JS
  de `inventory.js` a mano en cada call.
- Se investigaron 3 caminos: (a) índice de skills oficiales del MCP de Figma → no incluye skills
  comunitarios; (b) import de marketplace de Cursor (`.cursor-plugin/marketplace.json`) → el repo
  `southleft/skills-for-figma` no lo tiene; (c) el CLI `npx skills`.
- El propio desarrollador (mismo texto en `southleft/skills-for-figma` y en el repo canónico
  `southleft/figma-console-mcp-skills`) recomienda explícitamente el CLI como "fastest path" para
  Cursor, Claude Code, Codex y Gemini CLI por igual — sin necesidad de copiar archivos a mano.
- Alcance de proyecto (no global) porque el skill es parte del contrato de este repo (fileKeys, USS,
  vs ModUSS) y debe viajar con él, versionado y compartido con el equipo — no es una herramienta
  personal del usuario.
- Se evaluó migrar a la fuente canónica `figma-console-mcp-skills` (nombre `figma-design-system-inventory`);
  el usuario decidió explícitamente mantener `skills-for-figma` / `design-system-inventory-figma` para
  no forzar un renombrado en cascada de `AGENTS.md`, ADRs y el overlay ya escritos. Ambos repos son
  byte-idénticos en el contenido de este skill (el canónico solo agrega 4 skills REST/token que no se
  usan aquí).

## Consecuencias

- **Reabre parcialmente** [`decisions/2026-09-02-community-skill-source.md`](2026-09-02-community-skill-source.md):
  esa ADR prohibía "vendor" (copiar el script a la raíz del repo, fuera del mecanismo de skills). Esa
  prohibición **se mantiene** para copias ad hoc pegadas al chat o guardadas sueltas. La instalación en
  `.agents/skills/` es una acción distinta y sancionada por el propio Cursor — no es un fork silencioso,
  es la ruta oficial de distribución de skills, con su propio origen (`southleft/skills-for-figma`)
  trazable en el historial de git.
- `use_figma` con `skillNames: "design-system-inventory-figma"` ahora resuelve el skill real (Cursor lo
  carga progresivamente); ya no hace falta fetch manual a GitHub como camino principal. Ese fetch queda
  como *fallback* si la instalación local llegara a romperse o quedar desactualizada.
- El default upstream inseguro (`VERBOSITY = "full"`, `PAGE_NAMES = []`) sigue sin parchearse: se acota
  en cada call, igual que antes.
- Overlay (`skills/design-system-inventory-figma.md`), `context/design.md` y el nombre del skill no
  cambian.
