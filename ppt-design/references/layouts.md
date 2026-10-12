# Layouts & Slide Archetypes

CSS grid templates for the standard slide archetypes, plus the composition rules
behind them. All authored at 1920×1080.

## The composition rules

**Asymmetric over centered.** "Everything centered in a vertical stack" is the
single most-banned layout in the source set. The reference is MultiLayout's
layout-4 — four frames at:

```css
/* MultiLayout layout-4 — the canonical asymmetric composition */
.frame-1 { top: 15%; left: 15%; width: 30%; height: 50%; }
.frame-2 { top: 40%; left: 5%;  width: 80%; height: 60%; }
.frame-3 { top: 0%;  left: 40%; width: 40%; height: 60%; }
.frame-4 { top: 50%; left: 75%; width: 22.5%; height: 42.5%; }
```

**Left axis, one margin.** Content aligns to a single left axis; the eye never
re-finds the margin.

**Evidence left, interpretation right.** Figure width capped at 60% of content
width in side-by-side; full-width centered when the figure's aspect ratio > 1.6.

**Vertical fill floor.** Content below the title bar must span ≥75% of the
content height. If everything ends in the upper half, something is wrong.

## The 12-column grid

```css
.slide {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: auto 1fr auto;
  gap: var(--space-4);
  padding: var(--margin-outer);
  width: var(--slide-width);
  height: var(--slide-height);
}
```

## Archetype: Title slide

Dark background, oversized display type, one accent, generous whitespace.

```css
.slide-title {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  align-content: center;
  background: var(--color-bg-dark);
  color: var(--color-text-dark);
  padding: var(--margin-outer);
}
.slide-title .eyebrow {
  grid-column: 1 / -1;
  font-family: var(--font-mono);
  font-size: var(--font-size-micro);
  letter-spacing: var(--tracking-label);
  text-transform: uppercase;
  color: var(--color-accent-dark);
  margin-bottom: var(--space-6);
}
.slide-title h1 {
  grid-column: 1 / 9;
  font-family: var(--font-display);
  font-size: var(--font-size-h1);
  font-weight: var(--weight-black);
  line-height: var(--leading-display);
  letter-spacing: var(--tracking-display);
  text-wrap: balance;
}
.slide-title .subtitle {
  grid-column: 1 / 7;
  font-size: var(--font-size-lead);
  color: var(--color-text-secondary-dark);
  margin-top: var(--space-6);
  line-height: var(--leading-body);
}
```

## Archetype: Section divider

Dark, minimal — the section name and nothing else. A progress indicator showing
where you are.

```css
.slide-section {
  display: grid;
  place-content: center;
  background: var(--color-bg-dark);
  color: var(--color-text-dark);
  padding: var(--margin-outer);
}
.slide-section .number {
  font-family: var(--font-mono);
  font-size: var(--font-size-small);
  color: var(--color-accent-dark);
  letter-spacing: var(--tracking-label);
}
.slide-section h2 {
  font-family: var(--font-display);
  font-size: var(--font-size-h1);
  font-weight: var(--weight-bold);
  line-height: var(--leading-display);
  letter-spacing: var(--tracking-display);
  margin-top: var(--space-4);
}
```

## Archetype: Action title + body (the workhorse)

The most common content slide. Action title as a complete sentence, then one
supporting visual or argument.

```css
.slide-content {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: auto 1fr;
  gap: var(--space-6);
  padding: var(--margin-outer);
  background: var(--color-bg);
  color: var(--color-text);
}
.slide-content .action-title {
  grid-column: 1 / 11;
  font-family: var(--font-display);
  font-size: var(--font-size-h3);
  font-weight: var(--weight-bold);
  line-height: var(--leading-heading);
  letter-spacing: var(--tracking-heading);
  text-wrap: balance;
  /* NO accent line underneath — use whitespace */
}
.slide-content .body { grid-column: 1 / 8; align-self: center; }
.slide-content .visual { grid-column: 8 / -1; align-self: center; }
```

## Archetype: Two-column (evidence / interpretation)

```css
.slide-split {
  display: grid;
  grid-template-columns: 6fr 5fr;   /* asymmetric, not 1fr 1fr */
  gap: var(--space-12);
  align-items: center;
  padding: var(--margin-outer);
}
.slide-split .evidence { grid-column: 1; }
.slide-split .interpretation { grid-column: 2; }
```

## Archetype: Big-number / stat callout

Oversized type for emphasis rather than color or boxes. A stat at 60pt, or
"around 15 percent of slide height."

```css
.slide-stat {
  display: grid;
  place-content: center;
  gap: var(--space-6);
  padding: var(--margin-outer);
  text-align: left;   /* left, not centered */
}
.slide-stat .figure {
  font-family: var(--font-display);
  font-size: var(--font-size-display);
  font-weight: var(--weight-black);
  line-height: 0.9;
  letter-spacing: var(--tracking-display);
  color: var(--color-accent);   /* the ONE accent on this slide */
}
.slide-stat .label {
  font-size: var(--font-size-lead);
  color: var(--color-text-secondary);
  max-width: var(--measure);
  line-height: var(--leading-body);
}
```

## Archetype: Feature/metric grid (use sparingly)

The default everyone reaches for and the one that makes decks look generic. Vary
it, and never repeat it 3+ slides running.

