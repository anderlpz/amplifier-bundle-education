# Design System: [Subject Name]

<!-- INSTRUCTIONS: This template defines the visual identity for your education product.
     Fill in the values for your project — or leave the defaults (Paper Frame v3.0)
     if you want to use the reference implementation aesthetic.

     The content-strategist and visual-director agents will read this document.
     The asset-builder uses the tokens defined here for all SVG production.
     The section-author uses the layout specs for HTML construction. -->

| | |
|---|---|
| **Version** | 1.0 |
| **Date** | [Date] |
| **Applies to** | Education product — site, deck, diagrams |

This document is the single source of truth for every visual decision. It defines colors, typography, spacing, components, and layout. Companion file `tokens.css` implements these values as CSS custom properties.

---

## 1. Aesthetic Identity

<!-- Describe the overall visual identity in one paragraph.
     Reference implementations to consider:
     - Paper Frame (editorial, typographic, warm paper aesthetic)
     - Dark technical (dark backgrounds, bright accents, monospace-forward)
     - Magazine/editorial (rich imagery, generous whitespace, serif-heavy)
     - Dashboard/product (clean UI, functional, sans-serif dominant) -->

[Aesthetic description — what does this look and feel like? What is the reading experience?]

**Primary influences:**
- [Reference 1] — [what you're borrowing from it]
- [Reference 2] — [what you're borrowing from it]

---

## 2. Color System

**Guiding principle:** [Your color philosophy in one sentence]

### 2.1 Backgrounds

| Token | Value | Purpose |
|-------|-------|---------|
| `--bg-canvas` | `#EDEBE6` | Page background — warm stone (Paper Frame default) |
| `--bg-card` | `#FFFFFF` | Reading surface — white card on canvas |
| `--bg-code` | `#F5F3EE` | Code block backgrounds — warm light gray |
| `--bg-muted` | `#F7F5F0` | Callouts, blockquotes — subtle background |

<!-- Adjust these values for your aesthetic. The defaults are the Paper Frame palette.
     For a dark aesthetic, swap canvas to ~#1A1A1A and card to ~#242424 -->

### 2.2 Text Colors

| Token | Value | Contrast | Purpose |
|-------|-------|----------|---------|
| `--text-primary` | `#1A1815` | 15.2:1 (AAA) | Headlines, emphasis |
| `--text-secondary` | `#6B6560` | 5.8:1 (AA) | Body text |
| `--text-muted` | `#9C9590` | 3.2:1 (AA large) | Captions, metadata |

### 2.3 Accent

| Token | Value | Purpose |
|-------|-------|---------|
| `--accent` | `#0082EB` | Interactive elements — the only chromatic color |
| `--accent-hover` | `#006BC4` | Hover/active state |
| `--accent-deep` | `#005AA0` | Accent text on light backgrounds |
| `--accent-bg` | `rgba(0, 130, 235, 0.08)` | Subtle accent background |

### 2.4 Semantic Colors (Subject-Specific)

<!-- Add semantic color tokens for concept-level color coding in diagrams.
     The amplifier-masterclass used: amber=kernel, teal=modules, indigo=bundles, etc.
     Define your own based on your subject's conceptual layers. -->

| Token | Value | Concept |
|-------|-------|---------|
| `--color-[concept-a]` | `#hex` | [What this color represents] |
| `--color-[concept-b]` | `#hex` | [What this color represents] |

---

## 3. Typography

**Guiding principle:** [Your typography philosophy in one sentence]

### 3.1 Font Stacks

| Token | Value | Role |
|-------|-------|------|
| `--font-serif` | `'Lora', Georgia, serif` | Reading voice — headlines, chapter titles |
| `--font-sans` | `'Inter', system-ui, sans-serif` | Working voice — body text, UI |
| `--font-mono` | `'Space Grotesk', monospace` | Precision voice — labels, eyebrows |
| `--font-code` | `'SF Mono', 'Fira Code', monospace` | Code blocks |

<!-- These are the Paper Frame defaults. For a more technical aesthetic,
     consider JetBrains Mono + IBM Plex Sans. For a more academic look,
     consider Source Serif 4 + Source Sans 3. -->

### 3.2 Type Scale

| Token | Value | Usage |
|-------|-------|-------|
| `--text-xs` | `0.75rem` | 12px — labels, metadata |
| `--text-sm` | `0.875rem` | 14px — captions, nav text |
| `--text-base` | `1rem` | 16px — body text |
| `--text-lg` | `1.125rem` | 18px — lead paragraphs |
| `--text-xl` | `1.25rem` | 20px — subheadings |
| `--text-2xl` | `clamp(1.5rem, 2vw, 1.75rem)` | 24-28px — h3 |
| `--text-3xl` | `clamp(1.875rem, 3vw, 2.25rem)` | 30-36px — h2 |
| `--text-4xl` | `clamp(2.25rem, 4vw, 3rem)` | 36-48px — h1 chapter titles |

---

## 4. Spacing

**Base unit:** 8px

| Token | Value | Usage |
|-------|-------|-------|
| `--space-2` | `8px` | Small padding, label gaps |
| `--space-4` | `16px` | Grid gaps, element margins |
| `--space-6` | `24px` | Paragraph spacing |
| `--space-8` | `32px` | Card padding (standard) |
| `--space-12` | `48px` | Between content blocks |
| `--space-16` | `64px` | Between major sections |
| `--space-24` | `96px` | Chapter top/bottom padding |

---

## 5. Layout

### Three Content Widths

| Width | Token | Value | Usage |
|-------|-------|-------|-------|
| Reading | `--measure-reading` | `650px` | Prose, inline code, small media |
| Wide | `--measure-wide` | `1100px` | Diagrams, tables, comparison layouts |
| Full | — | `100%` | Hero moments only (rare) |

### Card Layout

Each chapter renders as a white card on the canvas:

```
canvas (--bg-canvas)
  card (--bg-card, max-width 1200px, border-radius 8px, shadow)
    reading column (max-width 650px, centered)
      prose, inline code, small media
    wide breakout (max-width 1100px)
      diagrams, tables, comparison layouts
```

### Breakpoints

| Name | Max width | Changes |
|------|-----------|---------|
| Desktop | None | Default layout |
| Tablet | `1024px` | Card padding tightens |
| Mobile | `768px` | Card goes full-width, canvas disappears |
| Small mobile | `480px` | Further padding reduction |

---

## 6. Component Patterns

### Card / Chapter Container

```
background: --bg-card
padding: --space-24 --space-8
border-radius: --radius-md (8px)
box-shadow: 0 1px 3px rgba(0,0,0,0.04), 0 4px 12px rgba(0,0,0,0.03)
```

### Callout / Aside

```
background: --bg-muted
border-left: 3px solid --accent
padding: --space-6 --space-8
border-radius: 0 4px 4px 0
font-style: italic
```

### Code Block

```
background: --bg-code
border: 1px solid rgba(0,0,0,0.08)
border-radius: 8px
padding: --space-6
font: --font-code, --text-sm
```

---

## 7. Diagram Design

### Default Diagram Palette

All diagrams use the same background as the canvas with white card nodes:

```
SVG background:  --bg-canvas (#EDEBE6)
Node fill:       --bg-card (#FFFFFF)
Node border:     rgba(0,0,0,0.15)
Label:           Inter 400, 13px, --text-primary
Caption:         Inter 400, 11px, --text-muted
Title:           Lora 600, 16px, --text-primary
Mono/code:       SF Mono, 11px, --text-primary
```

### Semantic Color Coding

Use the subject-specific semantic tokens from Section 2.4 to color-code conceptual layers in diagrams.

---

## 8. Accessibility

- All text/background contrast ratios meet WCAG AA minimum
- `prefers-reduced-motion` respected — all animation durations set to 0.01ms
- Touch targets: 44x44px minimum
- Focus indicators: `outline: 2px solid --accent; outline-offset: 2px`
- All diagrams have `role="img"` and `aria-label`
- Semantic HTML throughout

---

## 9. Do / Don't

### Color

| Do | Don't |
|----|-------|
| Use one accent color for all interactive elements | Introduce a second accent hue |
| Use `--text-secondary` for body text | Use `--text-muted` for body text |
| Let whitespace handle most separation | Add borders everywhere |

### Typography

| Do | Don't |
|----|-------|
| Use serif for chapter titles | Use serif for body text |
| Use generous line-height (1.7) | Compress line-height below 1.5 |
| Use monospace for labels and code references | Use monospace for body text |

### Motion

| Do | Don't |
|----|-------|
| Use entrance-only scroll reveals | Add looping animations |
| Respect `prefers-reduced-motion` | Ignore the reduced-motion query |
| Keep transitions under 600ms | Use slow, dramatic entrances |
