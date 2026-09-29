# TEXT style ↔ variable modes

**Síntoma:** estilos `Desktop/…` y `Mobile/…` bindeados a la misma variable `Type/…` con modos Desktop/Mobile. En el panel de estilos ambos muestran el valor del modo **default** (Desktop).

**Causa:** `TextStyle` no tiene `setExplicitVariableModeForCollection` (eso es de nodos). El bind apunta a la variable; el modo lo resuelve el consumidor (frame/página).

**Qué hacer:** esperado. En el canvas, setear el modo Typography del frame para ver Mobile. No duplicar variables `Type/Desktop/…` vs `Type/Mobile/…` salvo ADR nuevo.

**No:** reescribir literales en el estilo para “forzar” Mobile en el panel; el fallback del estilo no sustituye el modo.
