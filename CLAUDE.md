# CLAUDE.md

Guidance for Claude Code in this repo.

## What this is

Landing page for SPL (Sistema de Processo Legislativo) — B2G product selling legislative-process software to Brazilian municipal chambers (Câmaras Municipais). Portuguese-language marketing copy. Not a git repo tool-wise beyond plain git, no build tooling, no package.json — static single-page site, open/served directly.

## Architecture

Plain static HTML/CSS/JS. No framework, no runtime, no generated files.

- `index.html` — whole page. Standard `<html><head><body>`, all styling inline `style="..."` attributes plus one global `<style>` block in `<head>` (mostly hover states and the mobile media query). No external CSS file, no utility-class framework — keep new markup consistent with inline styles.
- One `<script>` at the end of `index.html` (vanilla JS, IIFE): sets the WhatsApp link href on all `.whatsapp-link` elements, and drives the image carousel (slide state, dots, touch swipe, autoplay).
- Responsive behavior is plain CSS: `@media (max-width: 899px)` in the `<style>` block toggles `.nav-desktop` / `.nav-mobile-cta` / `.mobile-bar`. No JS width checks.
- `assets/` — images actually used on the page (logo, símbolo, painel/voto/tela screenshots).
- `uploads/` — extra brand assets (logo variants, palette PDF) not wired into the page. Check there before asking for new assets.

## Content notes

- WhatsApp number is a placeholder: `WHATSAPP_NUMBER = "55SEUNUMERO"` near the top of the `<script>`. Swap in the real number (digits only, `55` + DDD + number) when known.
- Several spots are explicit placeholder/TODO copy — marked in the text itself with `A INSERIR`, `RESERVADO`, `BLOCO A DEFINIR`, or bracketed `[INSERIR ...]`. Don't invent facts to fill these (pricing, support SLA, testimonials, named authority figures) — leave the placeholder or ask for the real content.

## Working in this repo

No build/lint/test commands. Verify changes by opening `index.html` directly in a browser (or serving the directory) — it's fully self-contained aside from Google Fonts and the local `assets/` images.
