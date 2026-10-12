# Design Rules & the Anti-Slop Checklist

71 numbered rules with values, synthesized across the 100 source repos and
ordered by how often they appear and how specific the source values are.

## Color

1. **Three colors maximum: one primary, one accent, one for emphasis/alerts.**
2. **One accent, used exactly once per slide.** "reserve blue `#2F5BFF` for a
   single emphasis per slide" (Runway); "appears once per slide to mark the
   takeaway" (McKinsey); "lands on one element per slide" (TED); "Max 2 accent
   cards per slide — more dilutes emphasis" (powerpoint-skill rule 5).
3. **Never red+green for a binary contrast; use blue+orange.** WCAG AA ≥ 4.5:1
   for text.
4. **Color carries meaning, not decoration.** Bauhaus: red square = emphasis,
   blue circle = data, yellow triangle = next step, "never put a color on the
   wrong shape." powerpoint-skill: positive `#0173B2`, negative `#DE8F05`,
   emphasis `#029E73`, neutral `#8C8C8C`.
5. **Adaptive accent ramp beats named grays.** See design-tokens.md.
6. **Dark slides are rationed.** Dark only on title and section dividers.
   "Sandwich" = dark title + dark conclusion + light content.
7. **No gradients except one deliberate, load-bearing one.** Stripe's mesh band
   (violet `#7038ff`, magenta `#ef008f`, sky `#6ec3f4`, amber `#ffba27`, skewed
   -8°, used only on cover and closing) is the sanctioned case.
8. **Ban list of specific colors:** `#6366f1` (generic indigo), purple-on-white
   gradients, pastel rainbow, "evenly-distributed safe palettes."
9. **Atmosphere over flat fill:** two radial gradients at 10–15% opacity, or a
   50×50px grid at `rgba(255,255,255,0.03)`.
10. **Semantic status colors are functional only** and few: Linear's In Progress
    `#f2c94c`, On track `#4cb782`, Off track `#eb5757`, Urgent `#f2994a`.

## Typography

11. **Two font families maximum**; academic decks use one. Banned as *display*:
    Inter, Roboto, Arial, system fonts — and don't converge on Space Grotesk.
12. **A fixed type scale with values.** Never below 10pt in PPTX.
13. **Weight hierarchy, not family hierarchy.** h1 800 → h2 700 → h3 600 → h4 500.
14. **Display leading 0.8–1. Body 1.4–1.7.** 65-character measure.
15. **Fluid type via `clamp()` or `vw`.**
16. **Negative tracking on display, positive on small caps.** `-0.02em` to
    `-0.035em`; small mono labels `0.18em` uppercase.
17. **Never center-align body paragraphs.**
18. **Mono has exactly two jobs:** code, and small uppercase letterspaced
    metadata.
19. **Serif + sans in one headline is legitimate.**
20. **Telegraphic copy.** Headlines ≤6–8 words; action titles ≤15 words and ≤2
    lines.
21. **Bold and italic sparingly** — bold for key terms on first introduction and
    the focal finding; italic for statistical notation, species names, titles.
22. **`text-wrap: balance` on headings.**

## Layout, spacing, grid

23. **0.5" minimum margin on all four sides** on a 10"×5.625" canvas.
24. **Max 60% of the slide area contains content.** TED: "keep 40 percent of
    every slide empty."
25. **Vertical fill floor: content below the title bar spans ≥75% of the content
    height.** If everything ends in the upper half, something is wrong.
26. **Vertical gap discipline: ≥0.1"** between elements. Compute `y` from the
    preceding element's known bottom, never approximate.
27. **Alignment tolerance ≤0.05".** Same-type elements share identical
    x-coordinates.
28. **Every element satisfies `y + h ≤ SH - M`.** Verify the bottom edge before
    writing the element.
29. **`shrinkText: true` → `<a:normAutofit/>`; never `autoFit: true`** →
    `<a:spAutoFit/>`, which *grows* the shape.
30. **A spacing scale as tokens, not ad-hoc values.** Geist: 6/12/24/48px.
31. **Asymmetric over centered.** MultiLayout layout-4: four frames at
    top/left 15%/15%, 40%/5%, 0%/40%, 50%/75% with sizes 30%×50%, 80%×60%,
    40%×60%, 22.5%×42.5%.
32. **Left axis, one idea per slide.**
33. **Evidence left, interpretation right.** Figure width capped at 60% of
    content width in side-by-side; full-width centered when aspect ratio > 1.6.
34. **One radius value, deck-wide.** Academic decks use zero.

## Hierarchy and emphasis

35. **1px hairlines for structure, 3–4px bars for emphasis.**
36. **No accent lines directly under titles** — "a tell-tale of AI-generated
    slides." Exceptions: load-bearing rules only.
37. **One idea per slide. Sequential points become sequential slides; bullets
    never appear.**
38. **≤6 bullets; 3–5 typical; ≤40 words of body text per content slide.**
39. **Max 2 accent cards per slide**; demote the rest to plain text.
40. **Density floors as well as ceilings.** ≤3 short text-only bullets is too
    sparse. Each card fills ≥40% of its interior area.
41. **Content-ratio target:** visual 60–70%, charts/diagrams 10–15%, text 15–20%.
42. **The 10-second test:** if the audience can't get the slide from text alone
    in 10 seconds, it needs a visual.
43. **Oversized type for emphasis** rather than color or boxes. A stat at 60pt,
    or "around 15 percent of slide height."
