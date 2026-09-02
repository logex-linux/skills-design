---
name: web-style-pptx
description: "Create, redesign, or edit polished, editable landscape or portrait PowerPoint presentations with a web-design-like workflow: structured content, reusable visual components, design tokens, SVG-quality graphics, programmatic layout, rendering, and visual QA. Use when the user asks to make a PPT, PPTX, slide deck, presentation, pitch deck, proposal, report, keynote-style deck, vertical or mobile-first deck, or to turn documents, research, data, an outline, a webpage, or a visual direction into refined slides. Also use for requests such as “像做 HTML 一样做 PPT”, “做一套精美演示文稿”, “生成可编辑 PPT”, “横屏/竖屏 PPT”, “SVG 风格 PPT”, or “用 Work 创建 PPT”. Produce a real .pptx unless the user explicitly requests another format."
---

# Web-style PPTX

Create presentations as designed systems rather than decorated documents. Treat each slide as a fixed-size, orientation-aware canvas, define a small visual language, compose reusable primitives, render the result, inspect it like a webpage, and iterate until it is presentation-ready.

## Non-negotiable outcome

Deliver a real, editable `.pptx` that:

- communicates one clear idea per slide;
- has a coherent narrative and consistent visual system;
- uses editable PowerPoint text, shapes, tables, and charts wherever practical;
- uses SVG or raster images only when they materially improve visual quality;
- contains source notes for researched claims and externally sourced assets;
- has been rendered and visually inspected slide by slide;
- uses the requested or inferred landscape/portrait canvas consistently;
- contains no unintended overlap, clipping, overflow, broken characters, or unresolved placeholders.

Do not satisfy a PPT request with only HTML, screenshots, a PDF, image files, or a plan.

## Choose the production route

Use this order of precedence:

1. **Existing PPTX or template supplied**: preserve its masters, layouts, typography, geometry, canvas size, orientation, and brand language unless the user explicitly asks to change them. Edit inherited elements rather than recreating the deck from scratch. An orientation change requires deliberate reflow, not merely swapping slide width and height.
2. **Explicit visual direction supplied**: build a custom system from that direction.
3. **No visual direction supplied**: infer a restrained system from the topic and audience. Prefer editorial composition and strong typography over generic dashboard cards.

Use the installed presentation-creation runtime and its required workflow when available. Read its complete presentation instructions and required style guidance before implementation. Load the bundled workspace dependencies before creating files.

If the presentation runtime is unavailable, use PptxGenJS as the cross-platform fallback. Generate native PowerPoint text and shapes, embed sanitized inline SVG for complex vector visuals, and preserve the same render-and-review loop. Do not switch to `python-pptx` merely because it is familiar.

## Workflow

Follow the steps in order. Make reasonable assumptions for low-risk details; ask only when audience, purpose, language, page count, or brand constraints would materially change the result and cannot be inferred.

### 1. Establish the communication job

Extract or infer:

- audience and decision to influence;
- presentation context: live talk, read-ahead, proposal, report, pitch, teaching, or update;
- language, orientation, slide size or aspect ratio, approximate length, and delivery format;
- source materials, factual cutoff date, brand rules, and visual references;
- desired editability: normal, vector-heavy, or maximum native-shape editing.

Write a one-sentence deck thesis. Every slide must advance it.

### 2. Choose and lock the slide canvas

Decide the slide orientation and dimensions before mapping or composing slides. Use this precedence:

1. preserve the canvas of an existing PPTX or supplied template;
2. follow explicit orientation, aspect-ratio, or physical-size requirements;
3. otherwise infer the canvas from the viewing context.

Use these defaults when no template overrides them:

- **Landscape 16:9 — 13.333 × 7.5 in**: projected talks, desktop presentations, pitches, and ordinary slide decks. This is the general default.
- **Portrait 9:16 — 7.5 × 13.333 in**: phone-first viewing, vertical screens, social distribution, and requests that simply say “portrait/vertical PPT” without a print context.
- **Portrait A4 — 8.267 × 11.693 in**: printable reports, proposals, handouts, and document-like read-aheads in A-series regions.
- **Portrait Letter — 8.5 × 11 in**: printable document-like decks where US Letter is explicitly requested or clearly expected.
- **Landscape 4:3 — 10 × 7.5 in**: use only for a matching template, legacy display, or explicit request.

