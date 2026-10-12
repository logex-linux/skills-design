# Frameworks & Native PPTX

## The fidelity law

**Every conversion in the source set preserves appearance *or* structure, never
both.** This is the single most important thing to internalize before choosing a
toolchain.

Marp documents it most bluntly:

| Export | Fidelity | Editability |
|---|---|---|
| `--pptx` (default) | Pre-rendered background images | **"contents cannot be modified or re-used"** |
| `--pptx-editable` | **Explicitly degraded** | Editable, but EXPERIMENTAL |

Marp's own warning on `--pptx-editable`: "presenter notes are not supported" and
"lower slide reproducibility compared to the conversion into regular PPTX and
other formats… **We do not recommend to export the editable PPTX if maintaining
the slide's appearance is important.**" It may throw an error or output an
incomplete result when the theme uses complex styles — they name `gaia`
specifically.

Slidev has the same split: `slidev export --format pptx-editable` measures slides
in the browser and rebuilds them as PowerPoint shapes (text selectable, boxes
movable, notes carried), falling back per-slide to image export when a slide
can't be safely rebuilt.

**Read that trade before you promise the user an editable deck.**

## The three honest routes to a beautiful native PPTX

1. **Bring a designed template and fill it.** docxtemplater, python-docx opening
   a designed file, OfficeCLI `merge`, ppt-master's template route. Best fidelity,
   least layout freedom.
2. **Generate native objects with enough layout intelligence to not look
   generated.** python-pptx / PptxGenJS *with real design rules*. Route 2 without
   design rules is exactly what produces "AI slop" decks.
3. **Compile from a higher-fidelity intermediate.** ppt-master's SVG pipeline.

## Framework comparison

| Framework | Language | Output | Editable | Best for |
|---|---|---|---|---|
| **reveal.js** (72.4k) | HTML/JS | HTML | n/a | talks with code, fragments, speaker view |
| **Slidev** (49k) | Vue/Markdown | HTML, PDF, PPTX | partial | developer decks, best theme system |
| **Marp** (12.6k) | Markdown | HTML, PPTX, PDF, PNG | partial | fast Markdown decks |
| **impress.js** (38.2k) | HTML | HTML | n/a | spatial/non-linear arguments |
| **Shower** (4.9k) | HTML | HTML, PDF | n/a | clean, accessible HTML decks |
| **WebSlides** (6.3k) | HTML | HTML | n/a | landing-page-style decks |
| **presenterm** (8.9k) | Markdown/YAML | terminal | n/a | terminal presentations |
| **python-pptx** (3.6k) | Python | PPTX | **full** | programmatic native PPTX |
| **PptxGenJS** (6.3k) | JS | PPTX | **full** | programmatic native PPTX in JS |
| **PPTist** (9.4k) | Vue | PPTX (~95%+) | **full** | browser-based editing |
| **docxtemplater** (3.6k) | JS | PPTX | **full** | template filling |
| **ppt-master** (60k) | Python/CLI | PPTX | **full** | AI decks with native output |
| **Beamer** | LaTeX | PDF | LaTeX | academic, math-first |

## python-pptx

**The critical constraint: python-pptx has no theme-authoring API at all.** The
default template is a 4:3 "White" Office file. You either bring a designed
`.pptx` or you fill one — there is no third option.

```python
from pptx import Presentation
from pptx.util import Inches, Pt
from pptx.dml.color import RGBColor

prs = Presentation()
prs.slide_width  = Inches(13.333)
prs.slide_height = Inches(7.5)

slide = prs.slides.add_slide(prs.slide_layouts[6])   # 6 = blank
tb = slide.shapes.add_textbox(Inches(0.6), Inches(0.4), Inches(12), Inches(1))
tf = tb.text_frame
tf.word_wrap = True
p = tf.paragraphs[0]
run = p.add_run()
run.text = "Intervention reduced costs by 23% (p < 0.01)"
run.font.size = Pt(22)
run.font.bold = True
run.font.name = "Georgia"
run.font.color.rgb = RGBColor(0x14, 0x14, 0x13)

prs.save('deck.pptx')
```

