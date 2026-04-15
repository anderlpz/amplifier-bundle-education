# Design System: [Subject Name]

<!-- INSTRUCTIONS: This template defines the visual identity for your education product.
     Two named variants are provided: Slate Folio (production-validated default) and
     Paper Frame (warm editorial alternate). Choose one, or define your own.

     The content-strategist and visual-director agents will read this document.
     The asset-builder uses the tokens defined here for all SVG production.
     The section-author uses the layout specs for HTML construction. -->

| | |
|---|---|
| **Version** | 2.0 |
| **Date** | [Date] |
| **Variant** | Slate Folio |
| **Applies to** | Education product — site, deck, diagrams |

This document is the single source of truth for every visual decision. It defines colors, typography, spacing, components, and layout. Companion file `tokens.css` implements these values as CSS custom properties.

---

## 1. Aesthetic Identity

<!-- Describe the overall visual identity in one paragraph.
     Two reference variants are provided below. Use one as-is, blend them, or define your own.

     Slate Folio — Cool, technical, reading-forward. Production-validated in amplifier-masterclass.
     Paper Frame — Warm, editorial, typographic. The original bundle default.
     Custom     — Define your own palette and typography below. -->

[Aesthetic description — what does this look and feel like? What is the reading experience?]

**Primary influences:**
- [Reference 1] — [what you're borrowing from it]
- [Reference 2] — [what you're borrowing from it]

---

## 2. Color System

**Guiding principle:** [Your color philosophy in one sentence]

<!-- Choose a variant below or define custom values. The active variant's tokens are what
     the site builder reads. Comment out the inactive variant or delete it. -->

### Variant: Slate Folio (default — production-validated)

#### 2.1 Backgrounds

| Token | Value | Purpose |
|-------|-------|---------|
| `--bg-canvas` | `#F4F5F9` | Cool blue-gray canvas |
| `--bg-card` | `#FFFFFF` | White reading surface |
| `--surface-recessed` | `#EDEDF2` | Recessed areas (sidebar, code) |
| `--bg-code` | `#F0F0F5` | Code block backgrounds |
| `--bg-muted` | `#EDEDF2` | Callouts, blockquotes |

#### 2.2 Text Colors

| Token | Value | Contrast | Purpose |
|-------|-------|----------|---------|
| `--text-primary` | `#1C1E2B` | 14.8:1 (AAA) | Headlines, emphasis |
| `--text-secondary` | `#5C5E6E` | 6.1:1 (AA) | Body text |
| `--text-muted` | `#8B8D9C` | 3.4:1 (AA large) | Captions, metadata |

#### 2.3 Accent

| Token | Value | Purpose |
|-------|-------|---------|
| `--accent` | `#3E5C8A` | Slate blue — links, interactive elements |
| `--accent-hover` | `#2E4A74` | Hover/active state |
| `--accent-soft` | `#E4E9F2` | Accent backgrounds |

#### 2.4 Borders

| Token | Value | Purpose |
|-------|-------|---------|
| `--border` | `#D8D9E3` | Standard borders |
| `--border-light` | `#E8E9EF` | Light borders |

#### 2.5 Status Colors

| Token | Value | Purpose |
|-------|-------|---------|
| `--positive` | `#3D7A4A` | Success states |
| `--caution` | `#8A6B3E` | Warning states |

### Variant: Paper Frame (alternate — warm editorial)

<!-- Uncomment and set **Variant** to "Paper Frame" in the header table to use this instead. -->

<!--
#### 2.1 Backgrounds

| Token | Value | Purpose |
|-------|-------|---------|
| `--bg-canvas` | `#EDEBE6` | Warm stone canvas |
| `--bg-card` | `#FFFFFF` | White reading surface |
| `--bg-code` | `#F5F3EE` | Warm light gray for code blocks |
| `--bg-muted` | `#F7F5F0` | Callouts, blockquotes |

#### 2.2 Text Colors

| Token | Value | Contrast | Purpose |
|-------|-------|----------|---------|
| `--text-primary` | `#1A1815` | 15.2:1 (AAA) | Headlines, emphasis |
| `--text-secondary` | `#6B6560` | 5.8:1 (AA) | Body text |
| `--text-muted` | `#9C9590` | 3.2:1 (AA large) | Captions, metadata |

#### 2.3 Accent

| Token | Value | Purpose |
|-------|-------|---------|
| `--accent` | `#0082EB` | Interactive elements — the only chromatic color |
| `--accent-hover` | `#006BC4` | Hover/active state |
| `--accent-deep` | `#005AA0` | Accent text on light backgrounds |
| `--accent-bg` | `rgba(0, 130, 235, 0.08)` | Subtle accent background |
-->

### 2.6 Semantic Layer Colors (both variants)

Used for architecture/concept diagrams and term definition tooltips. Map your subject's conceptual layers to these colors:

| Layer | Token | Color | Soft variant | Concept |
|-------|-------|-------|-------------|---------|
| Kernel | `--layer-kernel` | `#9B7B3E` (warm amber) | `#F5F0E4` | [Core runtime / central concept] |
| Modules | `--layer-modules` | `#3E7D8A` (teal) | `#E4F0F2` | [Module system / composable units] |
| Bundles | `--layer-bundles` | `#5C6B9E` (muted indigo) | `#ECEEF5` | [Distribution / packaging] |
| Foundation | `--layer-foundation` | `#5A7D5A` (sage green) | `#E8F0E8` | [Base infrastructure / primitives] |
| Boundary | `--layer-boundary` | `#8A5A4A` (warm coral) | `#F2EAE6` | [Interfaces / contracts / edges] |
| Apps | `--layer-apps` | `#7B6B8A` (muted purple) | `#F0ECF2` | [Application layer / user-facing] |
| Ecosystem | `--layer-ecosystem` | `#8A7B8A` (dusty mauve) | `#F2EEF2` | [External / community / broader context] |

<!-- Rename layers to match your subject's conceptual hierarchy.
     The amplifier-masterclass used these exact names. For a different subject,
     you might have: "Core", "API", "Plugins", "Config", "Security", "UI", "Community" -->

---

## 3. Typography

**Guiding principle:** [Your typography philosophy in one sentence]

### 3.1 Font Stacks

<!-- Slate Folio typography (default): -->

| Token | Value | Role |
|-------|-------|------|
| `--font-heading` | `'Inter', system-ui, sans-serif` | Headlines, chapter titles |
| `--font-body` | `'Source Serif 4', Georgia, serif` | Reading voice — body text |
| `--font-mono` | `'JetBrains Mono', 'SF Mono', monospace` | Code blocks, inline code |
| `--font-label` | `'Inter', system-ui, sans-serif` | Labels, eyebrows, nav |

<!-- Paper Frame typography (alternate):
| Token | Value | Role |
|-------|-------|------|
| `--font-heading` | `'Lora', Georgia, serif` | Reading voice — headlines, chapter titles |
| `--font-body` | `'Inter', system-ui, sans-serif` | Working voice — body text, UI |
| `--font-mono` | `'SF Mono', 'Fira Code', monospace` | Code blocks |
| `--font-label` | `'Space Grotesk', monospace` | Precision voice — labels, eyebrows |
-->

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
|------|-----------|---------
| Desktop | ≥1200px | Permanent sidebar (280px fixed) |
| Tablet | 768px–1199px | Modal flyout sidebar |
| Mobile | ≤767px | Full-width bottom bar + bottom sheet |
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

### Design Decision Callout

```
background: var(--layer-boundary-soft)   /* #F2EAE6 */
border-left: 3px solid var(--layer-boundary)   /* #8A5A4A */
border-radius: 4px
padding: --space-6 --space-8
Label: "DESIGN DECISION" + flag icon
```

### Code Block

```
background: --bg-code
border: 1px solid var(--border-light)
border-radius: 8px
padding: --space-6
font: --font-mono, --text-sm
```

### Annotated Code Block

```
Side-by-side on desktop (code left, annotations right)
Stacked on mobile (code above, annotations below)
Numbered markers: ①②③④ positioned at specific lines
Layer-colored border via data-layer attribute
```

---

## 7. Diagram Design

### Default Diagram Palette

All diagrams use the same background as the canvas with white card nodes:

```
SVG background:  --bg-canvas
Node fill:       --bg-card (#FFFFFF)
Node border:     var(--border) or rgba(0,0,0,0.15)
Label:           --font-body, 13px, --text-primary
Caption:         --font-body, 11px, --text-muted
Title:           --font-heading, 600, 16px, --text-primary
Mono/code:       --font-mono, 11px, --text-primary
```

### Semantic Color Coding

Use the semantic layer tokens from Section 2.6 to color-code conceptual layers in diagrams. Both solid and soft variants are available for fills vs. backgrounds.

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
| Use semantic layer colors for concept coding | Use random colors in diagrams |

### Typography

| Do | Don't |
|----|-------|
| Use heading font for chapter titles | Mix heading fonts |
| Use generous line-height (1.7) | Compress line-height below 1.5 |
| Use monospace for code and labels | Use monospace for body text |

### Motion

| Do | Don't |
|----|-------|
| Use entrance-only scroll reveals | Add looping animations |
| Respect `prefers-reduced-motion` | Ignore the reduced-motion query |
| Keep transitions under 600ms | Use slow, dramatic entrances |