Do not mix canvas sizes or orientations within one deck. Record the chosen width, height, aspect ratio, safe margins, and intended viewing context in the design tokens. If a portrait request could reasonably mean either phone-first 9:16 or printable A4/Letter and the context cannot resolve it, ask before layout because that choice changes content density and composition.

### 3. Build the story before styling

Create a concise slide map with:

- slide purpose;
- one-sentence takeaway or action title;
- supporting evidence;
- intended visual form;
- source or provenance;
- transition to the next slide.

Use a suitable narrative pattern rather than forcing one formula:

- problem → insight → solution → proof → action;
- situation → complication → question → answer;
- context → options → recommendation → roadmap;
- claim → evidence → implication.

Remove slides that repeat a point. Split slides with two independent messages. Keep the title slide minimal.

### 4. Define a design system

Set explicit tokens before composing slides:

- canvas width, height, orientation, and safe margins;
- background, text, muted, accent, positive, warning, and negative colors;
- title, subtitle, body, caption, and numeric typography;
- spacing scale, corner radius, stroke weights, and image treatment;
- chart palette and data-emphasis rule.

Limit the palette to one dominant neutral family, one accent, and semantic colors. Use color to encode meaning, not to decorate empty space.

Use the following minimum sizes when no template overrides them:

- deck title: 50 pt;
- slide title: 35 pt;
- subheading or callout: 24 pt;
- body: 16 pt;
- source note: 9–11 pt.

For Chinese, prefer a locally available CJK sans serif such as PingFang SC, Microsoft YaHei, MiSans, or Noto Sans CJK SC. For English, use a locally available, presentation-safe family. Never assume a font is installed; verify or use a safe fallback.

Make margins proportional to the selected canvas, generally about 5–7% of its width and height, then tune for the viewing context. Keep type readable at the final viewing scale; a tall canvas is not permission to shrink a landscape slide into portrait dimensions.

### 5. Compose like a webpage, not like a dashboard

Treat the deck as a component system, but keep each slide visually distinct.

Create reusable primitives for:

- slide frame, header, footer, page number, and source note;
- text styles and automatic text fitting;
- image crop and mask;
- metric, quote, comparison, timeline, process, and chart modules;
- section divider and closing slide.

Use primitives to enforce alignment and consistency, not to fill every slide with repeated cards, pills, tabs, or faux interface elements. Prefer one dominant composition per slide. Vary adjacent slide silhouettes while retaining the same visual language.

Use a clear grid. Keep equal outer margins unless the concept intentionally breaks the grid. Align objects to shared edges and baselines. Avoid decorative elements that do not support hierarchy or meaning.

For portrait decks, design a genuinely vertical reading path:

- favor stacked sections, full-width visuals, and one or two columns at most;
- place the main takeaway in the upper portion and let evidence progress downward;
- turn wide horizontal processes into vertical timelines, stepped flows, or multiple slides;
- reformat wide tables and charts rather than squeezing or rotating them;
- use fewer, larger elements so the slide remains legible on a phone or printed page.

For landscape decks, use the horizontal span intentionally for comparisons, sequences, and side-by-side evidence. In either orientation, do not mechanically transpose an existing composition; reflow hierarchy, text measure, visual crops, and component geometry for the target canvas.

### 6. Choose native objects versus visual assets

Prefer native PowerPoint objects for:

- all important text;
- simple geometry and connectors;
- tables that users may update;
- straightforward charts;
- simple diagrams that need later editing.

Use SVG for:

- icons and logos with known licensing;
- complex but static vector illustrations;
- maps, ornamental linework, and controlled data graphics;
- visual elements whose appearance matters more than element-level editing.

Use high-resolution raster images for photography, texture, or generated illustration. Crop deliberately for the allocated frame; never stretch.

For maximum editability, constrain SVG to simple paths, rectangles, circles, lines, solid fills, and basic strokes before converting to native shapes. Avoid filters, scripts, external CSS, external references, and unsupported gradients. Treat unknown SVG/XML as untrusted input and sanitize it.

Do not flatten an entire slide into one image. Keep titles, body copy, citations, and recurring elements editable.

### 7. Handle data and claims rigorously

