# Design Tokens

One file, many renderers. Define here, then map to CSS custom properties,
PptxGenJS constants, python-pptx, or Beamer `\definecolor`.

**The 60-second palette QA** (run before shipping):
1. Squint at the slide — does one thing pop? If not, you have two accents.
2. Convert to greyscale — does the hierarchy survive? If not, you're relying on
   hue for hierarchy.
3. Check contrast at 7:1 for anything the audience must read.
4. Count chromatic elements. If it's more than three, remove some.

## Canvas and geometry

```css
--slide-width: 1920px;    /* author at this size, scale the stage */
--slide-height: 1080px;
--slide-ratio: calc(16 / 9);

--margin-outer: 128px;    /* 6.7% of width — between Shower's 9.4% and MultiLayout's 5% */
--margin-inner: 96px;
--margin-tight: 48px;
--margin-hair: 24px;

--baseline: 8px;
--space-1: 8px;   --space-2: 16px;  --space-3: 24px;  --space-4: 32px;
--space-6: 48px;  --space-8: 64px;  --space-12: 96px; --space-16: 128px;
```

**The 8px base is settled, not a preference.** Two independent repos converge on
it (WebSlides' "multiples of 8", guizang's Carbon 2× grid). Use it.

**Minimum margin: 0.5" on all four sides** on a 10"×5.625" canvas → content area
9"×4.625". Web equivalent: 48px (Stripe), 56px (Bauhaus), 80px+ (TED).

## Color — the five layers

Strictly ordered. Never skip one; never add a sixth.

```
1. background    — the slide canvas. One value per slide.
2. surface       — cards, panels, table headers, code blocks. A step off background.
3. border        — the weakest boundary. Spacing/alignment first, border second.
4. text          — three levels only: primary / secondary / muted.
5. accent        — exactly ONE hue.
```

### The palette-generation rules

1. **Start from the background, not the accent.** Pick the canvas first.
2. **Derive surfaces by mixing toward the background**, not by picking a new hue.
3. **Text levels are the same hue at three lightnesses.** Never three different
   greys that happen to look similar.
4. **Generate the accent last**, and test it against every surface it touches.
5. **Categorical chart colors are a separate, luminance-matched set** — don't
   derive them from the accent ramp.
6. **Check the dark variant by flipping roles, not inverting values.** A warm
   white, not `#ffffff`.

### Palette A — Apple-like

Near-neutral canvas, warm off-white, restrained accent, generous whitespace, one
accent, tight type.

```css
:root {
  --color-bg: #fbfbfd;                 /* Apple's off-white */
  --color-surface: #f5f5f7;            /* Apple's section grey */
  --color-surface-raised: #ffffff;
  --color-border: #d2d2d7;
  --color-border-strong: #86868b;

  --color-text: #1d1d1f;               /* three levels, all near-black */
  --color-text-secondary: #6e6e73;
  --color-text-muted: #86868b;

  --color-accent: #0071e3;             /* ONE accent, deepened for projection */
  --color-on-accent: #ffffff;

  --color-positive: #34c759;
  --color-caution: #ff9500;
  --color-negative: #ff3b30;

  --font-display: -apple-system, "SF Pro Display", "Helvetica Neue", Arial, sans-serif;
  --font-body: -apple-system, "SF Pro Text", "Helvetica Neue", Arial, sans-serif;
  --font-mono: "SF Mono", Menlo, monospace;

  /* Dark — flip roles */
  --color-bg-dark: #000000;
  --color-surface-dark: #1d1d1f;
  --color-border-dark: #424245;
  --color-text-dark: #f5f5f7;
  --color-text-secondary-dark: #a1a1a6;
  --color-accent-dark: #2997ff;        /* lighter in dark mode */

  --chart-1: #0071e3;  --chart-2: #34c759;  --chart-3: #ff9500;
  --chart-4: #af52de;  --chart-5: #5ac8fa;  --chart-6: #8e8e93;
}
```

### Palette B — Anthropic-like

Warm paper canvas, serif display, terracotta accent. Editorial, restrained,
print-heritage.

