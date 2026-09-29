# Pass de fills demasiado amplio

**Síntoma:** casi todos los TEXT de Ext Tipografía en `Color Tokens/Text/Subtle` (Sandbox sano: Neutral 100 / Strong).

**Causa:** script que trataba “cualquier fill cuyo id ≠ Subtle” como candidato y rebindeaba **todos** los rangos a Subtle.

**Qué hacer:** targetear `variable.id` o `variable.name` exactos. Restore: Strong + `Text interactive/Default` (ADR `2026-09-29-canvas-text-fill-strong-interactive`). `textStyleId = id` no recupera fill.

**No:** `if (fillId !== subtleId) bind(subtle)` sobre una página de documentación.
