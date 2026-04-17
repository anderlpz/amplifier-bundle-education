# Deliverable Specifications

The education pipeline produces five deliverables from a single content source. Each deliverable has distinct technical requirements, rendering conventions, and production notes.

### Output File Naming

All deliverables are named using the subject slug derived from `subject_name`:

| Deliverable | Filename Pattern | Example |
|-------------|-----------------|---------|
| Reading site | `{slug}-site.html` | `design-intelligence-for-amplifier-site.html` |
| Presentation deck | `{slug}-deck.html` | `design-intelligence-for-amplifier-deck.html` |
| Audio narration | `audio/{voice}/chNN-slug.mp3` | `audio/fable/ch01-introduction.mp3` |
| Reference document | `{slug}.md` | `design-intelligence-for-amplifier.md` |
| Print-ready PDF | `{slug}.pdf` | `design-intelligence-for-amplifier.pdf` |

The slug is derived from `subject_name`: lowercase, spaces→hyphens, special chars removed. The intermediate print HTML (`{slug}-print.html`) follows the same pattern.

The markdown and PDF are the canonical "document" and get the clean `{slug}` name. Site and deck get format suffixes (`-site`, `-deck`). Audio stays in the `audio/` directory — the voice subdirectories and chapter files inside are already well-named.

---

## Deliverable 1: HTML Site (`{slug}-site.html`)

A standalone single-file HTML reading site. No build step, no server required — the file opens in any browser.

### Technical Requirements

- **Format:** Single self-contained HTML file
- **Dependencies:** Embedded or CDN-linked (no local assets except SVG diagrams referenced inline)
- **Target size:** 200-400KB for the HTML skeleton; SVG assets are inline
- **Chapters:** Determined by the section count from `shape-content` phase

### Visual Design

The education bundle supports two named design variants. Choose one in `.design/DESIGN-SYSTEM.md` — the build pipeline reads the active variant from that file.

#### Paper Frame (warm editorial)

| Token | Value | Purpose |
|-------|-------|---------|
| `--bg-canvas` | `#EDEBE6` | Warm stone canvas |
| `--bg-card` | `#FFFFFF` | White reading surface |
| `--text-primary` | `#1A1815` | Headlines |
| `--text-secondary` | `#6B6560` | Body text |
| `--text-muted` | `#9C9590` | Captions |
| `--accent` | `#0082EB` | Links, interactive |
| `--bg-code` | `#F5F3EE` | Code blocks |

Typography: Headings `Lora` (serif) · Body `Inter` (sans) · Labels `Space Grotesk` · Code `SF Mono / Fira Code`

#### Slate Folio (cool technical) — validated in amplifier-masterclass production

| Token | Value | Purpose |
|-------|-------|---------|
| `--bg-canvas` | `#F4F5F9` | Cool blue-gray canvas |
| `--bg-card` | `#FFFFFF` | White reading surface |
| `--surface-recessed` | `#EDEDF2` | Recessed areas |
| `--text-primary` | `#1C1E2B` | Headlines |
| `--text-secondary` | `#5C5E6E` | Body text |
| `--text-muted` | `#8B8D9C` | Captions |
| `--accent` | `#3E5C8A` | Slate blue accent |
| `--accent-hover` | `#2E4A74` | Accent hover |
| `--accent-soft` | `#E4E9F2` | Accent backgrounds |
| `--border` | `#D8D9E3` | Standard borders |
| `--border-light` | `#E8E9EF` | Light borders |
| `--bg-code` | `#F0F0F5` | Code blocks |
| `--positive` | `#3D7A4A` | Success states |
| `--caution` | `#8A6B3E` | Warning states |

Typography: Headings `Inter` (sans) · Body `Source Serif 4` (serif) · Code `JetBrains Mono`

#### Semantic Layer Colors (available in both variants)

Used for architecture/concept diagrams and interactive term definitions:

