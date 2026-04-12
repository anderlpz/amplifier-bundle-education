# Content Strategy: [Subject Name]

<!-- INSTRUCTIONS: This template is a starter for the content-strategist agent.
     The agent will generate the actual content strategy document for your project.
     This template shows the structure and explains what goes in each section.
     Replace all [bracketed placeholders] with your project-specific content.
     Lines marked <!-- COMMENT --> are instructions — remove them from the final document. -->

**Date:** [Date]
**Status:** [Draft | Final]
**Subject:** [Subject Name]

---

## Part 1: Writing Style Guide

### Voice

<!-- Describe the voice in specific, concrete terms — not generic ("be clear").
     The reference implementation used: "Someone who has mastered this material.
     Not someone who built it yesterday and is excited to show you — someone who has
     lived with it, internalized it, and can now teach it clearly to anyone." -->

[Voice description — who is speaking, what is their relationship to the material, what is their relationship to the audience]

The voice respects the audience's intelligence while understanding the gap. The reader is smart — they just haven't been introduced to this system yet.

### Tone

<!-- Educational register. University course. Specify the level — introductory survey course?
     Graduate seminar? Practitioner workshop? -->

[Tone calibration — educational register, experience level, depth of engagement]

### Approach: Inductive, Not Deductive

This is a foundational rule for all content in this project.

**Inductive** means: start with the concrete, then name the principle. Show the thing first. Then explain the concept behind it.

**Deductive** means: state the principle, then show examples. This is what textbooks do, and it's what AI-generated content defaults to. Do not do this.

**Example — wrong (deductive):**
> "[Abstract principle] means [explanation]. For instance, [example]..."

**Example — right (inductive):**
> "[Concrete thing that exists]. [What it does]. [Another concrete thing]. [Pattern you can see]. This pattern has a name: [principle name]."

### Audience Calibration

<!-- Who is the primary reader? Technical background? Motivation for reading?
     What can you assume they already know? -->

[Audience description — background, technical level, motivation]

**Vocabulary glossary — terms that get explained on first use:**

| Term | First-use explanation |
|------|----------------------|
| [Term] | [Plain-language explanation woven into a sentence] |
| [Term] | [Plain-language explanation woven into a sentence] |

<!-- Add all technical terms specific to your subject that need first-use explanation. -->

### Vocabulary Rules

**Never use:**
- [Hype words specific to your subject]
- "game-changing", "revolutionary", "superpower", "magic"
- "Wait, really?", "Here's the surprise", "This changes everything"
- Em dashes (— or `&mdash;`) — AI writing tell; use periods, colons, or restructure the sentence
- [Any verification methodology language in body copy — belongs in appendix only]

**Always:**
- Use proper technical names and explain them on first use
- Active voice: "[subject] does X" not "X is done by [subject]"
- Concrete over abstract: "[specific number] [specific thing]" not "several things"
- When citing numbers, make them precise

**Tone calibration examples:**

| Instead of this | Write this |
|----------------|------------|
| "[Hype framing]" | "[Concrete fact stated plainly]" |
| "[Manufactured drama]" | "[Direct statement of what is true]" |
| "[Vague abstraction]" | "[Specific, concrete example]" |

<!-- Add 3-5 before/after examples specific to your subject. -->

### Depth Calibration

| Section/Concept | Depth Level | Rationale |
|----------------|-------------|-----------|
| [Concept] | DEEP | [Why this deserves deep treatment] |
| [Concept] | OVERVIEW | [Why this gets one paragraph] |
| [Concept] | MENTION | [Why this is a footnote] |

<!-- DEEP = show internals, explain trade-offs, use code examples, explain WHY
     OVERVIEW = state clearly, one concrete example, move on
     MENTION = one sentence in context -->

### Anti-Patterns to Avoid

1. **[Anti-pattern name]** — [Description of the specific bad pattern]
2. **Dense walls of text with no visual relief** — Break content with code blocks, comparisons, diagrams. Each one teaches something.
3. **Dramatic framing** — If a fact is interesting, state it. Let the reader be surprised on their own.
4. **Equal-weight treatment** — Some concepts deserve depth. Others deserve a sentence. Weight them accordingly.
5. **Abstract before concrete** — Principles are earned. Show the thing, then explain the thinking behind it.

---

## Part 2: Content Outline

<!-- One section entry per chapter. The content dictates the chapter count — don't decide in advance. -->

### Section 1: [Title]

**Primary concept:** [ONE sentence — what this section teaches]

**Content:**
- [Content point with depth note]
- [Content point]

**Depth level:** [DEEP / OVERVIEW / MENTION]
[Rationale]

**Source VCs (internal):** [VC-XX, VC-YY — NEVER rendered in output]

**Visual assets:**
- [What diagram/table/comparison this section needs]

**Presentation mode:** [N] slides. [Brief description]

---

<!-- Repeat for each section -->

---

## Part 3: Site vs Presentation Strategy

### The Fundamental Principle

The SAME content powers both modes. The site is the source of truth. The presentation is a transformation of that content, not a separate artifact.

### Site Mode

[Description of how content reads in long-form site mode — scrollable sections, inline diagrams, generous whitespace]

### Presentation Mode

[Description of how content transforms into slides — condensation rules, slide count per section, speaker notes convention]

**Content transformation examples:**

| Site mode | Presentation mode |
|-----------|------------------|
| [Section type] | [N] slides — [brief description] |

---

## Part 4: Source Claim Mapping

<!-- INTERNAL USE ONLY — never renders in any deliverable.
     This table is for edition-manager to trace which sections are affected
     when source files change. -->

| Section | Source VCs | Cross-Cutting Insights |
|---------|-----------|----------------------|
| 1. [Title] | VC-01, VC-02 | XC-01 |
| 2. [Title] | VC-03, VC-04 | — |

---

## Part 5: What NOT to Say

<!-- A specific blocklist. Add anti-patterns, banned framings, and forbidden phrases
     specific to this subject. -->

1. **[Banned pattern]** — [Why it's wrong for this content]
2. **No [pattern]** — [Reason]

---

## Part 6: Multi-Modal Content Guidance

### Audio Narration

Audio narration is NOT a direct reading of the prose. Key adaptations:
- Replace visual references with verbal descriptions
- Read code only if short (1-3 lines); describe longer blocks
- Add verbal signposts where prose uses visual breaks
- Simplify tables into verbal lists

### Presentation Slides

- Each slide has ONE idea
- Visual assets get their own slides
- 3-5 bullets maximum per text slide
- The site's inline explanations become speaker notes

### The Inductive Principle Across All Modalities

No modality starts with an abstraction. Every modality starts with something concrete:
- Prose: concrete example → principle
- Audio: concrete scenario → architectural concept
- Slides: specific fact → general pattern
- Speaker notes: what the audience is seeing → what to say about it
