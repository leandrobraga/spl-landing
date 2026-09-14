# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Landing page for SPL (Sistema de Processo Legislativo) — B2G product selling legislative-process software to Brazilian municipal chambers (Câmaras Municipais). Portuguese-language marketing copy. Not a git repo, no build tooling, no package.json — a static single-page site meant to be opened/served directly.

## Architecture: this is a Claude Design Canvas (`.dc`) artboard

`index.html` is NOT plain HTML — it's a Design Canvas artboard rendered by a small React-based runtime:

- `support.js` is a **generated** runtime (header says `GENERATED from dc-runtime/src/*.ts — do not edit. Rebuild with 'cd dc-runtime && bun run build'`). The `dc-runtime` source isn't in this repo — treat `support.js` as vendored/off-limits.
- `index.html` structure:
  - `<x-dc><helmet>...</helmet><div>...markup...</div></x-dc>` — the template. `<helmet>` holds font preconnects and global `<style>`; everything else is the page body.
  - A trailing `<script type="text/x-dc" data-dc-script data-props="...">` block defines a `class Component extends DCLogic` with `state`, lifecycle methods (`componentDidMount`/`componentWillUnmount`), and a `renderVals()` method returning the values interpolated into the template.
  - The `data-props` attribute (URL-escaped JSON) declares the artboard's editable props schema (editor type, default, TS type, grouping "section") — this is what a design-tool properties panel would expose. Current props: `headlineVariant` (enum A/B/C), `whatsappUrl` (text), `showOpenBlocks` / `showScarcity` (booleans).
- Template syntax to know when editing markup:
  - `{{ expr }}` interpolates a value from `renderVals()` (or a prop) into HTML text/attributes.
  - `<sc-if value="{{ cond }}" hint-placeholder-val="{{ true }}">...</sc-if>` conditionally renders a block; `hint-placeholder-val` is only a design-time placeholder hint, not runtime logic.
  - `style-hover="..."` on an element declares hover-state CSS handled by the runtime (there is no real `:hover` pseudo-class support in inline styles here).
  - Event props (`onClick`, `onTouchStart`, `onTouchEnd`, etc.) bind to functions returned from `renderVals()`.
- All styling is inline `style="..."` attributes (plus the one global `<style>` block in `<helmet>`) — there is no external CSS file and no utility-class framework. Keep new markup consistent with this pattern rather than introducing a stylesheet.
- Responsive behavior (mobile vs. desktop nav/CTA) is driven by JS (`state.narrow`, a `window.innerWidth < 900` check + resize listener) rather than CSS media queries, and exposed to the template as `isNarrow`/`isWide` via `renderVals()`.

## Content notes specific to this artboard

- Headline copy has 3 A/B/C variants gated by `isA`/`isB`/`isC` (driven by the `headlineVariant` prop) — when editing hero copy, check whether the change should apply to one variant or all three.
- Several sections are wrapped in `sc-if value="{{ showOpenBlocks }}"` / `showScarcity` — these are explicitly marked placeholder/TODO content in the copy itself (e.g. "A INSERIR", "RESERVADO", "BLOCO A DEFINIR", bracketed `[INSERIR ...]` FAQ answers). Don't fill these in with invented facts (pricing, support SLA, testimonials, named authority figures) — leave the placeholders or ask for the real content.
- Images referenced from `assets/` are the ones actually used on the page (`spl-logo-horizontal.svg`, `spl-simbolo-branco.svg`, `spl-painel.png`, `spl-voto.png`, `spl-tela-vereador.png`). `uploads/` contains additional brand assets (logo variants, palette PDF) not currently wired into the page — check there before asking for new assets.

## Working in this repo

- No build/lint/test commands exist. Verify changes by opening `index.html` directly in a browser (it loads `support.js` via relative `<script src>`, so serve or open from this directory; no dev server is configured).
- Since there's no framework tooling, be extra careful hand-editing the `data-props` JSON blob in the trailing `<script>` tag — it's URL/HTML-entity-escaped JSON (`&quot;`) and must stay valid JSON after unescaping, and its keys must match what `renderVals()`/the template actually reference.
