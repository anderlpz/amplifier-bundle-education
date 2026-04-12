# Education Pipeline — Agent & Recipe Directory

This session has access to a complete multi-modal education production pipeline. Use these agents and recipes to turn source content into finished educational products.

---

## Available Agents

| Agent | When to delegate | Primary output |
|-------|-----------------|----------------|
| `education:content-strategist` | Analyzing source material, defining voice/tone, creating section outline | `.design/CONTENT-STRATEGY.md`, section outline |
| `education:section-author` | Writing individual section markdown with YAML frontmatter and block composition | `.design/sections/*.md` |
| `education:visual-director` | Art direction, diagram specifications, concept image generation, aesthetic guide | Concept images, `AESTHETIC-GUIDE.md`, asset specs |
| `education:asset-builder` | Code-drawn SVG production from approved specs and design tokens | `public/diagrams/*.svg` |
| `education:deck-composer` | Condensing section content into presentation slides with speaker notes | `deck.html` |
| `education:narration-adapter` | Adapting prose into conversational lecture-style voiceover scripts | `audio/*.txt` voiceover scripts |
| `education:edition-manager` | Change detection, update planning, edition bookkeeping, rebuild orchestration | Update plans, `.edition/` artifacts |

---

## Available Recipes

| Recipe | Command | What it does |
|--------|---------|--------------|
| `full-edition` | `education:recipes/full-edition.yaml` | Complete pipeline: discover → shape → assets → deliverables |
| `discover` | `education:recipes/discover.yaml` | Parallax Discovery on source content |
| `shape-content` | `education:recipes/shape-content.yaml` | Content strategy + section authoring |
| `generate-assets` | `education:recipes/generate-assets.yaml` | Visual pipeline with approval gate |
| `produce-deliverables` | `education:recipes/produce-deliverables.yaml` | HTML site + deck + narration scripts |
| `update-edition` | `education:recipes/update-edition.yaml` | Detect changes, plan update, rebuild |

---

## The Pipeline

```
Source repo / document
        │
        ▼
  [Phase 1: Discover]
  Parallax Discovery → reconciliation.md
  (verified claims with file:line evidence)
        │
        ▼
  [Phase 2: Shape Content]
  content-strategist → CONTENT-STRATEGY.md
  section-author     → .design/sections/*.md
        │
        ▼
  [Phase 3: Generate Assets]  ← APPROVAL GATE
  visual-director → concept images + AESTHETIC-GUIDE.md
  asset-builder   → public/diagrams/*.svg
        │
        ▼
  [Phase 4: Produce Deliverables]
  section-author     → site.html
  deck-composer      → deck.html
  narration-adapter  → audio/*.txt
        │
        ▼
  Finished edition — site + deck + audio
```

**Key principle:** Each phase writes human-editable intermediate files. Edit any file before the next phase reads it. All content is in the workspace, not the bundle.

---

## Workspace Layout (Generated)

```
project/
├── .design/
│   ├── CONTENT-STRATEGY.md      ← human override point
│   ├── DESIGN-SYSTEM.md         ← human override point
│   ├── AESTHETIC-GUIDE.md       ← human override point
│   ├── VISUAL-STRATEGY.md       ← asset manifest
│   ├── reconciliation.md        ← Discovery output
│   └── sections/
│       ├── 01-*.md              ← generated section content
│       └── ...
├── .edition/
│   ├── manifest.json            ← edition metadata
│   ├── source-hashes.json       ← change detection
│   └── section-hashes.json
├── public/
│   └── diagrams/                ← generated SVG assets
├── audio/
│   ├── ch01-*.mp3               ← generated audio
│   └── ch01-*.txt               ← voiceover scripts
├── site.html                    ← deliverable: full site
└── deck.html                    ← deliverable: presentation
```