```css
:root {
  --color-bg: #faf9f5;                 /* warm paper */
  --color-surface: #f0eee6;
  --color-surface-raised: #ffffff;
  --color-border: #e0ddd2;
  --color-border-strong: #b8b2a3;

  --color-text: #141413;
  --color-text-secondary: #55524b;
  --color-text-muted: #837e74;

  --color-accent: #c96442;             /* terracotta — ONE per deck */
  --color-on-accent: #ffffff;

  --color-positive: #6a994e;
  --color-caution: #bc6c25;
  --color-negative: #9b2226;

  --font-display: "Copernicus", "Tiempos Headline", Georgia, serif;
  --font-body: "Styrene B", "Neue Haas Grotesk", "Helvetica Neue", Arial, sans-serif;
  --font-mono: "Berkeley Mono", "JetBrains Mono", "SF Mono", Menlo, monospace;

  --color-bg-dark: #1a1917;
  --color-surface-dark: #26241f;
  --color-border-dark: #3d3a34;
  --color-text-dark: #f5f3ee;
  --color-text-secondary-dark: #b8b2a7;
  --color-accent-dark: #d97b56;

  --chart-1: #c96442;  --chart-2: #4a7c59;  --chart-3: #3d5a80;
  --chart-4: #bc6c25;  --chart-5: #7d5ba6;  --chart-6: #6c757d;
}
```

### Palette C — Bold / editorial

High contrast, one aggressive accent, oversized display type, grid-breaking
layouts.

```css
:root {
  --color-bg: #ffffff;
  --color-surface: #f4f4f4;
  --color-surface-raised: #ffffff;
  --color-border: #e5e5e5;
  --color-border-strong: #111111;

  --color-text: #111111;               /* near-black, high contrast */
  --color-text-secondary: #444444;
  --color-text-muted: #777777;

  --color-accent: #ff3b00;             /* aggressive single accent */
  --color-on-accent: #ffffff;

  --color-positive: #00875a;
  --color-caution: #ff8b00;
  --color-negative: #de350b;

  --font-display: "Anton", "Archivo Black", "Helvetica Neue", Impact, sans-serif;
  --font-body: "Inter", "Helvetica Neue", Arial, sans-serif;
  --font-mono: "JetBrains Mono", "SF Mono", Menlo, monospace;

  --color-bg-dark: #0a0a0a;
  --color-surface-dark: #1a1a1a;
  --color-border-dark: #333333;
  --color-text-dark: #fafafa;
  --color-text-secondary-dark: #a3a3a3;
  --color-accent-dark: #ff5722;

  --chart-1: #ff3b00;  --chart-2: #00875a;  --chart-3: #0065ff;
  --chart-4: #ff8b00;  --chart-5: #5243aa;  --chart-6: #6b778c;
}
```

### Dark deck rules

Dark decks are **not** light decks with inverted colors:

| | Light deck | Dark deck |
|---|---|---|
| Body size | 40px | **44–48px** — light-on-dark needs more size |
| Accent | mid-tone, saturated | **lighter/desaturated** — saturated accents vibrate |
| Shadows | soft, large blur | mostly absent; use borders |
| Elevation | shadow | **lighter surface** |
| Image scrim | light gradient | **dark gradient, mandatory** |

Marp's `light-dark()` pattern is the correct implementation — one token, two
values, resolved by `color-scheme`:

```css
section {
  --color-foreground: light-dark(#455a64, #fff8e1);
  --color-highlight: light-dark(#0288d1, #81d4fa);   /* LIGHTER in dark mode */
  color-scheme: light;
}
```

### The adaptive accent ramp (best light/dark strategy found)

Geist's `accents-1..8` inverts between modes so `accents-2` always means "one
step from background" — it never needs a separate dark palette:

```css
:root {
  --accents-1: var(--color-bg);
  --accents-2: var(--color-surface);
  --accents-3: var(--color-border);
  --accents-4: var(--color-border-strong);
  --accents-5: var(--color-text-muted);
  --accents-6: var(--color-text-secondary);
  --accents-7: var(--color-text);
  --accents-8: var(--color-accent);
}
[data-theme="dark"] { /* same roles, dark values — the ramp stays meaningful */ }
```