| Layer | Color | Soft variant |
|-------|-------|-------------|
| Kernel | `#9B7B3E` (warm amber) | `#F5F0E4` |
| Modules | `#3E7D8A` (teal) | `#E4F0F2` |
| Bundles | `#5C6B9E` (muted indigo) | `#ECEEF5` |
| Foundation | `#5A7D5A` (sage green) | `#E8F0E8` |
| Boundary | `#8A5A4A` (warm coral) | `#F2EAE6` |
| Apps | `#7B6B8A` (muted purple) | `#F0ECF2` |
| Ecosystem | `#8A7B8A` (dusty mauve) | `#F2EEF2` |

### Layout Structure — 3-Tier Responsive Navigation

#### Desktop (≥1200px)

```
Permanent sidebar (280px fixed, always visible)
│  Collapse button (chevron on right edge)
│  body.sidebar-collapsed hides sidebar, floating pill reappears
│
│  ┌─ Sidebar ────────────────────────┐
│  │  Branding / subject title        │
│  │  Chapter list (TOC)              │
│  │    ● Ch 1 — Introduction    ▶   │
│  │    ○ Ch 2 — Architecture        │
│  │    ○ Ch 3 — The Kernel          │
│  │  ──────────────────────────────  │
│  │  Audio controls (docked bottom)  │
│  │    Speed selector · Voice pills  │
│  └──────────────────────────────────┘
│
Body content (padding-left: 280px)
│  Warm canvas (--bg-canvas)
│    White card (max-width 1200px, centered, border-radius 8px)
│      Reading column (max-width 650px, centered in card)
│      Wide breakout (max-width 1100px)
```

#### Tablet (768px–1199px)

```
Modal flyout sidebar (triggered by floating pill button)
│  Slide-in from left, 280px wide
│  Same content as desktop sidebar
│  Overlay backdrop dims content area
```

#### Mobile (≤767px)

```
Full-width bottom bar (not floating pill)
│  height: calc(44px + env(safe-area-inset-bottom))
│  Contains: progress ring, chapter number, chapter title (ellipsis), audio play button
│  2px progress line along top edge
│
Bottom sheet (replaces left-sliding drawer)
│  height: 65vh, slides up from bottom
│  Drag-to-dismiss (touchstart/touchmove/touchend, 80px threshold)
│  border-radius: 12px 12px 0 0
│  Contains:
│    Branding
│    Chapter list with play buttons
│    Audio controls (transport, speed, voice)
```

#### Scrollspy

IntersectionObserver tracks visible chapter:
- Updates TOC active state (accent highlight on active entry)
- Auto-scrolls sidebar to keep active link visible
- Coordinates with audio (does NOT auto-jump if audio is playing)

### Interactive Features

- **Sidebar TOC** — sticky table of contents with scroll tracking, active chapter highlight
- **Scroll-reveal animations** — `IntersectionObserver` entrance for content blocks
- **D3 dotgraph viewer** (for architecture sections) — compact inline render with fullscreen overlay; zoom (scroll wheel), pan (drag), click-to-highlight nodes
- **Presentation mode** — floating "Present" button transforms the site into a fullscreen slide deck (arrow-key navigation, speaker notes panel, slide counter)
- **Audio player** — per-chapter playback wired to MP3 files (see Audio Player Contract below)

### Interactive Components

#### Term Definition Tooltips

Inline `.lt` (layer term) spans for domain vocabulary:

```html
<span class="lt" data-l="kernel" tabindex="0">kernel</span>
```

- Underlined in semantic layer color
- Click or keyboard focus → styled tooltip card:
  - Colored dot + term name + layer badge
  - Definition (30–50 words)
  - "Read more in §N.N" link to relevant section
- Requires `TERM_DEFS` JavaScript catalog: `term → { layer, definition, section }`
- Dismiss: click outside, Escape, or re-click

#### Annotated Code Blocks

- Side-by-side on desktop: code left, numbered annotations right
- Stacked on mobile: code above, annotations below
- Numbered markers (①②③④) positioned at specific code lines
- Marker position: measure each source line's rendered bounding rect, center marker vertically
- Layer-colored border on code container (`data-layer` attribute)

#### Flow-Table Connected Units

- Diagram + table as one visual block (`.flow-table-unit`)
- Numbered-step flow diagram (IKEA-style circles with arrows) above
- Corresponding table below, sharing semantic layer colors
- Wide breakout on desktop, full-width on mobile

