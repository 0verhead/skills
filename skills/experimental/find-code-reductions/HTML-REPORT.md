# HTML Report Format

Render the scan as one HTML file in the OS temp directory. Load Tailwind and Mermaid from CDNs. Use Mermaid for dependency and call graphs; use hand-built divs and SVG for ownership mass, deletion, and interface depth.

Keep it editorial, visual, sparse, and opinionated. Show the **maintenance surface** the repository owns before and after.

## Scaffold

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Code reductions — {{repo name}}</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
      import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
      mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });
    </script>
    <style>
      .owned { background: #fff7ed; border-color: #fdba74; }
      .kept { background: #f0fdf4; border-color: #86efac; }
      .deleted { opacity: 0.38; text-decoration: line-through; }
      .seam { stroke-dasharray: 4 4; }
      .leak { stroke: #dc2626; }
    </style>
  </head>
  <body class="bg-stone-50 font-sans text-slate-900">
    <main class="mx-auto max-w-6xl space-y-12 px-6 py-12">
      <header>...</header>
      <section id="candidates" class="space-y-10">...</section>
      <section id="reviewed-and-kept">...</section>
      <section id="top-recommendation">...</section>
    </main>
  </body>
</html>
```

## Header

Repo, scope, date, and the sources that define preserved behaviour. Add a compact legend: amber = repository-owned maintenance, emerald = retained behaviour, faded = deleted, dashed line = seam, red arrow = leaked knowledge. No introduction paragraph.

## Candidate card

Each candidate is one `<article>`:

- **Title** — name the reduction, not the smell.
- **Badges** — recommendation strength plus lever (`delete`, `reuse`, `deep module`, `consolidation`, `native interface`, `dependency`, `stronger model`, `test-surface lift`).
- **Files** — monospaced paths.
- **Before / After visual** — the centrepiece.
- **Owned today** — short bullets: implementation, interface, states, tests, configuration, failure modes.
- **Reduction** — what replaces the owned surface.
- **What disappears** — concrete symbols, seams, states, tests, or files.
- **What remains ours** — domain rules and feature proof.
- **Evidence / decision** — code references, package sources, and any semantics or compatibility the user must accept.
- **Wins** — ≤6 words each, stated through locality, leverage, depth, and ownership removed.

No candidate may say only “cleaner,” “more maintainable,” or “fewer lines.” Show what the repository stops owning.

## Diagram patterns

Pick the pattern that fits; vary the report.

### Ownership mass

Before: a tall amber stack of custom implementation, interface, glue, tests, and failure modes. After: domain rules remain in emerald; transferred or deleted behaviour fades.

### Deepening collapse

Before: shallow modules with wide interfaces and tests crossing each seam. After: one deep module; former helpers appear faded inside its implementation.

### Package transfer

Before: repository boxes for parser, formatter, scheduler, locale rules, and edge-case tests. After: one maintained dependency, direct use or a seam justified by real variation, and retained domain tests. Any adapter must be visibly smaller than the behaviour transferred.

### State-space reduction

Before: invalid states plus validation, errors, and tests. After: type, schema, model, or input shaping prevents those states; removed branches fade.

### Test-surface lift

Before: tests attached to private helpers and call order. After: fewer tests attached to the module interface or user-visible feature, with domain-boundary tests retained.

### Protocol consolidation

Use Mermaid when several callers repeat an ordering-sensitive protocol. After: callers cross one seam into one deep implementation.

### Arrangement repair

Before: one concept scattered across unrelated paths. After: the concept is gathered under one purpose-named path, using the story and concatenation tests from `/codebase-design`.

## Package evidence

Use one compact line:

```text
current release · maintenance · license · dependencies · runtime/framework/SSR fit · advisory status · bundle implication
```

Link primary sources. Mark unknowns; do not infer them.

## Reviewed and kept

A compact table of tempting reductions and why they fail the ownership test. Examples: one-caller package seams, decimal objects while persistence remains Float, or deleting transaction tests because an input package was added.

## Top recommendation

One card: candidate, one sentence naming ownership removed, why it wins on locality and leverage, and an anchor link. Do not rank by estimated lines.

## Style and tone

Editorial, not dashboard-like. Stone/slate base; amber for owned maintenance, emerald for retained behaviour, red for leakage. Sparse prose; redraw instead of explaining at length.

Use `/codebase-design` vocabulary exactly: module, interface, implementation, depth, deep, shallow, seam, adapter, arrangement, story, leverage, locality, discoverability.

Phrasings that fit:

- “Three callers own the same protocol.”
- “Deepen: one interface owns ordering and races.”
- “The package absorbs locale and caret correctness.”
- “The model removes two states and six tests.”
- “Keep: the adapter preserves all existing glue.”

Avoid package popularity as evidence, line-count claims without deletions, and fewer tests as a goal by itself.
