# 2026-09-02 — Memoria persistente en el repo

Status: **aceptada**.

## Qué

`AGENTS.md` + `context/` + `decisions/` + `state/` + `skills/` + `gotchas/` + `logs/` como memoria del agente. El skill Figma **no** vive en este repo (southleft; ver ADR community-skill-source). Dumps en `output/` (gitignored).

## Por qué

El repo era 3 archivos de producto. FileKeys USS, access view-only, truncación MCP, timeout de páginas y el contrato vs ModUSS vivían solo en chats (`Figma design context extraction`, ModUSS). Cada sesión redescubría el runtime.

## Consecuencias

- Arranque = `AGENTS.md` + `state/current.md`, no transcripts.
- `inventory.js` upstream y dumps no se cargan enteros por defecto.
- Cierre de sesión importante actualiza `state/`, ADRs y `logs/`.