## Typography

```css
--font-display: "Copernicus", "Tiempos Headline", Georgia, serif;
--font-body: "Styrene B", "Neue Haas Grotesk", "Helvetica Neue", Arial, sans-serif;
--font-mono: "Berkeley Mono", "JetBrains Mono", "SF Mono", Menlo, monospace;

/* Type scale — authored at 1920×1080. 14px hard floor. */
--font-size-micro: 20px;
--font-size-small: 28px;
--font-size-base: 40px;
--font-size-lead: 48px;
--font-size-h4: 48px;
--font-size-h3: 64px;
--font-size-h2: 88px;
--font-size-h1: 120px;
--font-size-display: 176px;

--leading-display: 1.05;
--leading-heading: 1.2;
--leading-body: 1.5;
--leading-dense: 1.4;
--leading-loose: 1.7;

--tracking-display: -0.02em;   /* negative as it grows */
--tracking-heading: -0.01em;
--tracking-body: 0;
--tracking-label: 0.08em;      /* positive for small caps */

--weight-regular: 400;
--weight-medium: 500;
--weight-bold: 700;
--weight-black: 900;

--measure: 60ch;               /* max line length */
```

### The type-scale rules

- **Two families maximum** (display + body). Academic decks use **one**.
- **Weight hierarchy, not family hierarchy.** Geist: h1 800 → h2 700 → h3 600 →
  h4 500 as size drops. Penguin: all headings `font-display font-bold`, size only.
- **Display leading 0.8–1.0. Body 1.4–1.7.** MultiLayout title `line-height: 0.8`
  at 6em; Economist prose `1.7` at a **65-character measure**.
- **Negative tracking on display, positive on small caps.** `-0.02em` (Runway,
  Stripe), `-0.025em` (Linear titles), `-0.035em` (Linear cover). Small mono
  labels: `letter-spacing: 0.18em` uppercase.
- **`text-wrap: balance` on headings.**
- **Never center-align body paragraphs.** Left-align everything except slide
  titles and axis labels.
- **Mono has exactly two jobs:** code, and small uppercase letterspaced metadata
  (eyebrows, labels, slide numbers, metric captions).
- **Serif + sans in one headline is legitimate.** Shapes: light geometric sans
  (300, uppercase) with a high-contrast serif for `<em>`/`<strong>`. McKinsey:
  Gelasio serif action titles + Arimo sans body.
- **Telegraphic copy.** "Intervention reduced costs by 23% (p < 0.01)" not "Our
  study found that the intervention reduced costs significantly."
- **Banned as display faces:** Inter, Roboto, Arial, system fonts — and don't
  over-rely on Space Grotesk.

### PPT point sizes (for native PPTX)

| Element | Size |
|---|---|
| Cover | 48 |
| Section | 36 |
| Title | 22 |
| Subtitle | 18 |
| Card title | 18 |
| Body | 15 |
| Table head | 13 |
| Table cell | 12 |
| Caption | 11 |
| Stat | 60 |

**Never below 10pt.** Academic variant: action title 24–28 / section 20–22 /
body 20 / chart labels 16–18 / citations 12–14.

### Fluid type

```css
--title-size: clamp(2rem, 6vw, 5rem);
--subtitle-size: clamp(0.875rem, 2vw, 1.25rem);
--body-size: clamp(0.75rem, 1.2vw, 1rem);
```

Codrops uses `--fontsize-stitle: 12vw`; Shapes `clamp(2rem, 6.5vw, 6rem)`.

## Layout

```css
--grid-columns: 12;
--grid-gap: 32px;
--grid-gap-dense: 16px;
```

See [layouts.md](layouts.md) for the archetype grid templates.

## Material

```css
--radius-sm: 4px;
--radius-md: 12px;
--radius-lg: 24px;
--radius-pill: 999px;

--shadow-card: 0 1px 2px rgb(0 0 0 / 0.04), 0 4px 16px rgb(0 0 0 / 0.06);
--shadow-raised: 0 2px 4px rgb(0 0 0 / 0.06), 0 12px 40px rgb(0 0 0 / 0.10);
```

