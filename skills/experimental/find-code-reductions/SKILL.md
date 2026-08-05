---
name: find-code-reductions
description: Scan a codebase, pull request, or named scope for ways to reduce repository-owned maintenance. Present them as a visual HTML report, then grill the candidate the user picks.
disable-model-invocation: true
---

# Find Code Reductions

Find places where the team can **own less** without losing required behaviour. Count the full **maintenance surface**: implementation, interfaces, seams, configuration, states, tests, and failure modes the repository must keep correct.

Use `/codebase-design` whenever a reduction changes modules or interfaces: apply its vocabulary, deletion test, interface-as-test-surface rule, seam discipline, and replace-don't-layer principle. Architecture is one reduction lever, not the scope of this skill.

Read `CONTEXT.md` and relevant ADRs before treating domain behaviour as removable.

The governing test is:

> **The ownership test:** after the change, does the team own fewer rules, states, interfaces, tests, and failure modes — or did the proposal merely move them?

## Process

### 1. Explore

**Scope before you scan — YAGNI.**

- If the user names a PR, diff, module, path, dependency, or pain point, use it. For a PR, read the complete discussion and originating issue/specification; ratified behaviour is not reduction fodder.
- Otherwise, inspect `git log --oneline` for recurring hot spots and let those paths lead.
- Read repository instructions, coding standards, the domain glossary, and relevant ADRs first.

Use the Agent tool with `subagent_type=Explore`. Adapt the lenses to the scope rather than running a fixed ceremony:

- **Delete and reuse** — dead code, duplicated logic, needless helpers, empty adapters, repeated UI or query ceremony, generated behaviour, and existing modules that callers should reuse instead of reimplement.
- **Platform and dependencies** — prefer standard-library, platform, and framework interfaces, then existing dependencies. Use `.context` and primary sources to verify behaviour. Consider a new package only when it removes meaningful implementation after compatibility, security, maintenance, license, and remaining glue are accounted for.
- **Model and tests** — make invalid states unrepresentable; remove tests of private helpers, library behaviour, typing, or states the model now prevents. Preserve feature proof and domain-rule proof.
- **Architecture** — where it removes ownership, deepen shallow modules, consolidate protocols, shrink interfaces, remove speculative seams, improve locality, and repair arrangements that do not tell the system's story.

For a broad scope, explore independent lenses in parallel, then make one completeness pass for missed reduction modes.

Apply the deletion test and **replace, don't layer**. A package earns its place when maintained behaviour and its tests disappear; a wrapper that keeps the old parsing, validation, and glue has not reduced the maintenance surface. Keep internal seams private; expose one only when real variation justifies it.

Verify callers before claiming reuse. Include evidence-backed **keep** candidates where a tempting package or abstraction would increase ownership.

### 2. Present candidates as an HTML report

Write one HTML file outside the repository at `<tmpdir>/code-reductions-<timestamp>.html`. Resolve `<tmpdir>` from `$TMPDIR`, falling back to `/tmp` on Unix or `%TEMP%` on Windows. Open it with `open`, `xdg-open`, or `start` as appropriate, then report the absolute path.

Use Tailwind and Mermaid from CDNs, mixed with hand-built CSS/SVG. Every candidate needs a before/after visual of the maintenance surface.

For each candidate, show:

- **Files**
- **Owned today** — implementation, interface, states, tests, configuration, failure modes
- **Reduction** — deepened module, better arrangement, standard API, dependency, stronger model, or deletion
- **What disappears**
- **What remains ours** — domain rules and feature proof
- **Evidence / compatibility** — code references and package primary sources where applicable
- **Before / After diagram**
- **Strength** — `Strong`, `Worth exploring`, or `Speculative`

Include a compact **Reviewed and kept** section. End with one **Top recommendation**, chosen by net ownership removed, locality, leverage, and any discoverability gain — not estimated lines deleted.

Use domain vocabulary from `CONTEXT.md` and architecture vocabulary from `/codebase-design`. Do not propose detailed interfaces yet. See [HTML-REPORT.md](HTML-REPORT.md) for the scaffold and visual patterns.

After opening the report, ask: **"Which reduction would you like to explore?"**

### 3. Grilling loop

Once the user picks a candidate, run `/grilling` to test the preservation claim, what actually disappears, what remains domain-owned, and whether the replacement is a clean cut rather than another layer.

- Use `/codebase-design` and design-it-twice when a new or deepened interface is needed.
- Use `/research` or primary-source inspection when package suitability remains uncertain.
- Use `/domain-modeling` when the reduction changes domain language or rules.
- Offer an ADR when a rejected reduction has a load-bearing reason future scans would otherwise re-suggest.

This skill finds and sharpens reductions. It does not implement them automatically.
