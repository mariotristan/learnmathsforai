# Visual Learning Path — Design Spec
Date: 2026-05-18

## Overview

Add a floating "RUTA" / "PATH" button to both the Spanish and English pages. Clicking it opens a full-screen overlay displaying all 14 sections organized into 4 named phases. The overlay is a static reference map — no navigation, no interactivity beyond closing it. Pressing Esc or clicking ✕ closes it.

## Button

- Fixed position: bottom-right corner, `right: 32px; bottom: 32px`
- Style: `DM Mono` font, 11px, letter-spacing 0.15em, uppercase
- Colors: gold border (`var(--accent)`), transparent background, gold text — matches existing filter buttons
- Labels: `RUTA →` (ES), `PATH →` (EN)
- On mobile (`max-width: 700px`): `right: 20px; bottom: 20px`

## Overlay

- Full-screen fixed backdrop: `background: rgba(13,15,20,0.92)`, `z-index: 100`
- Centered content panel: `max-width: 680px`, `background: var(--surface)`, `border: 1px solid var(--border)`, `border-radius: 4px`, `padding: 40px 48px`
- Close button: ✕ top-right of panel, `DM Mono`, muted color, hover turns gold
- Esc key also closes
- Scroll within panel on small screens (`max-height: 80vh; overflow-y: auto`)
- Open/close toggled by a single JS function; body scroll locked while open

## Phase Layout

Vertical stack of 4 phase rows inside the panel, with a thin connector line between each.

### Phase definitions

| # | ES label | EN label | Sections |
|---|---|---|---|
| ① | Fundamentos | Foundations | 00 Álgebra Lineal · 01 Geometría · 02 Cálculo · 03 Optimización |
| ② | Core ML | Core ML | 04 Probabilidad · 05 Estadística · 06 Teoría Información |
| ③ | Avanzado | Advanced | 07 Medidas · 08 Señales · 09 Grafos · 10 Geom. Diferencial |
| ④ | IA Moderna | Modern AI | 11 Transformers · 12 Difusión · 13 RL |

### Row structure (per phase)

```
[phase label pill]  ─────────────────────────────
  [chip] [chip] [chip] [chip]
```

- Phase label pill: colored background + border matching `section.accent` from the existing data, 10px `DM Mono`, uppercase
- Horizontal rule: 1px, `var(--border)`, flex-grows to fill the row
- Topic chips: `background: var(--tag-bg)`, `border: 1px solid var(--border)`, 9px `DM Mono`, same style as existing `.tag` elements
- Connector between rows: `width: 1px; height: 20px; background: var(--border); margin-left: 16px`

### Phase accent colors (from existing site palette)

| Phase | Color |
|---|---|
| ① Fundamentos | `#c8a96e` (gold) |
| ② Core ML | `#7eb8c9` (blue) |
| ③ Avanzado | `#a87ec9` (purple) |
| ④ IA Moderna | `#c97e7e` (red) |

## Implementation scope

- Edit `docs/index.html` (ES) and `docs/en/index.html` (EN)
- Add CSS for `.path-btn`, `.path-overlay`, `.path-panel`, `.path-phase`, `.path-chip` inside the existing `<style>` block
- Add HTML for the button and overlay panel just before `</body>`
- Add ~20 lines of JS: `openPath()`, `closePath()`, Esc key listener, body scroll lock
- No new files, no build step — pure HTML/CSS/JS additions to existing pages
- Both pages get identical structure; only text labels differ (ES vs EN)

## Out of scope

- Clicking a topic chip does not navigate
- No progress tracking, no "completed" state
- No animation beyond a simple opacity fade-in on the overlay