Use a chart only when it answers a question more clearly than prose.

- line: change over time;
- bar: comparison or ranking;
- stacked bar/area: composition over time;
- scatter: relationship or distribution;
- table: exact values or mixed attributes;
- process/timeline: ordered stages, not quantitative magnitude.

State the conclusion in the slide title or adjacent callout. Highlight only the data that proves it. Label units, periods, definitions, and cutoff dates. Do not invent missing values or convert estimates into facts.

Put source URLs and asset provenance in speaker notes using the presentation runtime’s required `[Sources]` format. For user-provided materials, note the filename or supplied source. For model-generated visuals, identify them as generated.

For portrait slides, prefer charts with short category labels, direct labels, and a strong vertical or full-width reading order. Split dense comparisons across slides or convert wide tables into grouped records. Preserve scale integrity when changing chart orientation; never distort the visual simply to fill a tall canvas.

### 8. Implement on a fixed canvas

Build in a temporary working directory and place only final deliverables in the requested output location. Use absolute paths.

Keep content, design tokens, reusable components, and slide assembly logically separate in the implementation, similar to:

```text
content/spec → theme/tokens → components → slide compositions → PPTX → renders
```

This is a conceptual separation, not a requirement to create unnecessary files or abstractions. For a short one-off deck, one clear implementation file is enough.

Use deterministic coordinates and sizes. Do not rely on browser CSS, DOM measurement, or unsupported HTML-to-PPT conversion for the final geometry. If HTML is used as a visual prototype, rebuild the final slide with PowerPoint-native objects or supported assets.

Define the slide layout once, before creating slides, and derive all layout calculations from named canvas tokens such as `SLIDE_W`, `SLIDE_H`, `MARGIN_X`, and `MARGIN_Y`. Never scatter hard-coded 16:9 dimensions through components. When using PptxGenJS with a custom portrait canvas, define and activate the layout explicitly, for example:

```js
pptx.defineLayout({ name: "PORTRAIT_9_16", width: 7.5, height: 13.333 });
pptx.layout = "PORTRAIT_9_16";
```

Use the equivalent layout API in another presentation runtime. Confirm that inherited masters and reusable components use the same canvas. If converting an existing deck between orientations, rebuild or reflow each slide against the new tokens and inspect every slide; do not rely on automatic scaling.

Create connectors before diagram nodes so edges remain behind shapes. Use helper functions to prevent repeated layout arithmetic. Include overlap checks where supported, but treat warnings as prompts for visual inspection rather than automatically ignoring them.

### 9. Render, inspect, and iterate

Before delivery:

1. render every slide to an image;
2. create a montage that preserves the target aspect ratio to assess rhythm and consistency;
3. inspect every slide individually at full size;
4. run the available overflow or slide-boundary test against the actual canvas width and height;
5. fix all unintended overlap, clipping, wrapping, misalignment, weak image crops, illegible labels, and inconsistent styling;
6. re-render every changed slide and re-check the full deck;
7. open or inspect the final PPTX structure when possible to confirm the file is valid, the slide size and orientation are correct, and objects remain editable.

Never deliver from code inspection alone. A successful export is not evidence of a successful design.

## Visual quality rules

- Lead with a conclusion, not a topic label.
- Keep text concise; change the layout or shorten copy before reducing font size.
- Do not allow a one-line title box to wrap.
- Use generous but purposeful negative space.
- Prefer a few strong visuals over many tiny decorations.
- Avoid stock-template clichés: excessive gradients, glowing blobs, generic icon rows, random rounded cards, and ornamental charts.
- Do not reuse the same image on multiple slides unless it is a deliberate background motif.
- Avoid dense diagrams unless the relationships cannot be explained more clearly another way.
- Keep all audience-facing text free of internal planning language.
- Use consistent punctuation, numeric formats, capitalization, and terminology.

## Delivery

Return the final `.pptx` and a short summary of the deck’s narrative, visual direction, and canvas format, such as “landscape 16:9,” “portrait 9:16,” or “portrait A4.” Mention whether sources were used and whether the file was visually verified. Do not include temporary code, render folders, planning notes, or intermediate specs unless the user asks.

If PDF or preview images are also requested, export them in addition to—not instead of—the editable `.pptx`.
