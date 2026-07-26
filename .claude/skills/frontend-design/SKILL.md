---
name: frontend-design
description: Frontend design guidance for the TREERA Ecosystem site — design tokens, typography, theming, Tailwind workflow, and verification steps. Use when creating or modifying any page, section, component, or style in this repo.
---

# TREERA Frontend Design

You are working on the TREERA Ecosystem site: a static, dark-first, nature/impact-branded site built with plain HTML + Tailwind CSS v3 (compiled, no CDN). Pages: `index.html` (landing, "The Impact Ecosystem"), `treex.html` (Treera Portal), `gizlilik.html` (privacy / KVKK — Turkish legal text).

## Stack & build

- Tailwind v3 compiled from `src/input.css` into `styles.css`. **Never edit `styles.css` by hand** — after adding new utility classes to HTML, rebuild:
  ```bash
  npm run build:css
  ```
  (`npm run watch:css` for live rebuild.) `tailwind.config.js` scans `./**/*.html`.
- Page-specific custom CSS lives in each page's inline `<style>` block, not in `src/input.css`, unless it is shared across pages.
- No JS framework. Vanilla JS in inline `<script>` tags. Keep it that way — do not introduce React/Vue/bundlers.

## Design tokens (source of truth: `:root` in `index.html`'s `<style>` block)

The site is **dual-theme via `data-theme` on `<html>`** (`dark` is default). All colors MUST go through CSS variables — never hardcode hex colors in new markup or CSS.

| Variable | Dark | Light | Role |
|---|---|---|---|
| `--bg` / `--bg2` / `--bg3` | `#070A08` / `#0B1410` / `#020503` | `#F5F8F5` / `#EDF2EE` / `#E2EBE3` | backgrounds |
| `--text` / `--text2` / `--text3` | `#F5F5F0` / `#A8A89E` / `#1C3D2A` | `#0F1F14` / `#1E3B26` / `#558763` | text hierarchy |
| `--moss1` / `--moss2` | `#0F2417` / `#1C3D2A` | `#C8E0CC` / `#A8C8AF` | brand surfaces |
| `--accent2` | `#5AAF78` | `#123A1F` | primary accent (CTAs, highlights) |
| `--glass-bg`, `--glass-border` | translucent | translucent | glassmorphism panels |
| `--border`, `--border2`, `--glow` | translucent | translucent | hairlines, glows |

Every new component must look correct in **both** themes — check both before calling it done. If a page lacks these tokens, copy the `:root` block from `index.html`.

## Typography

Google Fonts, loaded per-page with `preconnect`:

- **Cormorant Garamond** (`font-display`) — display serif for headlines and editorial moments. Light weights (300–500), often italic, large sizes, tight leading (`leading-[1.05]`), negative tracking (`tracking-[-.02em]`).
- **DM Sans** (`font-sans`) — body text, UI. Default on `<html>`.
- **DM Mono** (`font-mono`) — labels, kickers, data. Used small (10–13px) with wide tracking (`tracking-[0.2em]`/`[0.25em]`) and `uppercase` for section kickers/eyebrows.

Established scale patterns: hero `text-4xl sm:text-6xl md:text-8xl font-display`; section titles `text-3xl md:text-5xl font-display`; body `text-sm`/`text-[15px] leading-relaxed text-[var(--text2)]`; kickers `font-mono text-[11px] uppercase tracking-[0.25em]`.

## Visual language

- **Mood:** quiet luxury meets nature-tech. Dark forest tones, generous whitespace (`py-20`/`py-24` sections), thin hairline borders, subtle emerald glows. Restraint over decoration.
- **Glass panels:** `background: var(--glass-bg)` + `border: 1px solid var(--glass-border)` + backdrop blur.
- **Motion:** slow and soft — `duration-300`/`duration-500`, `ease-in-out`, opacity/translate reveals (`opacity-0 translate-y-12` → visible). Theme switch transitions at 0.5s. No bouncy or flashy animation. Respect `prefers-reduced-motion` for anything larger than a hover state.
- **Accent usage:** emerald (`--accent2`) is scarce by design — status dots, CTA hovers, small highlights. Never large emerald surfaces.
- **Layout:** `max-w-7xl mx-auto px-6` containers; 12-col grids on `lg:` (`lg:grid-cols-12` with 5/7 splits); mobile-first, stack with `flex-col` → `md:flex-row`.
- **Radii:** minimal — `rounded-[2px]`, `rounded-lg` at most; `rounded-full` only for dots/pills.

## Brand assets

Logo pack in `treera-logo-full/` (mark, stacked, horizontal, favicon, social 1080, 16:9 banner). Use these — never redraw the logo.

## Language & content

Public-facing copy is bilingual in spirit: `index.html` is English-led, `gizlilik.html` is Turkish (KVKK). Match the language already used on the page being edited. Do not machine-alter the legal text in `gizlilik.html` beyond markup/styling.

## Quality bar — The $10K Checklist

Before declaring any page or section done, walk these eight points and fix failures (TREERA already has 01–03 locked: dark nature-luxury direction, Cormorant/DM pairing, token palette — don't drift from them):

1. **Point of view** — stays committed to TREERA's quiet nature-luxury direction; nothing generic.
2. **Typography that does work** — display/body/mono roles used as defined above; headlines feel chosen.
3. **Restrained color** — tokens only, emerald accent stays scarce.
4. **Hierarchy that breathes** — clear primary/secondary/tertiary per screen; add space, not elements.
5. **Imagery with intent** — assets match the art direction; brand logo pack, inline SVG icons, no stock defaults.
6. **Motion that whispers** — slow, eased, purposeful; `prefers-reduced-motion` respected.
7. **Mobile designed, not shrunk** — layout re-decided at 375px; tap targets ≥ 44px; no horizontal scroll.
8. **Invisible expensive stuff** — sub-2s load, WCAG AA contrast, keyboard nav + focus states, semantic HTML, real meta/OG tags.

## Workflow for any design change

1. Read the target page's inline `<style>` block first — tokens and page-specific rules live there.
2. Build with existing Tailwind utilities + CSS variables; add inline `<style>` rules only for what utilities can't express.
3. Rebuild CSS if new utility classes were added (`npm run build:css`).
4. Verify in the browser at desktop (1280) **and** mobile (375) widths, in **both** themes. No horizontal scroll, readable contrast (WCAG AA: body text ≥ 4.5:1), visible focus states on interactive elements.
5. Keep pages self-contained — no new external dependencies (CDNs, icon fonts, JS libs) without explicit approval. Inline SVG for icons.
