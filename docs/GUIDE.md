# Education Bundle — User Guide

This bundle turns source content into a complete multi-modal education product: a reading site, a presentation deck, and chapter-by-chapter audio narration scripts. Visual assets are generated through a concept-first workflow with a human art direction gate.

---

## What This Bundle Produces

From a source repository or document, the pipeline produces:

| Deliverable | File | Description |
|-------------|------|-------------|
| Reading site | `site.html` | Single-file standalone HTML — open in any browser |
| Presentation | `deck.html` | Standalone HTML presentation with arrow-key navigation |
| Audio scripts | `audio/chNN-*.txt` | Voiceover scripts per chapter (adapted for listening) |
| SVG diagrams | `public/diagrams/*.svg` | Code-drawn technical diagrams |

All deliverables are generated from section markdown files in `.design/sections/`. Edit those files and rebuild to get updated deliverables.

---

## Quickstart

Three commands to produce a complete edition:

```bash
# Install and activate the bundle
amplifier bundle add git+https://github.com/your-org/amplifier-bundle-education@main
amplifier bundle use education
```

Then in a session:

```bash
# 1. Full pipeline (one-shot command)
amplifier run "execute education:recipes/full-edition.yaml with source_repo=/path/to/your/repo and subject_name='Your Subject Name'"

# 2. Or interactive chat
amplifier
> execute the full-edition recipe with source_repo=/path/to/repo and subject_name="Your Subject Name"

# 3. Update after source changes
> execute education:recipes/update-edition.yaml with source_repo=/path/to/your/repo

# 4. Rebuild deliverables only (if you edited sections manually)
> execute education:recipes/produce-deliverables.yaml

# 5. Use agents directly
> Delegate to education:content-strategist to analyze the reconciliation
> Use the site-builder to rebuild site.html
```

The `full-edition` recipe will pause at the visual art direction gate (Phase 3) for your approval. Everything else runs automatically. Discovery defaults to quick (single-pass). For complex repos, use deep discovery:

```bash
amplifier run "execute education:recipes/full-edition.yaml with source_repo=/path/to/repo and subject_name='Complex System' and discovery_depth=deep"
```

---

## Prerequisites