44. **Do not wrap text in background cards.** Use spacing and typography. When a
    background is unavoidable: `bg-white/5` + `border border-white/10`.
45. **Borders subtle:** `border-l border-muted/20`, never `border-l-2`. No
    purposeless drop shadows. No `rounded-full` gradient badges.
46. **Layout diversity: never the same layout for 3+ consecutive slides.**

## Structure and chrome

47. **Four slide roles, each with its own background:** title (dark), section
    divider (dark), content (light), closing (dark).
48. **Section navigation as a repeating tracker.** Current section bold on a
    `#F0F4F8` band with a 3px accent left border.
49. **A running head / breadcrumb on content slides.** 40px breadcrumb bar.
50. **Footer chrome is the smallest text and never accent-colored.** 8–11px,
    muted, with a hairline above. Source bottom-left, page number bottom-right.
    No date stamp.
51. **A progress indicator** — bottom bar (Beamer, 0.25cm, proportional fill), a
    nav index circle, or a filling rail.
52. **Chart grammar:** no gridlines where direct labels work; no y-axis; no
    legend boxes — label series directly beside the lines in their own color.
53. **One chart per slide, redrawn natively** with one message.
54. **Speaker notes are telegraphic** — 3–5 talking points, never a full script.
55. **End on conclusions, not "Thank You."** The conclusions slide stays up
    during Q&A. Backup slides after an appendix divider, excluded from the count.

## Motion

56. **Default duration 0.4–0.6s; never above 0.8s** unless cinematic.
57. **Stagger 0.08–0.12s** between children.
58. **Match motion to mood:** Professional = 200–300ms subtle; Dramatic =
    0.8–1.2s fades + scale 0.9→1; Playful = spring; Calm = very slow, high
    whitespace, serif; Editorial = staggered text reveals, pull quotes,
    grid-breaking; Techy = neon glow, scramble, grid.
59. **Slide transitions: fade + ≤30px translate. No 3D transforms, rotations, or
    scale bounces.** Exit fast (0.2s), enter slower.
60. **Only `transform` and `opacity`.** Add `will-change: transform`.
61. **Shared elements glide between slides** (FLIP / Magic Move).
62. **Reveal gradually.** Sequential points appear one at a time.

## File and process discipline

63. **No `#` prefix on hex in PptxGenJS.**
64. **Never reuse option objects** in PptxGenJS — use factory functions.
65. **Validate three ways:** the file opens, the geometry is in bounds, and the
    render matches ground truth (SSIM / IoU / color histograms, SHA-256
    fingerprints).
66. **An independent critic, not the author model grading its own homework.**
67. **Never invent a number, name, or word.** Every figure traces to source
    material. Export is *blocked* when structure, text occupancy, number
    provenance, or page bounds fail.
68. **Read the spec before every page.** A `spec_lock.md` re-read per page
    resists context-compression drift.
69. **Hand-author pages; don't batch-generate them.**
70. **Regenerate from a build script, don't hand-edit output.** The script ships
    next to the file, so "warmer palette" or "10 slides" is one sentence.
71. **Record portable taste as a plain file the user owns** — it only ever
    *seeds* a choice; the current request always wins.

---

## The pre-flight checklist

Run before shipping any deck.

**Structure**
- [ ] Action titles on every slide (complete sentences, not topic labels)
- [ ] One idea per slide; no bullet glyphs in a keynote register
- [ ] ≤6 bullets, ≤40 words of body text per content slide
- [ ] Four slide roles with correct backgrounds (title/section dark, content light)
- [ ] Ends on conclusions, not "Thank You"
- [ ] No layout repeated 3+ slides in a row

**Typography**
- [ ] Two families max; display face is not Inter/Roboto/Arial
- [ ] `text-wrap: balance` on headings
- [ ] Negative tracking on display, positive on small caps
- [ ] No centered body paragraphs
- [ ] Nothing below 10pt (PPTX) / 14px (web)
- [ ] 65-character measure respected

**Color**
- [ ] Exactly one accent, used once per slide
- [ ] No accent line under any title
- [ ] No `#6366f1`, no purple-on-white gradients
- [ ] Contrast ≥7:1 for anything the audience must read
- [ ] Hierarchy survives greyscale conversion

**Layout**
- [ ] 0.5" / 48px minimum margin on all four sides
- [ ] ≤60% of slide area has content
- [ ] Vertical fill ≥75% of content height below the title
- [ ] Alignment tolerance ≤0.05"; same-type elements share x-coordinates
- [ ] Every element satisfies `y + h ≤ SH - M`
- [ ] No text wrapped in background cards; no nested backgrounds
- [ ] `shrinkText: true`, never `autoFit: true`

**Charts**
- [ ] No legend where direct labels fit
- [ ] No y-axis where value labels sit on the bars
- [ ] No default library colors (pass `--theme` / re-skin)
- [ ] Key series in the accent and the heaviest stroke

**Integrity**
- [ ] Every number traced to source material
- [ ] No `#` prefix on PptxGenJS hex values
- [ ] No reused option objects
- [ ] Validated three ways (opens / in-bounds / renders correctly)
- [ ] Reviewed by an independent critic pass

## The ghost-deck test

Before designing, write the deck as plain text — one line per slide, action
titles only. If it reads as a coherent argument with no slide needed to explain
it, the argument works. If a slide's line is a topic label, the content isn't
thought through yet. **Fix the argument before touching the design.**
