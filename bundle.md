---
bundle:
  name: education
  version: 0.2.0
  description: Multi-modal education pipeline — turns source content into sites, presentations, and voiceover narration, with visual assets generated from a concept-first workflow

includes:
  - bundle: git+https://github.com/microsoft/amplifier-foundation@main
  - bundle: education:behaviors/education
---

# Education Product Pipeline

This bundle provides a complete multi-modal education production system. Point it at a repository or document and it produces a standalone reading site, a presentation deck, and chapter-by-chapter voiceover scripts — all from a single source of truth.
