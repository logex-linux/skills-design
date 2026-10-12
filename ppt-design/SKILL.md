---
name: ppt-design
description: >-
  PPT and presentation visual design collection, distilled from 100 high-star
  GitHub repositories (712k combined stars) plus the design, apple-design, and
  slides skills. Produces genuinely beautiful decks — Apple-grade restraint,
  Anthropic-style editorial typography, and Codrops-level motion — as editable
  native PPTX, HTML slides, or PDF. Use when the user wants to create, redesign,
  or review a presentation, pitch deck, talk, or slide deck; needs slide design
  tokens, palettes, or type scales; wants to pick a slide framework; needs
  copywriting structure for slides; or wants to stop AI-generated decks from
  looking generic. Triggers include: "make a deck", "design slides",
  "presentation design", "pitch deck", "too generic", "AI slop", "which slide
  framework", "slide template", "配色", "排版", "幻灯片设计", "PPT 美化".
version: 1.0.0
---

# PPT Design

A complete visual-design system for presentations. Everything here traces to a
source: the 100 most-starred slide/presentation repositories, plus the `design`,
`apple-design`, and `slides` skills.

**The organizing principle: one token file, many renderers.** The ecosystem's
biggest gap is that Slidev uses CSS custom properties, PptxGenJS skills use JS
constants, SlideSpeak uses prose in a prompt, and Beamer uses `\definecolor` —
with no interchange format. This skill fixes that: define once in
[design-tokens.md](references/design-tokens.md), render to any target.

## How to use this skill

| The user wants to… | Read |
|---|---|
| Build a deck end-to-end | [workflow.md](references/workflow.md) |
| Colors, type scale, spacing, motion tokens | [design-tokens.md](references/design-tokens.md) |
| The design rules and the anti-slop checklist | [design-rules.md](references/design-rules.md) |
| Layout grids and slide archetypes | [layouts.md](references/layouts.md) |
| Animation and transitions | [motion.md](references/motion.md) |
| Choose a framework or generate native PPTX | [frameworks.md](references/frameworks.md) |
| Chart and data-slide design | [data-viz.md](references/data-viz.md) |
| Copywriting structure for slides | [copywriting.md](references/copywriting.md) |
| Find the source repositories | [resources.md](references/resources.md) |

## The 60-second version

**The five decisions that separate a beautiful deck from a generic one:**

1. **One accent color, used exactly once per slide.** Not one per deck — one
   *per slide*, landing on the single element that carries the takeaway. This is
   the most-violated rule in the entire source set.
2. **Action titles, never topic labels.** "Intervention reduced costs by 23%
   (p < 0.01)" — not "Results". The title *is* the finding.
3. **One idea per slide. Bullets never appear.** Sequential points become
   sequential slides. Max 6 bullets if you must; ≤40 words of body text.
4. **Asymmetric over centered.** "Everything centered in a vertical stack" is the
   single most-banned layout. Left axis, one margin, the eye never re-finds it.
5. **40% of every slide is intentional empty space.** TED's rule verbatim.

**The #1 tell of an AI-generated deck: an accent line directly under the title.**
Named as such by two independent repos. Use whitespace instead. (The only
sanctioned exceptions are *load-bearing* rules — a McKinsey 1px rule under
title+lead-in, or the Economist's red rule + 40×10px rectangle.)

## The banned list

Copy this into your head. These are the specific patterns the sources name as
machine-made:

| Banned | Why |
|---|---|
| Accent line under the title | the AI tell |
| Topic-label titles ("Results", "Overview") | the ghost-deck test catches these |
| Everything centered in a vertical stack | most-banned layout |
| Identical card grids repeated | "every page is a card grid" |
| Text wrapped in background cards | use spacing and weight instead |
| Nested backgrounds | one colored box inside another |
| `border-l-2` heavy borders | use `border-l` at `/20` |
| `#6366f1` indigo | the specific banned hex |
| Purple-on-white gradients, pastel rainbow | |
| Inter/Roboto/Arial as the **display** face | and don't over-use Space Grotesk |
| Purposeless drop shadows, mesh gradients, blobs, glassmorphism | |
| Legends where direct labels fit; gridlines where none are needed | |
| Bullet glyphs in a keynote register | |
| `autoFit: true` on text boxes | it *grows* the shape and causes overlap — use `shrinkText` |
| Whole-page screenshots exported as PPTX | a third of your slides become pictures |
| Thank-you slides | end on conclusions, leave them up for Q&A |
| Full-bleed background images on content slides | title and section dividers only |

## The non-negotiables

- **Never invent a number, name, or word.** One tool in the source set printed
  *43% growth* where the real figure was *12%*. Every figure traces to source
  material.
- **No `#` prefix on hex in PptxGenJS** — it silently corrupts the file.
- **Never reuse option objects** in PptxGenJS — the library mutates them in place.
  Use factory functions.
- **`shrinkText: true`, never `autoFit: true`.**
- **Only `transform` and `opacity`** in animation.
- **Contrast: design to 7:1, not 4.5:1.** Projectors lose contrast — a 4.5:1 pair
  can render as 3:1.
- **Validate three ways:** the file opens, the geometry is in bounds, and the
  render matches ground truth (SSIM/IoU, SHA-256 fingerprints).
- **Have an independent critic review, not the author model grading its own
  homework.**

## When the user is vague

Ask three things: **what's the deck for and who's the audience** (this decides
the register — keynote, academic, consulting, investor, internal), **how many
slides and how long is the talk**, and **what's the output format** (editable
PPTX / HTML / PDF — this decides the toolchain, and it's the one people get
wrong).

If they want editable PPTX and you were about to render HTML to images, stop —
see [frameworks.md](references/frameworks.md) for the fidelity law.
