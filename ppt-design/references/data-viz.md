# Charts, Data Slides & Images

## Chart styling for projection

The four projection rules:

1. **Larger type.** Chart labels at `--font-size-small` (28px) minimum, axis
   titles at body size (40px). A 12px axis label is invisible from row five.
   **Chart text is never smaller than the slide's caption size.**
2. **Higher contrast.** `--chart-label: #55524b`, `--chart-axis: #837e74`. Muted
   greys that work on a monitor vanish on a projector with a washed-out lamp.
3. **Fewer gridlines.** Horizontal only, 1px, barely visible (`#e2e1de`). Drop
   the verticals entirely. Drop the chart border.
4. **No legend where direct labels fit.** Label series directly beside the lines
   in their own color.

### The chart grammar

```css
:root {
  --chart-label: #55524b;
  --chart-axis: #837e74;
  --chart-grid: #e2e1de;
  --chart-grid-width: 1px;
  --chart-stroke: 3px;          /* heavier than on screen */
  --chart-accent-stroke: 5px;   /* the key series */
}
```

- **No y-axis** where value labels sit on the bars
- **The key series in the accent and the heaviest stroke**
- **Forecasts as dashed outlines**
- **A two-part header:** bold descriptor + gray units line
- **An 11px `Source:` line bottom-left**
- **One chart per slide, redrawn natively**, with one message. The takeaway is
  the headline, not a caption.

### Never use default library colors

> "Graphviz/Mermaid default colors must never appear in final output. Pass
> `--theme`" (powerpoint-skill rule 25)

Chart libraries are theme-blind by default — every repo that uses one has to
manually re-skin it. Route all chart colors through your tokens:

```css
/* Marp's approach — the chart reads the theme, never hardcodes */
svg[data-marp-mermaid] {
  --marp-mermaid-background: transparent;
  --marp-mermaid-foreground: var(--color-text);
  --marp-mermaid-accent: var(--color-accent);
  --marp-mermaid-muted: var(--color-text-muted);
  --marp-mermaid-line: var(--color-border);
}
```

```js
// Chart.js — read the tokens, don't hardcode
const css = getComputedStyle(document.documentElement);
const token = (n) => css.getPropertyValue(n).trim();

new Chart(ctx, {
  data: { datasets: [{
    borderColor: token('--chart-1'),
    backgroundColor: token('--chart-1') + '20',
    borderWidth: 3,
  }]},
  options: {
    plugins: { legend: { display: false } },      // direct-label instead
    scales: {
      x: { grid: { display: false }, ticks: { color: token('--chart-axis'), font: { size: 28 } } },
      y: { display: false },                        // value labels on the bars
    },
  },
});
```

### Categorical color rules

- **Luminance-matched set**, separate from the accent ramp
- **Never red+green** for a binary contrast — use blue+orange
- The Economist's ordered series: `#006ba2`, `#3ebcd2`, `#379a8b`, `#ebb434` —
  blue, cyan, teal, amber; no red/green pair
- PowerPoint semantic set: positive `#0173B2`, negative `#DE8F05`, emphasis
  `#029E73`, neutral `#8C8C8C`
- **One meaning per color, deck-wide.** Color carries meaning, not decoration.

## The 10-second test

> If the audience can't get the slide from text alone in 10 seconds, it needs a
> diagram, table, chart, or figure.

## Data slide archetypes

### Big number

Oversized type for emphasis rather than color or boxes. 60pt, or "around 15
percent of slide height." One accent on the slide, on the figure.

### Comparison table

Hairline structure, no zebra striping, generous cell padding, `tabular-nums` on
numeric columns. See [layouts.md](layouts.md).

### Before / after

Two states side by side. Evidence left, interpretation right.

### Funnel / process

Value labels on each stage, the drop-off in the accent, no more than 5 stages.

### Timeline

A filling rail, 2px, filled to the current phase. See [layouts.md](layouts.md).

## Image treatment

### Full-bleed vs contained

**Full-bleed** — image fills the slide edge to edge. Use for title slides, section
dividers, and emotional beats. **Requires a scrim for text legibility.**

**Contained** — image respects the margin system. Use for content.

```css
.slide--fullbleed { padding: 0; }
.slide--fullbleed__media { position: absolute; inset: 0; }
.slide--fullbleed__media img {
  width: 100%; height: 100%;
  object-fit: cover;          /* the critical property — never stretch */
  object-position: 50% 50%;
  display: block;
}
.slide--fullbleed__scrim {
  position: absolute; inset: 0;
  background: linear-gradient(
    to top,
    rgb(0 0 0 / 0.75) 0%,
    rgb(0 0 0 / 0.35) 45%,
    rgb(0 0 0 / 0.05) 100%
  );
}
```

### The rules

- **`object-fit: cover`, always.** A stretched image is the fastest way to look
  amateur.
- **Bleed ≥ parallax.** If the image bleeds past the frame, it must move slower
  than the frame, or the illusion breaks.
- **Scrim is mandatory** on dark backgrounds; light gradient on light
  backgrounds.
- **Full-bleed on content slides is banned.** Reserve for title and section
  dividers.
- **Contrast check: white text on an image needs 4.5:1 *with the scrim***, and
  7:1 recommended.
- **Aspect-ratio discipline** — don't crop faces or key details. Set
  `object-position` deliberately.

### Duotone / overlay treatment

When an image clashes with the palette:

```css
.duotone { position: relative; }
.duotone img { filter: grayscale(1) contrast(1.1); }
.duotone::after {
  content: "";
  position: absolute; inset: 0;
  background: var(--color-accent);
  mix-blend-mode: multiply;
  opacity: 0.35;
}
```

### Image masks

```css
.mask-arch {
  border-radius: 50% 50% 0 0 / 60% 60% 0 0;
}
.mask-diagonal {
  clip-path: polygon(0 0, 100% 0, 100% 85%, 0 100%);
}
```

## When a chart is the wrong choice

- **One number** → a big-number slide, not a one-bar chart
- **Two numbers** → the two numbers as type, not a chart
- **A trend the audience can't act on** → cut it
- **Precise values the audience needs to read** → a table, not a chart
- **More than ~7 categories** → group the tail into "other," or split the slide

## The data-slide checklist

- [ ] Chart text ≥ caption size (28px web / 11pt PPTX)
- [ ] Contrast at 7:1 for labels and values
- [ ] Horizontal gridlines only, 1px, barely visible
- [ ] No legend where direct labels fit
- [ ] No y-axis where value labels sit on the bars
- [ ] Key series in the accent, heaviest stroke
- [ ] No default library colors — all routed through tokens
- [ ] One chart per slide, one message
- [ ] Takeaway is the headline, not a caption
- [ ] `Source:` line, bottom-left
- [ ] Images use `object-fit: cover`
- [ ] Scrim wherever text sits on an image
