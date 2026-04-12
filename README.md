# amplifier-bundle-education

A reusable Amplifier bundle that turns source content into a complete multi-modal education product: a reading site, a presentation deck, and chapter-by-chapter voiceover scripts — all from a single source of truth.

---

## What It Does

Point this bundle at a repository or document and it produces:

- **A reading site** (`site.html`) — standalone HTML with sidebar TOC, interactive diagrams, presentation mode, and audio player
- **A presentation deck** (`deck.html`) — standalone HTML with arrow-key navigation, speaker notes, and slide overview
- **Audio narration scripts** (`audio/*.txt`) — lecture-style voiceover adapted for listening (not a verbatim reading of the prose)
- **Visual assets** (`public/diagrams/*.svg`) — code-drawn SVG diagrams built from an art-directed concept

The pipeline runs with one command and pauses for human review at the visual art direction gate.

---

## Quickstart

Add to your `bundle.md`:

```yaml
includes:
  - bundle: git+https://github.com/your-org/amplifier-bundle-education@main
```

Then run:

```bash
# Produce a complete edition from scratch
amplifier recipe execute education:recipes/full-edition.yaml \
  source_repo=/path/to/your/repo \
  subject_name="Your Subject Name"

# Update an existing edition after source changes
amplifier recipe execute education:recipes/update-edition.yaml \
  source_repo=/path/to/updated/repo
```

---

## Features

- **Parallax Discovery** — content is built from verified claims, not AI assumptions. Multi-agent, multi-pass investigation produces a reconciliation document with file:line evidence for every claim.
- **Inductive content structure** — the content-strategist enforces concrete-first, principle-second structure across all sections. No AI-default deductive writing.
- **Concept-first visual workflow** — generate visual targets with nano-banana, approve the art direction, then build production SVGs. Human judgment at every visual gate.
- **Three deliverables, one source** — site, deck, and audio are all generated from the same section markdown files. Edit a section, rebuild all three.
- **Edition management** — hash-based change detection identifies the minimum rebuild when source content changes. One command to update.
- **Paper Frame aesthetic (default)** — editorial white-card-on-warm-canvas design system. Customizable via design-system.md and AESTHETIC-GUIDE.md.

---

## Pipeline

```
Source repo
    │
    ▼ discover.yaml
Parallax Discovery → reconciliation.md
    │
    ▼ shape-content.yaml
Content strategy → Section markdown files
    │
    ▼ generate-assets.yaml (STAGED — approval gate)
Concept images → Art direction approval → Production SVGs
    │
    ▼ produce-deliverables.yaml
site.html + deck.html + audio/*.txt
```

---

## Agents

| Agent | Role |
|-------|------|
| `content-strategist` | Voice, tone, depth calibration, section outline |
| `section-author` | Section markdown + HTML site assembly |
| `visual-director` | Art direction, concept generation, aesthetic guide |
| `asset-builder` | Production SVG diagrams |
| `deck-composer` | Presentation slides + speaker notes |
| `narration-adapter` | Voiceover scripts (adapted for listening) |
| `edition-manager` | Change detection, update planning, edition bookkeeping |

---

## Built From

This bundle was extracted and generalized from the `amplifier-masterclass` project — a fully produced education product for the Amplifier framework that includes a 5,400-line standalone site, 16-slide presentation, 13 chapter narrations, and 9 code-drawn SVG diagrams.

All patterns in this bundle were validated against that production run.

---

## Documentation

See [docs/GUIDE.md](docs/GUIDE.md) for the full user guide, including:
- Complete pipeline walkthrough
- Art direction workflow
- Edition update workflow
- Content model reference
- Agent reference with delegation examples
- Troubleshooting

---

## License

MIT