**Required for all deliverables:**
- [Amplifier](https://github.com/microsoft/amplifier) installed and configured
- **An LLM provider** — at least one API key configured. Every pipeline step uses an LLM. Supported providers: Anthropic (`ANTHROPIC_API_KEY`), OpenAI (`OPENAI_API_KEY`), or Google (`GOOGLE_API_KEY`).
- **`GOOGLE_API_KEY`** — required for concept image generation in Phase 3 (nano-banana uses Gemini for visual asset concepts)

**Required for audio production (MP3 synthesis):**
- **`OPENAI_API_KEY`** — required for TTS synthesis via the OpenAI speech API (also works as your LLM provider)
- **ffmpeg** — required for OPUS-to-MP3 conversion and multi-chunk audio concatenation. Install with:
  - macOS: `brew install ffmpeg`
  - Ubuntu/Debian: `sudo apt install ffmpeg`
  - Fedora: `sudo dnf install ffmpeg`
- **openai Python package** — `pip install openai` or `uv add openai`

**Optional:**
- **`design-intelligence-enhanced` bundle** — used by the interactive recipe's design review gate (Gate 2). Not required for the flat/autonomous pipeline.

If you don't need MP3 audio, set `produce_audio: false` in the recipe context to skip narration and TTS entirely.

---

## Installation

Add this bundle to your project's `bundle.md`:

```yaml
---
bundle:
  name: my-project
  version: 1.0.0

includes:
  - bundle: git+https://github.com/your-org/amplifier-bundle-education@main
---
```

Or reference it in a behavior YAML:

```yaml
includes:
  - bundle: git+https://github.com/your-org/amplifier-bundle-education@main
```

---

## The Pipeline Explained

### Phase 1: Discovery (`discover-quick.yaml` or `discover-deep.yaml`)

The pipeline starts with verified understanding of the source material. Two modes are available:

| Mode | Recipe | Token cost | Use when |
|------|--------|-----------|----------|
| **Quick** (default) | `discover-quick.yaml` | ~1× repo volume | Most repos. Single-pass structured reading with teaching lens. |
| **Deep** (opt-in) | `discover-deep.yaml` | ~3-5× repo volume | Large, complex, or poorly-documented repos. Full Parallax 3-pass with cross-validation. |

Set `discovery_depth: "deep"` in the `full-edition.yaml` context to use deep discovery. The default is `"quick"`.

Both modes produce the same output: `.design/reconciliation.md` — a verified knowledge base with VC-XX claims, core concepts in teaching order, design decisions, and a diagram content guide.

**Input:** Source repository path
**Output:** `.design/reconciliation.md` — verified knowledge with file:line evidence

Each knowledge claim gets a VC-XX identifier. These IDs are used internally to trace which sections are affected when the source changes. They never appear in any reader-facing output.

**Human override point:** Edit `reconciliation.md` after the discovery completes. If a claim is wrong or missing, fix it before running `shape-content`.

### Phase 2: Content Shaping (`shape-content.yaml`)

**Step 1 — Content strategy:** The `content-strategist` agent reads the reconciliation and produces `CONTENT-STRATEGY.md`. This document defines voice, tone, audience calibration, depth rules, vocabulary, and the section outline.

**Step 2 — Section authoring:** The `section-author` agent writes each section markdown file following the content strategy. Each file has YAML frontmatter (block composition, source claims, chapter metadata) and markdown content (prose, presentation slides, speaker notes).

**Output:** `.design/CONTENT-STRATEGY.md` and `.design/sections/*.md`

**Human override points:**
- Edit `CONTENT-STRATEGY.md` to change voice, depth rules, or vocabulary
- Edit any section markdown directly to revise content
- Add/remove blocks in section frontmatter to change what gets rendered

### Phase 3: Asset Generation (`generate-assets.yaml`) — STAGED

This is the only phase with an approval gate. Visual direction requires human judgment.

**Stage 1 (before approval):**
- `visual-director` reads sections, builds the asset catalog, establishes aesthetic direction
- Generates concept images using nano-banana (multiple directions, multiple styles)
- Writes `AESTHETIC-GUIDE.md` (draft) and `VISUAL-STRATEGY.md`

**Approval gate:** Review the concepts in `.design/concepts/`. Approve to proceed, or provide art direction guidance (e.g., "use concept 3 as the direction", "make it warmer", "less technical, more editorial").

**Stage 2 (after approval):**
- `visual-director` extracts style specs from approved concepts, writes final `AESTHETIC-GUIDE.md`
- `asset-builder` produces production SVG files for each diagram spec

**Output:** `public/diagrams/*.svg`

**Human override points:**
- Replace any concept image in `.design/concepts/` before approving
- Edit `AESTHETIC-GUIDE.md` to adjust color tokens, typography, or shape vocabulary
- Hand-edit any SVG in `public/diagrams/` after production

### Phase 4: Deliverable Production

Two variants — choose based on how much control you want:

| Recipe | Mode | Use when |
|--------|------|----------|
| `produce-deliverables.yaml` | Autonomous | Full-edition pipeline, rebuilds, CI |
| `produce-deliverables-interactive.yaml` | Interactive | First run, reviewing voice/design, expensive TTS |

**Step order (both variants):**

1. `audio/*.txt` — narration scripts written by `narration-adapter` (adapted for listening, not verbatim)
2. `audio/*.mp3` — synthesized from scripts via OpenAI TTS (requires `OPENAI_API_KEY` and `ffmpeg`)
3. `site.html` — built by `section-author` from all section files, with audio player wired to any MP3s found
4. `deck.html` — built by `deck-composer` from presentation slide blocks in sections
5. Verification — checks all expected outputs exist and are well-formed

Audio is produced first so the site builder can discover which MP3 files exist and wire the audio player (AUDIO_FILES map, nav pill, per-chapter TOC buttons, scroll sync, auto-advance).

**Interactive mode** pauses at two gates:

- **Gate 1 (after narration scripts, before TTS):** Review scripts in `audio/*.txt` before spending on TTS API calls. The gate shows word counts, estimated duration, and estimated cost. Approve to proceed, deny to revise scripts first.
- **Gate 2 (after site build, before deck):** A design-intelligence agent reviews `site.html` against the Paper Frame tokens, typography stack, layout constraints, and audio player contract. The gate shows the design review findings. Approve to build the deck (your feedback is passed to the deck builder via `{{_approval_message}}`), deny to revise the site first.

```bash
# Interactive — pauses for review at two gates
amplifier recipe execute education:recipes/produce-deliverables-interactive.yaml

# Autonomous — runs straight through (called by full-edition.yaml)
amplifier recipe execute education:recipes/produce-deliverables.yaml
```

Set `produce_audio: false` (flat version only) to skip narration and TTS entirely.

All intermediate files are human-editable. Edit a section, rebuild the deliverable.

---

## Art Direction

The visual pipeline is designed to be art-directed. You have two control points:

### 1. The Approval Gate (Recipe Level)

When you approve the art direction stage, include guidance in your message:

```
"Proceed. Build from concept 3 — the editorial/typographic direction.
Make the color palette warmer, closer to makingsoftware.com.
For the comparison diagrams, use the two-panel approach from concept 5."
```

Your guidance is passed to `visual-director` as `{{_approval_message}}`.

### 2. The Aesthetic Guide (File Level)

After the approval gate, `visual-director` writes `.design/AESTHETIC-GUIDE.md` with production color tokens and specifications. Edit this file before asset production begins (or between individual diagram builds) to fine-tune the visual language.

```markdown
## Color Tokens
| Token | Value | Usage |
|-------|-------|-------|
| --bg | #EDEBE6 | Background ← adjust this value
```

---

## Updating Editions

When your source content changes:

```bash
amplifier recipe execute education:recipes/update-edition.yaml \
  source_repo=/path/to/updated/repo
```

The recipe:
1. Hashes source files and detects what changed
2. Traces changes through verified claims → sections → assets
3. Produces an update plan (minimum rebuild set)
4. Pauses for your approval
5. Rewrites affected sections, regenerates affected assets, rebuilds deliverables
6. Bumps the edition number

**What stays unchanged:** Any section, asset, or deliverable not connected to changed source content. The pipeline only rebuilds what it needs to.

### Edition Numbering Convention

| Change | Version |
|--------|---------|
| Source factual update (1-3 sections) | 1.0 → 1.1 |
| Major content restructure (4+ sections) | 1.0 → 2.0 |
| Visual restyling only | 1.0 → 1.0.1 |

---

## Content Model

Every section file has this structure:

```markdown
---
title: "Section Title"
chapter: N
blocks:
  - type: prose         # or diagram, table, code, comparison, callout, audio
    status: ready       # or draft, stub
    variant: wide       # optional: wide, annotated, interactive
    asset_ref: FIG-04a  # optional: links to public/diagrams/FIG-04a.svg
source_claims:          # internal only — VC-XX ids for change detection
  - VC-07
depends_on: [4, 5]
---

# Chapter N: Title

[Section prose — inductive structure: concrete first, principle second]

## Presentation Slides
### Slide 1: [Title]
[bullets]

## Speaker Notes
### Slide 1
[full explanation for presenter]
```

See `@education:context/content-model.md` for the full schema reference.

---

## Individual Agent Reference

You can delegate to any agent directly without running the full recipe:

```bash
# Write a single section
amplifier delegate education:content-strategist "Analyze the reconciliation and write the content strategy"

# Write a specific section
amplifier delegate education:section-author "Write section 7 — Tools vs Hooks, following the content strategy"

# Build just the site
amplifier delegate education:section-author "Build site.html from the completed sections in .design/sections/"

# Generate concepts for a specific diagram
amplifier delegate education:visual-director "Generate concept images for FIG-04a — the kernel components diagram"

# Build a single diagram
amplifier delegate education:asset-builder "Build public/diagrams/FIG-04a.svg from the spec in AESTHETIC-GUIDE.md"

# Write a narration script
amplifier delegate education:narration-adapter "Write the voiceover script for chapter 7"

# Check what changed
amplifier delegate education:edition-manager "Detect changes in the source repo and produce an update plan"
```

---

## Workspace Layout

After a complete run:

```
project/
├── .design/
│   ├── CONTENT-STRATEGY.md      ← human override: voice, depth, vocabulary
│   ├── DESIGN-SYSTEM.md         ← human override: color, typography, layout
│   ├── AESTHETIC-GUIDE.md       ← human override: diagram style tokens
│   ├── VISUAL-STRATEGY.md       ← asset manifest
│   ├── reconciliation.md        ← Discovery output (source of truth for facts)
│   └── sections/
│       ├── 01-introduction.md
│       └── ...
├── .edition/
│   ├── manifest.json            ← edition metadata
│   ├── source-hashes.json       ← for change detection
│   ├── section-hashes.json
│   └── asset-hashes.json
├── public/
│   └── diagrams/                ← generated SVGs
├── audio/
│   ├── ch01-intro.txt           ← voiceover scripts
│   ├── ch01-intro.mp3           ← synthesized audio (TTS)
│   └── ...
├── site.html                    ← finished reading site
└── deck.html                    ← finished presentation
```

---

## Troubleshooting

**The content strategy sounds generic:**
Edit `.design/CONTENT-STRATEGY.md` — adjust the voice description, add more specific vocabulary rules, tighten the depth calibration table. Then re-run `shape-content` for the sections you want rewritten.

**The diagrams don't match the aesthetic I wanted:**
Edit `.design/AESTHETIC-GUIDE.md` and then re-run `asset-builder` for the affected diagrams. Or edit the SVG files directly in `public/diagrams/`.

**A section has wrong facts:**
Edit the section markdown in `.design/sections/`. Rebuild the deliverable by re-running `produce-deliverables`. If the wrong facts came from the reconciliation, edit that too and note the correction.

**The update plan is rebuilding too much:**
Review `.design/UPDATE-PLAN.md` before approving. In your approval message, specify which sections to skip: "Approve. Skip sections 3 and 5 — those facts didn't actually change in a way that affects the educational content."

**The narration sounds like someone reading a document:**
Delegate to `education:narration-adapter` to rewrite specific audio scripts. Provide feedback: "Rewrite ch07-tools-vs-hooks.txt — it should feel like a lecture, not like reading the site aloud. Shorter sentences. More direct address. Add more verbal signposts."

---

## Architecture Notes

The bundle follows the Amplifier mechanism/policy principle:
- **Bundle = capability** — the agents, context, and design artifacts that define the pipeline's capabilities
- **Recipes = orchestration** — what order to do things, where approval gates are
- **Workspace = policy** — what subject, what audience, what voice, what aesthetic

Content never lives inside the bundle. It lives in your workspace. The bundle provides the process; your workspace provides the substance.

For architecture background, see the EDUCATION-MODULE-ARCHITECTURE.md document in the reference implementation project.
