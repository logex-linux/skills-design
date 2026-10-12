# The Deck Workflow

End-to-end. Follow the phases in order — the most common failure is jumping to
design before the argument exists.

## Phase 0 — Establish the brief

Ask three things before anything else:

1. **What's the deck for and who's the audience?** → decides the register
   (keynote / academic / consulting / investor / internal)
2. **How many slides, and how long is the talk?** → ~1 slide per 45–90 seconds;
   a 20-minute talk is 13–20 slides
3. **What's the output format?** → editable PPTX / HTML / PDF. **This is the one
   people get wrong**, and it decides the toolchain. If they want editable PPTX
   and you were about to render HTML to images, stop and read
   [frameworks.md](frameworks.md).

## Phase 1 — The ghost deck

Write the deck as plain text, **one action-title line per slide**, before
designing anything.

```
1. Intervention reduced costs by 23% (p < 0.01)
2. Current triage costs $4.2M annually and scales linearly
3. Three prior approaches assume stationarity; ours does not
...
```

**If it reads as a coherent argument with no slide needed to explain it, the
argument works.** If a line is a topic label, the content isn't thought through
yet. Fix the argument before touching the design.

This is also where you catch invented numbers. Every figure traces to source
material *now*, not at the end.

## Phase 2 — Choose the register and palette

Register decides tone, type, motion, and density (see
[copywriting.md](copywriting.md)).

Palette: pick one of the three in [design-tokens.md](design-tokens.md), or
generate one following the rules:

1. **Start from the background**, not the accent
2. **Derive surfaces by mixing toward the background**
3. **Text levels are the same hue at three lightnesses**
4. **Generate the accent last** and test it against every surface
5. **Categorical chart colors are a separate, luminance-matched set**
6. **Check the dark variant by flipping roles, not inverting values**

Then run the **60-second palette QA**:
1. Squint — does one thing pop? If not, you have two accents
2. Convert to greyscale — does the hierarchy survive?
3. Check contrast at 7:1 for anything the audience must read
4. Count chromatic elements. More than three? Remove some.

## Phase 3 — Choose the toolchain

```
Editable PPTX needed?
├─ Template exists    → fill it (docxtemplater / OfficeCLI merge)
├─ No template        → python-pptx or PptxGenJS WITH design rules
├─ AI-generated       → ppt-master
├─ HTML is fine       → Slidev (best themes) or reveal.js (best talks)
├─ Markdown source    → Marp
├─ Spatial argument   → impress.js
└─ PDF + math         → Beamer
```

**Never promise editable PPTX from an HTML pipeline** without checking the
fidelity table in [frameworks.md](frameworks.md).

## Phase 4 — Design the slide system

Not the slides — the *system*. Decide once, apply everywhere:

- **Type scale** — from design-tokens.md, or a fixed PPT point scale
- **Spacing scale** — 8px base
- **Four slide roles** — title (dark), section divider (dark), content (light),
  closing (dark)
- **The chrome** — running head, footer, progress indicator
- **One radius value**, deck-wide
- **The one accent** and where it's allowed to land

**Write this down as a spec file** (e.g. `spec.md`) and re-read it before every
page. This is the single most effective defence against context-compression
drift and the "every page is a card grid" default.

## Phase 5 — Build the slides

**Hand-author pages; don't batch-generate them.** Cross-page visual consistency
depends on per-page authoring with full upstream context.

Per slide, in order:

1. **The action title** — complete sentence, ≤15 words, ≤2 lines
2. **The one idea** — if there are two, it's two slides
3. **The visual** — chart, diagram, image, or big number. The 10-second test:
   can the audience get it from text alone in 10 seconds? If yes, you may not
   need the visual; if no, you must have one.
4. **The accent** — exactly one element, landing on the takeaway
5. **The chrome** — footer, page number, source line

**Verify geometry before rendering:** every element satisfies
`y + h ≤ SH - M`. Compute `y` from the preceding element's known bottom, never
approximate.

**Layout diversity:** never the same layout for 3+ consecutive slides. Alternate
full-width / two-column / stat-callout / timeline / table / chart / diagram.

## Phase 6 — Render and look at it

**The render → look → fix loop is what separates good from generic.** An agent
that can *see* its slides fixes them; one that can't ships blind.

```bash
# HTML → PNG, one per slide
# (or OfficeCLI's built-in renderer — the only tool that works headless in CI)
```

Look for, in order:
1. **Overflow** — text outside the frame, clipped, or overlapping
2. **Alignment** — anything off the left axis by more than 0.05"
3. **Vertical fill** — does content span ≥75% of the height below the title?
4. **Accent count** — more than one chromatic element?
5. **Density** — ≤60% of the area has content; ≥40% is intentional empty space

## Phase 7 — Validate

**Three ways, not one:**

1. **The file opens.** A malformed part passes geometry and pixel checks but
   PowerPoint offers to "repair" it.
2. **The geometry is in bounds.** Every element satisfies `y + h ≤ SH - M`.
3. **The render matches ground truth.** SSIM / IoU / color histograms; record
   SHA-256 fingerprints so results are reproducible.

**Export is *blocked* (not warned)** when structure, text occupancy, number
provenance, or page bounds fail.

## Phase 8 — Independent critic

**Not the author model grading its own homework.** A separate pass, with the
pre-flight checklist from [design-rules.md](design-rules.md), looking for:

- Accent lines under titles
- Topic-label titles
- Repeated layouts
- Invented numbers
- Contrast failures
- Overflow

## Phase 9 — Ship with the script

**Regenerate from a build script; never hand-edit output.** The script ships next
to the file, so "warmer palette" or "10 slides" is one sentence.

```
deck/
  spec.md            # the design system, re-read per page
  build.py           # or build.js — regenerates the deck
  tokens.css         # or tokens.js — the palette and type scale
  slides/            # content, one file per slide or one section per file
  out/deck.pptx      # generated, never edited by hand
```

**Record portable taste as a plain file the user owns**, editable line by line,
that only ever *seeds* a choice — the current request always wins, and a new user
starts clean.

## The fast path

When the deck is small (≤10 slides) and the format is HTML:

1. Ghost deck (10 lines of plain text)
2. Pick a palette from design-tokens.md
3. Pick 4–5 archetypes from layouts.md
4. Build with the tokens
5. Render, look, fix
6. Run the pre-flight checklist

That's a good deck in well under an hour. The full workflow above is for decks
that matter.
