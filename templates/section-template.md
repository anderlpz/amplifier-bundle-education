---
# SECTION FRONTMATTER — fill in all fields before writing content
# See @education:context/content-model.md for full schema reference

title: "Section Title"
chapter: 1

blocks:
  # List all content blocks in the order they appear in this section.
  # Each block maps to a rendered component in the HTML site.
  # Valid types: prose | diagram | table | code | comparison | callout | audio | chat | vignette
  # Valid statuses: ready | draft | stub
  # Valid variants: null | wide | annotated | interactive

  - type: prose
    status: draft

  # Example: a wide diagram with a generated SVG
  # - type: diagram
  #   status: stub           # 'stub' until the SVG is produced
  #   variant: wide
  #   asset_ref: FIG-01a     # Maps to public/diagrams/FIG-01a.svg

  # Example: a wide comparison layout
  # - type: comparison
  #   status: draft
  #   variant: wide

  # Example: a callout box
  # - type: callout
  #   status: draft

  # Example: a code block with annotations
  # - type: code
  #   status: draft
  #   variant: annotated

  # Example: audio narration player (stub until MP3 is generated)
  # - type: audio
  #   status: stub
  #   asset_ref: ch01-section-slug    # Maps to audio/ch01-section-slug.mp3

  # Example: inline chat prompt (stub until chat endpoint is configured)
  # - type: chat
  #   status: stub

# INTERNAL USE ONLY — never renders in any deliverable
# List the VC-XX verified claim IDs from reconciliation.md that this section draws from
source_claims:
  # - VC-01
  # - VC-02

# Chapter numbers this section requires the reader to have read first
depends_on: []
---

<!-- ==================== CONTENT BEGINS HERE ==================== -->
<!-- Follow the inductive structure: concrete example FIRST, then name the principle.
     Never start with an abstraction. Never start with "The principle of X means..." -->

# Chapter [N]: [Title]

<!-- Lead paragraph: Concrete. The thing that exists. What it does. No abstractions yet. -->

[Opening paragraph — start with a specific, concrete thing. Something the reader can hold in their mind.]

[Supporting paragraph — develop the concrete picture. Add the parts. Show how they relate.]

[Principle paragraph — NOW you can name the pattern or architectural concept. The principle has been earned by the concrete example that preceded it.]

<!-- Use ### for subsections within a chapter. Never ## (that's the chapter level). -->

### [Subsection Title]

[Content following the same inductive pattern.]

<!-- CODE BLOCKS: Use language-fenced markdown. Keep them short (5-10 lines maximum).
     If the code is longer, show the key part and describe the rest in prose. -->

```python
# Short, focused code example
# Comment explains WHY, not just what
```

<!-- COMPARISON TABLES: Use when two things need explicit contrast.
     Keep columns to 2-3. Keep rows to 3-6. Each cell is one fact. -->

| [Left concept] | [Right concept] |
|----------------|----------------|
| [Specific fact] | [Contrasting fact] |

<!-- CALLOUT BOXES: Use for key facts, warnings, or things worth remembering.
     NOT for decorative purposes. Each callout should teach something. -->

<!-- To render as a styled callout in HTML, wrap in a div with class="callout":
     <div class="callout">
     Key fact worth calling out here.
     </div>
     In markdown draft form, just use a blockquote: -->

> [Key fact or insight worth highlighting — short, specific, standalone]

---

<!-- ==================== PRESENTATION SLIDES ==================== -->
<!-- One idea per slide. If a second idea arrives, make a second slide.
     Visual assets get their own slides.
     Text slides: headline + 3-5 bullet points maximum.
     The section prose becomes speaker notes — don't repeat it in bullets. -->

## Presentation Slides

### Slide 1: [Title]

[Chapter eyebrow] · [Section eyebrow]

**[Slide Headline — short, declarative, specific]**

- [Bullet — one complete thought, 10-15 words]
- [Bullet — concrete, active voice]
- [Bullet — specific number or fact when possible]

<!-- Maximum 5 bullets. Each bullet is one idea. -->

### Slide 2: [Diagram Slide Title]

<!-- Diagram slides: no bullets. Just the diagram with a caption. -->

[Caption for the diagram — what is it showing?]

[`asset_ref: FIG-01a` — inline SVG will be embedded here]

---

<!-- ==================== SPEAKER NOTES ==================== -->
<!-- Full explanation for the presenter. Written in complete sentences.
     Includes: key points to emphasize, transition language to next slide.
     May include internal VC-XX refs for presenter reference.
     These are HIDDEN from the audience — only visible in presenter view. -->

## Speaker Notes

### Slide 1

[Full explanation for the presenter. What to emphasize verbally. Context that doesn't fit on the slide. Transition language to the next slide: "Now let's look at..."]

### Slide 2

[Speaker notes for the diagram slide. What the diagram shows. What to say while it's on screen. How to guide the audience's attention through it.]