**One radius value, deck-wide.** 4px (Notion, Linear keys), 5px (geist, penguin),
6px (Linear bars), 8px (Stripe cards), 10–12px (Linear panels). Academic Beamer
and most consulting decks use **zero** radius. Never mixed casually.

**Boundary strength ladder** — pick by relationship, not taste:

```css
--boundary-space: 32px;                        /* weakest — spacing alone */
--boundary-rule: 1px solid var(--color-border);
--boundary-tint: var(--color-surface);
--boundary-outline: 1px solid var(--color-border-strong);
--boundary-panel: var(--color-surface);        /* strongest */
```

**1px hairlines for structure, 3–4px bars for emphasis.** Rules: `#CCCCCC`
(academic), `#D6DEE6` (McKinsey under the title), `#E3E8EE` (Stripe column
guides), `#E9E9E7` (Notion dividers). Emphasis bars: 3px left border on
`#F0F4F8` (McKinsey), 4px on theorem boxes.

**Atmosphere over flat fill** when depth is wanted: two radial gradients at
10–15% opacity over the base, or a 50×50px grid at `rgba(255,255,255,0.03)`.

## Motion

```css
--duration-instant: 120ms;
--duration-fast: 200ms;
--duration-base: 400ms;
--duration-slide: 800ms;
--duration-slide-slow: 1200ms;
--duration-reveal: 600ms;

--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
--ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1);
--ease-in-out-quint: cubic-bezier(0.83, 0, 0.17, 1);
--ease-slide: cubic-bezier(0.7, 0, 0.3, 1);           /* codrops */
--ease-reveal: cubic-bezier(0.26, 0.86, 0.44, 0.985); /* reveal.js */
--ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);

--stagger-dense: 10ms;
--stagger-base: 80ms;
--stagger-sparse: 140ms;
```

**Slide transitions are slower than UI.** reveal.js defaults to 800ms with
`cubic-bezier(0.26, 0.86, 0.44, 0.985)` (400ms fast, 1200ms slow). Codrops uses
`cubic-bezier(0.7,0,0.3,1)` at 1200ms out / 700ms in with 80ms stagger.

## Data visualization

```css
--chart-1: #c96442;  --chart-2: #4a7c59;  --chart-3: #3d5a80;
--chart-4: #bc6c25;  --chart-5: #7d5ba6;  --chart-6: #6c757d;
--chart-grid: #e0ddd2;
```

**Categorical chart colors are luminance-matched and separate from the accent
ramp.** Never red+green for a binary contrast — use blue+orange. The Economist's
ordered series is `#006ba2`, `#3ebcd2`, `#379a8b`, `#ebb434` — blue, cyan, teal,
amber; no red/green pair.

## Mapping to other renderers

### PptxGenJS (JS constants)

```js
// No '#' prefix on hex — it silently corrupts the file.
const C = {
  bg: 'FAF9F5', surface: 'F0EEE6', border: 'E0DDD2',
  text: '141413', text2: '55524B', muted: '837E74',
  accent: 'C96442', onAccent: 'FFFFFF',
};
const FONT = { display: 'Georgia', body: 'Arial', mono: 'Consolas' };

// Factory functions — never reuse option objects, the library mutates them.
const titleOpts = () => ({ x: 0.6, y: 0.4, w: 8.8, fontSize: 22, bold: true,
                           color: C.text, fontFace: FONT.display });
```

### python-pptx

```python
from pptx.util import Inches, Pt, Emu
from pptx.dml.color import RGBColor

BG      = RGBColor(0xFA, 0xF9, 0xF5)
ACCENT  = RGBColor(0xC9, 0x64, 0x42)
TEXT    = RGBColor(0x14, 0x14, 0x13)

# NOTE: python-pptx has NO theme-authoring API. The default template is a 4:3
# "White" Office file. You either bring a designed .pptx or you fill one.
```

### Beamer

```latex
\definecolor{bg}{HTML}{FAF9F5}
\definecolor{surface}{HTML}{F0EEE6}
\definecolor{accent}{HTML}{C96442}
\definecolor{text}{HTML}{141413}
```
