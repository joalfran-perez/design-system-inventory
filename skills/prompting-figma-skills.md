# Guía de prompting — skills de Figma instaladas

Cómo activar de forma confiable las 4 skills comunitarias instaladas en `.agents/skills/`
(`design-system-inventory-figma`, `deep-component-figma`, `analyze-component-set-figma`,
`export-tokens-figma`). No es un tutorial de Figma; es la mecánica de invocación de Cursor.

## Cómo Cursor realmente las invoca (2 mecanismos, no 1)

Confirmado en [cursor.com/docs/context/skills](https://cursor.com/docs/context/skills):

1. **Automático (por defecto)** — al iniciar, Cursor expone todas las skills instaladas junto con su
   `description` al agente. El agente decide **semánticamente** si el mensaje del usuario coincide con
   el propósito de alguna — no es un match de substring literal, es juicio del modelo leyendo la
   descripción completa. Las 4 skills tienen `disable-model-invocation: false` → esta vía está activa.
2. **Manual** — el usuario escribe `/nombre-skill` en el chat (p. ej. `/deep-component-figma`). Se
   adjunta solo a ese mensaje. Usar cuando el pedido es ambiguo entre 2+ skills (ver tabla de
   disambiguación) o cuando el match automático no ocurrió.

**Lo que NO hace nada:** el parámetro `skillNames` dentro de una llamada a `use_figma` no autoresuelve
ni inyecta la skill — es solo una anotación de log/telemetría. La skill se carga **antes** de llamar a
`use_figma` (por 1 o 2 arriba); una vez cargada, el agente adapta el script propio de la skill dentro
del `use_figma` call. Pasar `skillNames: "design-system-inventory-figma"` sin que la skill se haya
cargado primero no tiene efecto.

## Frases-gatillo verbatim (del `description` de cada `SKILL.md`)

El agente lee el `description` completo, no solo estas frases — pero son las que su propio autor
identificó como representativas. Usarlas (o parafrasearlas conservando los sustantivos técnicos) sube
la probabilidad de match automático.

| Skill | Cuándo usarla | Triggers (inglés, verbatim) |
|-------|---------------|------------------------------|
| `design-system-inventory-figma` | Kit completo de un archivo: tokens + components + styles en una pasada | "inventory my design system", "extract all tokens and components", "what's in this design system", "dump the design system kit", "give me every variable, component, and style", "build a design-system manifest", "audit my Figma library" |
| `deep-component-figma` | Un componente/nodo, árbol recursivo sin límite, tokens resueltos, reactions, instancias anidadas | "get the full component tree", "deep extract this component", "give me everything about this node for code gen", "resolve all the tokens in this component", "what instances does this component nest", "extract reactions/prototype links", "high-fidelity component spec" |
| `analyze-component-set-figma` | Un `COMPONENT_SET` → máquina de estados CSS (variant axes → pseudo-clases + diffs por variante) | "analyze this component set", "turn these variants into CSS states", "generate a button/input/checkbox from Figma variants", "what changes between the hover and default state", "map Figma variants to component props", "extract the state machine for this component" |
| `export-tokens-figma` | Variables de Figma → archivos de tokens en disco (DTCG, CSS, Tailwind, SCSS, TS, JSON, Style Dictionary, Tokens Studio) | "export tokens", "export Figma variables", "generate CSS variables from Figma", "turn my Figma variables into a tokens.json / Tailwind config / SCSS", "sync design tokens to code" |

Parafrasear en español funciona porque el match es semántico, no textual — pero conviene incluir el
sustantivo técnico ancla: "árbol"/"tree" → `deep-component-figma`; "variant"/"estado"/"CSS" →
`analyze-component-set-figma`; "tokens a disco"/"exportar variables" → `export-tokens-figma`;
"inventario"/"todo el archivo"/"auditoría" → `design-system-inventory-figma`.

## Disambiguación (solapamiento léxico real)

Estos pares comparten vocabulario ("extract", "tokens", "component") y pueden matchear la skill
equivocada si el pedido es vago:

| Pedido ambiguo | Puede matchear | Cómo desambiguar |
|----------------|-----------------|--------------------|
| "extract this component" | `deep-component-figma` **o** `design-system-inventory-figma` | Si es **un** nodo con árbol/tokens/reactions → `deep-component-figma`. Si es **todo el archivo** → `design-system-inventory-figma`. Si sigue ambiguo, usar `/deep-component-figma` explícito. |
| "give me the tokens" | `export-tokens-figma` **o** `design-system-inventory-figma` (su bloque `tokens`) | Si el destino es **un archivo en disco** (CSS/Tailwind/DTCG) → `export-tokens-figma`. Si es solo **inspeccionar** valores dentro de un kit más amplio → `design-system-inventory-figma`. |
| "analyze this component" (sin aclarar si es un set o una instancia) | `analyze-component-set-figma` **o** `deep-component-figma` | Si el nodo es un `COMPONENT_SET` con variantes de estado → `analyze-component-set-figma`. Si es un nodo único que quieres aplanar a código → `deep-component-figma`. |

Regla general: si el pedido nombra explícitamente "component set" / "variantes" / "estados CSS" → casi
siempre `analyze-component-set-figma`. Si nombra "todo el sistema" / "inventario" / "auditoría" →
`design-system-inventory-figma`. Ante duda real, preferir `/skill-name` explícito sobre confiar en el
match automático.

## Runtime común a las 4

Todas requieren cargar primero el skill oficial **`figma-use`** (Plugin API: plain JS, top-level
`await` + `return`, sin IIFE, sin `figma.closePlugin()`, errores atómicos). Ver `AGENTS.md` invariante 1.

`export-tokens-figma` tiene un paso extra: su conversor (`scripts/convert-tokens.mjs`) corre en Node
fuera de Figma, no dentro de `use_figma` — necesita terminal (disponible aquí, `node v22.19.0`).