```css
.slide-metrics {
  display: grid;
  grid-template-columns: repeat(3, 1fr);   /* 3, not 4 — less cramped */
  gap: var(--space-6);
  padding: var(--margin-outer);
  align-content: center;
}
.metric { display: grid; gap: var(--space-2); }
.metric .value {
  font-family: var(--font-display);
  font-size: var(--font-size-h2);
  font-weight: var(--weight-bold);
  line-height: 1;
  letter-spacing: var(--tracking-display);
}
.metric .caption {
  font-size: var(--font-size-small);
  color: var(--color-text-secondary);
  line-height: var(--leading-dense);
}
```

## Archetype: Quote / pull-quote

```css
.slide-quote {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  align-content: center;
  padding: var(--margin-outer);
}
.slide-quote blockquote {
  grid-column: 2 / 10;
  font-family: var(--font-display);
  font-size: var(--font-size-h2);
  font-weight: var(--weight-regular);
  line-height: var(--leading-heading);
  letter-spacing: var(--tracking-heading);
  text-wrap: balance;
}
.slide-quote cite {
  grid-column: 2 / 10;
  font-family: var(--font-mono);
  font-size: var(--font-size-small);
  font-style: normal;
  letter-spacing: var(--tracking-label);
  text-transform: uppercase;
  color: var(--color-text-muted);
  margin-top: var(--space-6);
}
```

## Archetype: Table

Hairline structure, no zebra striping, generous cell padding.

```css
.slide-table { padding: var(--margin-outer); }
table { width: 100%; border-collapse: collapse; font-size: var(--font-size-small); }
thead th {
  text-align: left;
  font-weight: var(--weight-bold);
  font-size: var(--font-size-micro);
  letter-spacing: var(--tracking-label);
  text-transform: uppercase;
  color: var(--color-text-muted);
  padding: var(--space-3) var(--space-4);
  border-bottom: var(--boundary-rule);
}
tbody td {
  padding: var(--space-4);
  border-bottom: var(--boundary-rule);
  color: var(--color-text-secondary);
}
tbody tr:last-child td { border-bottom: none; }
.num { font-variant-numeric: tabular-nums; text-align: right; }
```

## Archetype: Timeline / process

A filling rail, 2px, filled to the current phase.

```css
.slide-timeline {
  display: grid;
  grid-template-columns: repeat(var(--steps, 4), 1fr);
  gap: var(--space-4);
  padding: var(--margin-outer);
  align-content: center;
}
.step { display: grid; gap: var(--space-3); }
.step .rail { height: 2px; background: var(--color-border); position: relative; }
.step.done .rail { background: var(--color-accent); }
.step .index {
  font-family: var(--font-mono);
  font-size: var(--font-size-micro);
  color: var(--color-text-muted);
  letter-spacing: var(--tracking-label);
}
.step h4 { font-size: var(--font-size-lead); font-weight: var(--weight-bold); }
.step p { font-size: var(--font-size-small); color: var(--color-text-secondary); line-height: var(--leading-dense); }
```

## Archetype: Closing / conclusions

Dark. The conclusions stay on screen during Q&A — this is not a decorative slide.

```css
.slide-end {
  display: grid;
  align-content: center;
  gap: var(--space-8);
  padding: var(--margin-outer);
  background: var(--color-bg-dark);
  color: var(--color-text-dark);
}
.slide-end ol { display: grid; gap: var(--space-4); counter-reset: c; }
.slide-end li {
  counter-increment: c;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: var(--space-4);
  font-size: var(--font-size-lead);
  line-height: var(--leading-body);
}
.slide-end li::before {
  content: counter(c, decimal-leading-zero);
  font-family: var(--font-mono);
  font-size: var(--font-size-small);
  color: var(--color-accent-dark);
}
```

## The chrome

Running head, footer, progress — the smallest text on the slide and never
accent-colored.

```css
.slide-chrome {
  position: absolute;
  inset: auto 0 0 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: var(--space-4) var(--margin-outer);
  font-family: var(--font-mono);
  font-size: 11px;               /* smallest text on the slide */
  color: var(--color-text-muted); /* never accent-colored */
  border-top: var(--boundary-rule);
}
.progress {
  position: absolute;
  bottom: 0; left: 0;
  height: 2px;
  background: var(--color-accent);
  transition: width var(--duration-slide) var(--ease-out-expo);
}
```

## Codrops techniques worth stealing

### The 30×30 grid (ShapesSlideshow)

A full-canvas grid you compose images into, for editorial collages:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(30, 1fr);
  grid-template-rows: repeat(30, 1fr);
  width: 100%; height: 100%;
}
```

### Outline display type

```css
.outline {
  color: transparent;
  -webkit-text-stroke: 2px currentColor;
}
```

### Glitch band-slice

```css
.glitch {
  position: relative;
}
.glitch::before,
.glitch::after {
  content: attr(data-text);
  position: absolute;
  inset: 0;
  clip-path: polygon(0 15%, 100% 15%, 100% 35%, 0 35%);
}
.glitch::before { left: 2px;  text-shadow: -2px 0 #ff0080; }
.glitch::after  { left: -2px; text-shadow: 2px 0 #00ffff; }
```

### Fluid display type

```css
.title { font-size: clamp(2rem, 6.5vw, 6rem); line-height: 0.9; }
```

**License note:** Codrops demos are free to learn from; attribute and never
redistribute a demo folder as a template.