#### Design Decision Callout Boxes

- Warm coral left border (`border-left: 3px solid var(--layer-boundary)`)
- Soft coral background (`var(--layer-boundary-soft)`)
- 4px border-radius
- "DESIGN DECISION" label + flag icon + verdict text + reasoning paragraph

### Audio Player Contract

The audio player is conditional: it is built only when MP3 files exist in `audio/`. When no MP3s are present, the site omits all audio UI elements entirely.

**AUDIO_FILES + VOICE_AUDIO_FILES maps:**

The site builder scans `audio/` at build time and generates JavaScript maps:

```javascript
const AUDIO_FILES = {
  "ch01-introduction": "audio/ch01-introduction.mp3",
  "ch02-architecture-map": "audio/ch02-architecture-map.mp3",
  // ... one entry per MP3 file found in audio/ root (primary voice)
};

const VOICE_AUDIO_FILES = {
  fable: { 's1': 'audio/fable/ch01-introduction.mp3', ... },
  nova:  { 's1': 'audio/nova/ch01-introduction.mp3', ... },
  alloy: { 's1': 'audio/alloy/ch01-introduction.mp3', ... }
};

let activeVoice = 'fable';
```

Keys in AUDIO_FILES are section IDs (matching the chapter `<section id="...">` in the HTML). Values are relative paths to the MP3 files. VOICE_AUDIO_FILES maps each voice to the same section-ID keyed structure.

**Required controls:**

| Control | Location | Behavior |
|---------|----------|----------|
| Nav pill button | Fixed nav bar (right side) | Single play/pause toggle for current chapter. Shows ▶ when paused, ❚❚ when playing. |
| Per-chapter TOC buttons | Sidebar TOC, next to each chapter entry | Small play/pause icon. Clicking starts that chapter's audio (pauses any other). |
| Voice selector | Sidebar audio controls section | Pills or menu. Show only voices that have files. Switching voice preserves currentTime and playback state. Hide if only 1 voice detected. |
| Speed selector | Sidebar audio controls section | Cycle button: 0.75×, 1×, 1.25×, 1.5×, 2×. Persists across chapter changes. Sets `playbackRate` on the audio element. |
| Transport controls | Sidebar audio controls section | Back 15s, forward 15s, scrubber bar (3px track, accent fill, draggable thumb, mm:ss display). |
| Audio discoverability | Under each chapter title | Prominent "Listen to this chapter" affordance. |

**Required behaviors:**

| Behavior | Description |
|----------|-------------|
| Scroll-to-audio sync | `IntersectionObserver` tracks which chapter is in view. When the visible chapter changes, update which audio the nav pill button controls. Do NOT auto-play on scroll. |
| Auto-advance | When a chapter's audio finishes (`ended` event), load and play the next chapter's audio if it exists in AUDIO_FILES. |
| Keyboard toggle | `P` key toggles play/pause for the current chapter. |
| Single `<audio>` element | One shared `<audio>` element in the DOM. Swap its `src` when chapters change. Avoids multiple simultaneous streams. |
| Voice switch preserves state | When switching voices, preserve currentTime and playbackRate. Swap src to the new voice's file, seek to the saved time, and resume if was playing. |
| Dynamic voice detection | Populate voice selector from voices that actually have files. Hide selector entirely if only 1 voice. |

**CSS states:**

| State | Selector | Effect |
|-------|----------|--------|
| Chapter playing | `.toc-entry.playing` | Accent color highlight on the active TOC entry |
| Section playing | `section.playing` | Optional subtle indicator on the active section |
| Nav pill active | `.audio-pill.playing` | Icon swaps from ▶ to ❚❚ |

### Accessibility

- All contrast ratios pass WCAG AA minimum
- `prefers-reduced-motion` respected (all animations/transitions disabled)
- Semantic HTML: `<nav>`, `<main>`, `<article>`, `<section>`, `<figure>`, `<figcaption>`
- All diagrams have text alternatives

---