**To get a designed deck:** open a designed `.pptx` as the template, or build one
in PowerPoint/Keynote and fill it. `prs = Presentation('designed.pptx')`.

## PptxGenJS

```js
import pptxgen from "pptxgenjs";

const C = { bg: 'FAF9F5', text: '141413', accent: 'C96442' };

// Factory functions — the library MUTATES option objects in place.
const titleOpts = () => ({
  x: 0.6, y: 0.4, w: 12.1, h: 0.9,
  fontSize: 22, bold: true, fontFace: 'Georgia',
  color: C.text,
});

const pres = new pptxgen();
pres.defineLayout({ name: 'W16x9', width: 13.333, height: 7.5 });
pres.layout = 'W16x9';

const slide = pres.addSlide();
slide.background = { color: C.bg };
slide.addText("Intervention reduced costs by 23% (p < 0.01)", titleOpts());

await pres.writeFile({ fileName: "deck.pptx" });
```

**The three PptxGenJS traps:**
1. **No `#` prefix on hex** — it silently corrupts the file.
2. **Never reuse option objects** — they're mutated in place (shadows to EMU).
3. **`shrinkText`-equivalent behaviour differs from PowerPoint** — verify
   overflow by rendering, not by trusting the API.

## docxtemplater (template filling)

The highest-fidelity route when a template exists. Core is free; **images, HTML,
charts, and xlsx embedding are paid modules.**

```js
import Docxtemplater from "docxtemplater";
import PizZip from "pizzip";

const zip = new PizZip(templateBuffer);
const doc = new Docxtemplater(zip, {
  paragraphLoop: true,
  linebreaks: true,
});

doc.render({
  title: "Intervention reduced costs by 23%",
  slides: [
    { heading: "Method", body: "..." },
    { heading: "Result", body: "..." },
  ],
});

const out = doc.getZip().generate({ type: "nodebuffer" });
```

Template syntax in the PPTX: `{title}`, `{#slides}...{/slides}`, `{?cond}...{/cond}`.

## ppt-master — the AI route with native output

60k stars. "AI generates native PowerPoint from any document."

**The architecture is a routed workflow, not a script.** `SKILL.md` owns global
discipline and route selection; each route owns its procedure:

| Route | Authority |
|---|---|
| Image to PPTX | `workflows/profiles/image-to-pptx.md` |
| Beautify | `workflows/profiles/beautify-pptx.md` |
| Ordinary | `workflows/generate-pptx.md` |
| Create Template | `workflows/create-template.md` |
| Edit Native PPTX | `workflows/edit-native-pptx.md` |

**The distinctive decision: SVG as the page-design source.** `svg_output/` is the
complete page-design source; pages are authored as SVG, validated by
`svg_quality_checker.py` (real thresholds: 1px overlap, 5% text overflow), then
compiled to native PPTX by `svg_to_pptx.py`.

**The export is structure-explicit:** it compiles only the route's explicit
structure contract (`flat` Slide-local; `structured` Master/Layout/Slide parts)
and **never infers structure**.

**Its documented fidelity taxonomy is the honesty the rest of the cluster lacks:**

| Tier | Meaning |
|---|---|
| `Native-stable` | uses the corresponding editable DrawingML property within documented limits |
| `Native-normalized` | still editable, normalized into an equivalent structure |
| `Approximate` | visually close, structurally different |
| `Bake-required` | must be rasterized |
| `Sidecar-package` | shipped as a companion file |
| `Direct preservation` | passed through untouched |
| `Unsupported` | not handled |

SmartArt is a deliberate omission. Formulas compile to editable OMML. Charts and
tables ship as editable shapes by default; `--native-charts-and-tables` produces
data-backed Chart/Table objects with **Edit Data**.

