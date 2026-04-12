# Deliverable Specifications

The education pipeline produces three deliverables from a single content source. Each deliverable has distinct technical requirements, rendering conventions, and production notes.

---

## Deliverable 1: HTML Site (site.html)

A standalone single-file HTML reading site. No build step, no server required — the file opens in any browser.

### Technical Requirements

- **Format:** Single self-contained HTML file
- **Dependencies:** Embedded or CDN-linked (no local assets except SVG diagrams referenced inline)
- **Target size:** 200-400KB for the HTML skeleton; SVG assets are inline
- **Chapters:** Determined by the section count from `shape-content` phase

### Visual Design

The "Paper Frame" aesthetic — a white reading card on a warm stone canvas:

| Token | Value | Purpose |
|-------|-------|---------|
| Canvas background | `#EDEBE6` | Warm stone — the page ground plane |
| Card background | `#FFFFFF` | White reading surface — each chapter is a card |
| Primary text | `#1A1815` | Headlines, emphasis (15.2:1 contrast, AAA) |
| Body text | `#6B6560` | Paragraphs, descriptions (5.8:1, AA) |
| Muted text | `#9C9590` | Captions, chapter numbers, metadata |
| Accent | `#0082EB` | Links, interactive elements — the only chromatic color |
| Code background | `#F5F3EE` | Warm light gray for code blocks |

Typography stack (from Google Fonts + system):
- **Lora** (serif) — Chapter titles, section headings (600 weight)
- **Inter** (sans-serif) — Body text, navigation, UI elements
- **Space Grotesk** (mono-feeling sans) — Labels, chapter numbers, eyebrows
- **SF Mono / Fira Code** — Actual code blocks

### Layout Structure

```
Fixed nav bar (52px, z-index 100)
│  Chapter N · Title           ≡              ▶ Listen
│
Scrollable content
│
  Warm canvas (#EDEBE6)
  │
    White card (max-width 1200px, centered, border-radius 8px)
    │
      Reading column (max-width 650px, centered in card)
      │  Chapter eyebrow  [Space Grotesk, 12px, uppercase, muted]
      │  Chapter title    [Lora 600, fluid 36-48px, primary]
      │  Lead paragraph   [Inter 400, 18px, secondary]
      │  Body content     [Inter 400, 16px, leading 1.7]
      │
      Wide breakout (max-width 1100px)
      │  Diagrams, tables, comparison layouts
      │
      Full width (rare — hero moments only)
```

### Interactive Features

- **Sidebar TOC** — sticky table of contents with scroll tracking, active chapter highlight
- **Scroll-reveal animations** — `IntersectionObserver` entrance for content blocks
- **D3 dotgraph viewer** (for architecture sections) — compact inline render with fullscreen overlay; zoom (scroll wheel), pan (drag), click-to-highlight nodes
- **Presentation mode** — floating "Present" button transforms the site into a fullscreen slide deck (arrow-key navigation, speaker notes panel, slide counter)
- **Audio player** — per-chapter playback via `<audio>` element in the nav bar

### Accessibility

- All contrast ratios pass WCAG AA minimum
- `prefers-reduced-motion` respected (all animations/transitions disabled)
- Semantic HTML: `<nav>`, `<main>`, `<article>`, `<section>`, `<figure>`, `<figcaption>`
- All diagrams have text alternatives

---

## Deliverable 2: Presentation Deck (deck.html)

A standalone HTML presentation with arrow-key navigation. Same file, same browser — no dependencies.

### Technical Requirements

- **Format:** Single self-contained HTML file
- **Navigation:** Arrow keys (left/right or up/down), swipe on touch devices
- **Slide counter:** Current / Total in corner
- **Speaker notes:** Visible in presenter view (bottom panel), hidden from projected view
- **Escape / click button:** Exits to slide overview

### Slide Construction Rules

Each section condenses into however many slides the topic requires:

| Section type | Typical slide count |
|-------------|---------------------|
| Introduction / overview | 1-2 slides |
| Deep concept (e.g., Tools vs Hooks) | 4-6 slides |
| Architecture map | 1 slide (the diagram IS the slide) |
| Reference section (sessions, bundles) | 2-3 slides |
| Closing / capstone | 2-3 slides |

**Content rules per slide:**
- One idea per slide — if a second idea arrives, make a second slide
- Visual assets get their own slides
- Text slides: headline + 3-5 bullet points maximum — no paragraphs
- The site's inline explanations become speaker notes

### Speaker Notes Format

Every slide has speaker notes in `data-notes` attribute on the slide element:

```html
<section data-notes="Key points to emphasize: ... Transition to next slide: ... Source refs: VC-07, XC-03">
```

Notes include:
1. Key points to emphasize verbally
2. Transition language to the next slide
3. Internal source refs (VC-XX) for presenter reference — not shown to audience

### Visual Style

Same design tokens as the HTML site. Fullscreen slides use:
- Dark canvas or white card depending on slide type
- Large typography — headlines 48-72px
- Diagrams displayed at full slide width
- Minimal text — 3-5 bullets maximum per slide

---

## Deliverable 3: Audio Narration (audio/)

Per-chapter voiceover scripts and MP3 audio files.

### File Convention

```
audio/
├── ch01-introduction.txt      ← voiceover script (plain text)
├── ch01-introduction.mp3      ← rendered audio
├── ch02-architecture-map.txt
├── ch02-architecture-map.mp3
└── ...
```

### Script Format

Voiceover scripts are **not** a reading of the site's prose. They are adapted for listening:

**Key adaptations:**
1. Replace "as shown in the diagram below" with a verbal description of what the diagram shows
2. Read code examples only if short (1-3 lines); for longer blocks, describe what the code does
3. Add verbal signposts where prose uses visual breaks: "Now let's look at..." or "The next piece is..."
4. Simplify tables into verbal lists: "There are six module types. The first is the Orchestrator, which drives the conversation..."
5. Add pause markers `[pause]` where the listener needs a beat to absorb something
6. Shorter sentences than prose — audio requires it
7. More direct address — "You" and "we" are appropriate

**Voice:** Same "best professor" voice as the prose — patient, knowledgeable, clear. Slightly warmer because audio is intimate. Never casual ("hey guys"), never breathless.

**Inductive principle (required):** Always start with the concrete scenario, then name the architectural concept. Never start with an abstraction.

**Length targets:**
- 60-120 words per 30 seconds of audio
- Each chapter: 3-8 minutes depending on depth level
- Deep chapters (Tools vs Hooks, The Kernel): up to 10 minutes

### Podcast Concatenation

Scripts include markers for podcast-style concatenation:

```
[CHAPTER START: Chapter 4 — The Kernel]
[INTRO MUSIC: 3 seconds]

[BODY]
... voiceover text ...
[pause]
... more text ...

[OUTRO: "This was Chapter 4. Next: The Module System."]
[OUTRO MUSIC: 3 seconds]
[CHAPTER END]
```

The edition manager tracks which MP3s exist vs. which scripts need audio rendering.

---

## Bonus Deliverable: Astro Site (Optional)

For projects that need a production-deployable site (not just a standalone HTML file), the pipeline can additionally produce a componentized Astro site. This is an optional Phase 5 not included in the standard recipes.

**Component architecture:**
- One Astro component per block type: `ProseBlock`, `DiagramBlock`, `TableBlock`, `ComparisonBlock`, `CalloutBlock`, `AudioBlock`, `ChatBlock`, `VignetteBlock`
- Section markdown files drive content via Astro content collections
- Design tokens in `tokens.css` shared across all components

This is the production pattern for real deployment but requires Node.js/npm and is not part of the default `full-edition` recipe.