## Deliverable 2: Presentation Deck (`{slug}-deck.html`)

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

Same design tokens as the HTML site (per DESIGN-SYSTEM.md variant). Fullscreen slides use:
- Dark canvas or white card depending on slide type
- Large typography — headlines 48-72px
- Diagrams displayed at full slide width
- Minimal text — 3-5 bullets maximum per slide

---

## Deliverable 3: Audio Narration (audio/)

Per-chapter voiceover scripts and MP3 audio files, synthesized across multiple voices.

### File Convention

```
audio/
├── ch01-introduction.mp3       ← primary voice (default)
├── ch01-introduction.txt       ← narration script (clean prose)
├── ch02-architecture-map.mp3
├── ch02-architecture-map.txt
├── fable/
│   ├── ch01-introduction.mp3
│   ├── ch02-architecture-map.mp3
│   └── ...
├── nova/
│   ├── ch01-introduction.mp3
│   ├── ch02-architecture-map.mp3
│   └── ...
└── alloy/
    ├── ch01-introduction.mp3
    ├── ch02-architecture-map.mp3
    └── ...
```

The root `audio/*.mp3` files are copies of the primary (first) voice. Voice subdirectories contain the per-voice renders.

### Script Format

Voiceover scripts are **clean prose** — no production markers, no `[pause]`, no `[CHAPTER START]` / `[CHAPTER END]`. They are NOT a reading of the site's prose. They are dense summaries adapted for listening: compressed "key points" versions (8–20 lines of tight prose per chapter).

**Key adaptations:**
1. Replace "as shown in the diagram below" with a verbal description of what the diagram shows
2. Read code examples only if short (1-3 lines); for longer blocks, describe what the code does
3. Add verbal signposts where prose uses visual breaks: "Now let's look at..." or "The next piece is..."
4. Simplify tables into verbal lists: "There are six module types. The first is the Orchestrator, which drives the conversation..."
5. Shorter sentences than prose — audio requires it
6. More direct address — "You" and "we" are appropriate

**Voice:** Same "best professor" voice as the prose — patient, knowledgeable, clear. Slightly warmer because audio is intimate. Never casual ("hey guys"), never breathless.

**Inductive principle (required):** Always start with the concrete scenario, then name the architectural concept. Never start with an abstraction.

**Length targets:**
- 60-120 words per 30 seconds of audio
- Each chapter: 3-8 minutes depending on depth level
- Deep chapters (Tools vs Hooks, The Kernel): up to 10 minutes

### TTS Synthesis

MP3 files are synthesized from the .txt scripts via the `synthesize-audio` recipe:

**Process:**
1. Read clean prose script (no marker stripping needed — scripts are already clean)
2. Trim whitespace, collapse blank lines
3. Chunk cleaned text at paragraph/sentence boundaries (max 4096 characters per API call)
4. For each voice in the voices list, call OpenAI TTS API with OPUS intermediate format
5. Convert OPUS to VBR MP3 via `ffmpeg -i chunk.opus -q:a 2 chunk.mp3`
6. Concatenate multi-chunk outputs with ffmpeg (`-f concat -c copy`, no re-encoding)
7. Copy primary voice files to audio/ root as defaults

**Incremental:** Scripts whose .mp3 already has a newer mtime than the .txt are skipped (per voice).

**Configuration defaults:**
- Model: `tts-1-hd` (high quality)
- Voices: `["fable", "nova", "alloy"]` (multiple voices, first is primary)
- Speed: `0.95` (slightly slower for comprehension)
- Provider: OpenAI (extensible to ElevenLabs in future)

**Dependencies:** `openai` Python package, `ffmpeg` system binary.

---

## Deliverable 4: Verbose Markdown (`{slug}.md`)

A single markdown file with YAML frontmatter — the AI-consumable and human-readable complete reference document.

### Technical Requirements

- **Format:** Single markdown file with YAML frontmatter
- **Purpose:** AI-consumable and human-readable complete reference document
- **Contains:** ALL educational prose content from all chapters — nothing summarized, nothing omitted

### Frontmatter

