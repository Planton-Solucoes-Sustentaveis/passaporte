# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**Passaporte Digital do Produto** — A Digital Product Passport (DPP) showcase for Faber-Castell Grip 2011, built by Planton Soluções Sustentáveis. Demonstrates EU regulatory requirements (Green Deal / ESPR) for product environmental transparency, including carbon footprint, material traceability, and supplier chain visibility.

Repo: `https://github.com/Planton-Solucoes-Sustentaveis/passaporte`

## Stack

Zero build pipeline. No package manager, no transpiler, no bundler.

- `index.html` — 1,400+ lines: all HTML structure + embedded JS
- `style.css` — 1,400+ lines: all styling + animations
- `assets/` — SVG/PNG product images, maps, logos
- `anteriores/` — archived earlier versions (not active)

Open `index.html` in any browser to run locally.

## Architecture

Single page with 6 sections: Hero → Carbon → Journey → Materials → Highlights → Annexes.

**State management** is DOM-only: CSS classes (`.open`, `.active`, `.show`, `.is-active`) and `data-*` attributes control all UI state. No framework, no reactive state.

**JavaScript organization** (all inline at bottom of `index.html`):
- Theme toggle — CSS variable swap via `data-theme` on `<html>`
- Language system — `data-i18n` attribute lookup in embedded translation object (PT/EN/ES/DE)
- Command palette — Cmd+K / Ctrl+K, fuzzy search, keyboard nav (↑↓ Enter Esc)
- JSON modal — renders `passportJson` with syntax highlighting
- Carbon breakdown — click SVG segments → detail panel
- Journey accordion — step expand/collapse, arrow key navigation
- Supply chain map — generates Bezier SVG curves between origin points and destinations
- Scroll animations — `IntersectionObserver` adds `.in` class to `.reveal` / `.reveal-stagger` elements
- Carbon counter — animated number tick on first intersection
- Parallax tilt — pencil image tracks mouse position via `requestAnimationFrame`
- Magnetic hover — CTA buttons pull toward cursor

**Data** is a hardcoded JS object `passportJson` near bottom of `index.html`:
```
schema, product, carbon (breakdown by phase), materials (6 items), highlights, disclaimer
```

**Theming** — CSS custom properties on `:root` and `[data-theme="dark"]`. Forest green (`#005C35` / `#3DAA72`) + warm gold (`#C8A030`) brand palette.

**Responsive** — single breakpoint at 779px. SVG map viewBox crops on mobile.

## Conventions

- Class names mix BEM-like descriptors (`.carbon-segment`, `.journey-step`) with state classes (`.open`, `.active`)
- `data-i18n="key"` on any element that needs translation
- `data-step`, `data-idx`, `data-target`, `data-iso`, `data-lang` for JS hooks — never use inline `onclick`
- Animations via CSS transitions (0.15–0.35s) + JS-added classes; no `setTimeout` for visual state
- Accessibility: ARIA labels, keyboard navigation, `prefers-reduced-motion` respected

## Workflow

Edit HTML/CSS directly. No build step. Reload browser to see changes. Git push to deploy.
