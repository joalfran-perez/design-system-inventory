# TEXT style no tiene fill

**Síntoma:** bind de `Color Tokens/Text interactive` en estilos `Desktop|Mobile/Contenido/a…` no cambia el color en el canvas.

**Causa:** `TextStyle` en este runtime no expone `paints`/`fills` (36/36 `stylesNoPaint`). El estilo solo carga type (fuente, tamaño, underline). El color vive en el **nodo**.

**Qué hacer:** `setBoundVariableForPaint` en nodos TEXT que usan ese `textStyleId`. Recorrer la página (o páginas) de specimens.

**No:** asumir que reaplicar `textStyleId` restaura color; no hace undo de fills.