YAML frontmatter includes:
- **title** — subject name + "Complete Reference"
- **edition** — from `.edition/manifest.json` if available
- **date** — compilation date
- **subject** — subject name from content strategy
- **chapters** — array with: number, title, slug, word_count, depends_on, diagrams
- **glossary** — compiled from all sections' term_defs, deduplicated and sorted

### Content Rules

- Every diagram gets a **verbal description paragraph** (for readers without image support or for AI consumption)
- **Term definitions** appear inline as parenthetical on first use per chapter AND in per-chapter glossary sections at end of each chapter
- **Design Decisions** preserved as blockquote with `> **DESIGN DECISION:**` label + verdict + reasoning
- **Annotated code** degraded to sequential layout: code block first, numbered annotation list below
- **Interactive diagrams** degraded to static SVG file reference + verbal description paragraph

### Stripped Content

The following are removed entirely:
- Presentation Slides sections (`## Presentation Slides`)
- Speaker Notes sections (`## Speaker Notes`)
- Audio blocks (`type: audio`)
- Chat blocks (`type: chat`)
- Vignette blocks (`type: vignette`)
- `source_claims` frontmatter field
- `VC-XX` references in prose

---

## Deliverable 5: Print-Ready PDF (`{slug}.pdf`)

A professional typeset document generated via WeasyPrint from `{slug}-print.html` + `print.css`.

### Technical Requirements

- **Format:** PDF generated via WeasyPrint from intermediate HTML
- **Purpose:** Professional printed/digital document — textbook quality
- **Intermediate files:** `{slug}-print.html` (HTML with print-specific classes) + `@education:templates/print.css`
- **Prerequisites:** `weasyprint` Python package (`pip install weasyprint`)

### Typography

| Element | Font | Size | Notes |
|---------|------|------|-------|
| Body text | Source Serif 4 | 10.5pt / 14pt leading | Serif for sustained reading |
| Headings | Source Serif 4 | 24pt (h1), 16pt (h2), 13pt (h3) | Same family, weight differentiation |
| Labels, captions | Inter | 8pt | Sans-serif for UI-like elements |
| Code | JetBrains Mono | 8.5pt / 11pt leading | Monospace with good legibility at small sizes |
| Running headers | Inter | 8pt | Muted gray (#8B8D9C) |

### Page Layout

- **Page size:** US Letter (8.5" × 11")
- **Margins:** 1" top, 1.15" left/right, 1.25" bottom
- **Text block:** ~6.2" wide (~65-75 characters per line at 10.5pt)
- **Running headers:** chapter title (top center), document title (bottom left), page number (bottom right)
- **Chapter opener:** suppresses running header at top

### Structure

- Each chapter starts on a new page (`break-before: page`)
- Auto-generated **table of contents** with dot leaders and page numbers
- SVG diagrams **inlined natively** (Cairo rendering, no rasterization)
- Primarily grayscale with semantic color preserved in diagrams and design decision callouts
- Wide/breakout diagrams extend into margins (`margin-left: -1in; margin-right: -1in`)

### Content Handling

Same stripping and degradation rules as Deliverable 4 (`{slug}.md`):
- Stripped: Presentation Slides, Speaker Notes, audio/chat/vignette blocks, source_claims, VC-XX references
- Degraded: term tooltips → inline + glossary, annotated code → sequential, interactive diagrams → static SVG + description, design decisions → styled callout blocks

### Build Command

```bash
weasyprint --stylesheet print.css {slug}-print.html {slug}.pdf
```

---

## Bonus Deliverable: Astro Site (Optional)

For projects that need a production-deployable site (not just a standalone HTML file), the pipeline can additionally produce a componentized Astro site. This is an optional Phase 5 not included in the standard recipes.

**Component architecture:**
- One Astro component per block type: `ProseBlock`, `DiagramBlock`, `TableBlock`, `ComparisonBlock`, `CalloutBlock`, `AudioBlock`, `ChatBlock`, `VignetteBlock`
- Section markdown files drive content via Astro content collections
- Design tokens in `tokens.css` shared across all components

This is the production pattern for real deployment but requires Node.js/npm and is not part of the default `full-edition` recipe.