**Why it produces better decks than prompt-to-slides: the quality comes from the
workflow** — argument reasoning → template selection → staged confirmation → SVG
design → validation gates → native export. Not from a clever prompt.

## OfficeCLI — the agent-facing route

31.8k stars. The most complete agent-facing office tool.

- **`merge`** — zero-code `{{key}}` → JSON templating
- **`dump` / `batch`** — round-trip gives a learn-from-example loop
- **Built-in renderer** — **the only tool in the set that lets an agent *see* its
  own slides without a display**, which is what makes it usable in CI/Docker. An
  agent that can see overflow and overlap fixes it.

**Two gotchas:**
1. **Resident mode defers disk writes** — you must `save` before any
   non-officecli reader sees changes.
2. **The .NET binary is a supply-chain dependency** — verify it.

## reveal.js

```js
Reveal.initialize({
  width: 1920, height: 1080, margin: 0.04,
  transition: 'fade',             // none/fade/slide/convex/concave/zoom
  transitionSpeed: 'default',     // default/fast/slow → 800/400/1200ms
  backgroundTransition: 'fade',
  controls: true, progress: true, slideNumber: 'c/t',
  hash: true, center: false,      // center: false — asymmetric layouts need it
  fragments: true,
});
```

**`center: false` matters** — the default centers content vertically, which is
the most-banned layout.

**The speaker view (S key)** shows current slide, next slide, elapsed time,
speaker notes, and a presentation URL for audience sync.

**Themes** are SCSS files; `black.scss` is the "super-theme" pattern:

```scss
@use 'template/settings' with (
  $background-color: #1a1917,
  $heading-text-transform: none,
);
```

**Type-scale note:** the default `h1` is 3.77em of a 40px base (~151px); the
`black` theme drops it to ~105px. Check what your theme actually resolves to.

## Slidev

Best theme system in the set. Theme package anatomy:

```
styles/index.ts     → injects --slidev-theme-* tokens
layouts/            → cover, center, two-cols, image, quote, section, fact, ...
components/         → reusable Vue components
setup/              → theme setup code
```

Frontmatter:

```yaml
---
theme: default
fonts:
  sans: Inter
  serif: Gelasio
  mono: JetBrains Mono
colors:
  primary: '#c96442'
aspectRatio: 16/9
canvasWidth: 1920
---
```

**The `@slidev/theme-apple-basic` lessons** (worth copying regardless of
framework): negative-margin optical alignment, weight inversely proportional to
size, the kicker/eyebrow pattern, and `font-weight: 700` on display sizes.

## Marp

```markdown
---
marp: true
theme: gaia
paginate: true
size: 16:9
---

<!-- _class: lead -->

# Action title as a complete sentence
```

Custom themes use the `/* @theme name */` directive with `section` selectors and
the `light-dark()` role pattern.

**Export fidelity:** HTML perfect; `--pptx` is images; `--pptx-editable` is
experimental and needs LibreOffice. PDF via Chrome.

## Beamer (academic)

The print-first tradition. No PPTX round-trip preserves TikZ. Use it when the
deliverable is a PDF and the content is math-heavy.

**The academic conventions worth stealing even outside Beamer:**
- Visual restraint — no decorative anything
- A progress indicator (0.25cm bar, proportional fill)
- `\backupbegin` / `\backupend` to exclude appendix slides from the page count
- Semantic color aliases (`\definecolor{emphasis}{HTML}{...}`) rather than raw hex
  at every use site

## The decision

```
Does the user need an editable .pptx?
├─ YES, and a template exists      → fill it (docxtemplater / OfficeCLI merge)
├─ YES, no template                → python-pptx or PptxGenJS WITH design rules
├─ YES, AI-generated, high quality → ppt-master
├─ NO — HTML is fine               → Slidev (best themes) or reveal.js (best talks)
├─ NO — Markdown is the source     → Marp
├─ NO — the argument is spatial    → impress.js
└─ PDF + math                      → Beamer
```

**Never promise editable PPTX from an HTML pipeline without checking the
fidelity table above.**
