# Content Model Reference

The content model defines how educational content is structured in section markdown files. Every section file has a YAML frontmatter block followed by markdown prose. Agents that read or write sections must follow this schema precisely.

---

## Section Frontmatter Schema

```yaml
---
title: string              # Section display title — e.g., "The Orchestrator"
chapter: integer           # Chapter number (1-based)
blocks:                    # Ordered list of content blocks in this section
  - type: prose            # Block type (see Block Types below)
    status: ready          # Status (see Status Values below)
    variant: null          # Variant (see Variant Options below)
    asset_ref: null        # Reference to generated asset, e.g. "FIG-04a"
source_claims: []          # VC-XX references to verified claims from reconciliation.md
                           # Internal use only — NEVER appear in reader-facing output
depends_on: []             # Chapter numbers this section builds on
---
```

**Required fields:** `title`, `chapter`, `blocks`
**Optional fields:** `source_claims`, `depends_on`

---

## Block Types

Each entry in `blocks:` has a `type` field. Valid types:

### `prose`
Body text. The main reading content. Paragraphs, subsections with `###` headings, inline code.

```yaml
- type: prose
  status: ready
```

### `diagram`
A code-drawn SVG diagram. Must have an `asset_ref` pointing to the file in `public/diagrams/`.

```yaml
- type: diagram
  status: ready
  variant: wide
  asset_ref: FIG-04a
```

### `table`
A structured comparison or data table. Rendered as a styled HTML table.

```yaml
- type: table
  status: ready
  variant: wide
```

### `code`
A code block. Language specified inline in the markdown fence.

```yaml
- type: code
  status: ready
  variant: annotated    # optional: annotated adds inline callouts
```

### `comparison`
Side-by-side comparison panels (e.g., Tools vs Hooks, before/after, OS kernel vs Amplifier kernel).

```yaml
- type: comparison
  status: ready
  variant: wide
```

### `callout`
Highlighted aside, pull quote, or key fact. Rendered with left accent border.

```yaml
- type: callout
  status: ready
```

### `audio`
Chapter audio narration player. References an MP3 in `audio/`.

```yaml
- type: audio
  status: stub          # stub until MP3 is generated
  asset_ref: ch04-the-kernel
```

### `chat`
Inline chat prompt block. A professor-style invitation to ask questions.

```yaml
- type: chat
  status: stub          # stub until chat endpoint is configured
```

### `vignette`
Short animated motion graphic (30-60 seconds). Video explainer for one concept.

```yaml
- type: vignette
  status: stub          # stub until video is produced
  asset_ref: VIG-07a
```

---

## Status Values

| Value | Meaning |
|-------|---------|
| `ready` | Content is complete, asset is available |
| `draft` | Content is drafted but needs review |
| `stub` | Placeholder — asset or content not yet produced |

---

## Variant Options

| Variant | Applies to | Meaning |
|---------|-----------|---------|
| `null` | Any | Default width (reading column, ~650px) |
| `wide` | diagram, table, comparison, code | Wide breakout (~1100px) |
| `annotated` | code | Code block with inline callout annotations |
| `interactive` | diagram | Clickable/pannable (e.g., D3 dotgraph viewer) |

---

## Asset Reference Convention

`asset_ref` is a short ID that maps to a file. Convention:

| Prefix | Type | Example |
|--------|------|---------|
| `FIG-` | Diagram SVG | `FIG-04a` → `public/diagrams/FIG-04a.svg` |
| `VIG-` | Vignette video | `VIG-07a` → `public/vignettes/VIG-07a.mp4` |
| `ch` | Audio narration | `ch04-the-kernel` → `audio/ch04-the-kernel.mp3` + `.txt` |

The visual strategy document (`.design/VISUAL-STRATEGY.md`) maintains the full manifest of all asset refs, their specs, and their status.

---

## Source Claims Traceability

`source_claims` holds internal VC-XX references to verified claims from the Parallax Discovery reconciliation document. These are:

- Used by `edition-manager` to detect which sections are affected when source content changes
- **Never rendered in any user-facing output** — not in HTML, not in speaker notes, not in narration scripts
- Written in the content strategy document for internal use only

Example:
```yaml
source_claims:
  - VC-07   # Orchestrator privilege level
  - VC-16   # str return boundary
  - XC-03   # uniquely privileged position
```

---

## Complete Section Example

```markdown
---
title: "The Orchestrator"
chapter: 6
blocks:
  - type: prose
    status: ready
  - type: comparison
    status: ready
    variant: wide
  - type: diagram
    status: ready
    variant: wide
    asset_ref: FIG-06a
  - type: callout
    status: ready
  - type: audio
    status: stub
    asset_ref: ch06-orchestrator
source_claims:
  - VC-07
  - VC-16
  - XC-03
depends_on: [4, 5]
---

# Chapter 6: The Orchestrator

One module gets live access to the full system at runtime.
[... prose content follows ...]
```

---

## Writing Style Rules

All section prose must follow these rules (enforced by `content-strategist`'s CONTENT-STRATEGY.md):

1. **Inductive, not deductive** — show the concrete thing first, then name the principle
2. **Explain terms on first use** — "The Coordinator — a Rust-backed registry where all modules register themselves"
3. **Active voice** — "The kernel loads modules" not "Modules are loaded by the kernel"
4. **No em dashes** — AI writing tell; use periods, colons, or restructure
5. **No hype words** — "game-changing", "revolutionary", "superpower", "magic" are banned
6. **No verification language in body** — "VC-XX" and "code-verified" appear in the strategy doc only
7. **No manufactured drama** — "Wait, really?" and "Here's the surprise" are banned
