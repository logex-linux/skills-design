# Motion for Slides

Slide motion has different rules from UI motion. UI is fast because it must feel
attached to the cursor; slides are slower because the audience is reading.

## The timing values

| Purpose | Duration | Easing |
|---|---|---|
| Reveal (fragment/appear) | 600ms | `cubic-bezier(0.26, 0.86, 0.44, 0.985)` |
| Slide transition | **800ms** (400 fast / 1200 slow) | `cubic-bezier(0.26, 0.86, 0.44, 0.985)` |
| Codrops out | 1200ms | `cubic-bezier(0.7, 0, 0.3, 1)` |
| Codrops in | 700ms | `cubic-bezier(0.7, 0, 0.3, 1)` |
| Stagger between children | **80–120ms** (10ms dense, 140ms sparse) | — |
| Exit | 200ms (fast) | — |
| Enter | slower than exit | — |

**Default 0.4–0.6s; never above 0.8s** unless cinematic. Slides are the one
place 800–1200ms is correct.

## The transition families

| Family | When |
|---|---|
| **Fade + ≤30px translate** | the default. Correct almost always. |
| Slide (horizontal) | moving forward through a sequence |
| Zoom | entering a detail from an overview |
| Convex / concave | reveal.js's 3D options — use rarely |
| Page flip | playful registers only |

**Banned in slide transitions:** 3D rotations, scale bounces, and anything that
moves more than ~30px. The audience is reading; don't make them track a moving
target.

```css
/* The default slide transition */
.slide {
  transition: opacity 800ms cubic-bezier(0.26, 0.86, 0.44, 0.985),
              transform 800ms cubic-bezier(0.26, 0.86, 0.44, 0.985);
}
.slide:not(.active) { opacity: 0; transform: translateY(30px); }
.slide.active { opacity: 1; transform: none; }
```

## Mood-matched motion

| Mood | Motion |
|---|---|
| **Professional** | 200–300ms, subtle. Fade + small translate only. |
| **Dramatic** | 0.8–1.2s fades + scale 0.9→1 |
| **Playful** | spring, slight overshoot |
| **Calm** | very slow, high whitespace, serif |
| **Editorial** | staggered text reveals, pull quotes, grid-breaking |
| **Techy** | neon glow, scramble text, grid |

## Reveal patterns

### Staggered children

```css
.reveal > * {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 600ms var(--ease-out-expo),
              transform 600ms var(--ease-out-expo);
}
.reveal.is-visible > * { opacity: 1; transform: none; }
.reveal.is-visible > *:nth-child(1) { transition-delay: 0ms; }
.reveal.is-visible > *:nth-child(2) { transition-delay: 80ms; }
.reveal.is-visible > *:nth-child(3) { transition-delay: 160ms; }
.reveal.is-visible > *:nth-child(4) { transition-delay: 240ms; }
/* total stagger budget: ≤400ms across the group */
```

### Line-by-line reveal

```css
.reveal-line { overflow: hidden; }
.reveal-line span {
  display: block;
  transform: translateY(100%);
  transition: transform 600ms var(--ease-out-expo);
}
.reveal-line.is-visible span { transform: none; }
```

### Word-by-word (for a key sentence only)

```js
const el = document.querySelector('.reveal-words');
el.innerHTML = el.textContent.split(' ')
  .map((w, i) => `<span style="transition-delay:${i * 60}ms">${w}</span>`).join(' ');
```

## Shared-element transitions (FLIP / Magic Move)

The highest-value motion pattern for decks. Shared elements *glide* between
slides instead of hard-cutting — overview→detail, agenda→section header, grid
card→focused detail, diagram nodes staying connected while the explanation
changes.

```js
// FLIP: First, Last, Invert, Play
function flip(el, mutate) {
  const first = el.getBoundingClientRect();
  mutate();
  const last = el.getBoundingClientRect();
  const dx = first.left - last.left;
  const dy = first.top - last.top;
  const sx = first.width / last.width;
  const sy = first.height / last.height;

  el.animate([
    { transform: `translate(${dx}px, ${dy}px) scale(${sx}, ${sy})` },
    { transform: 'none' }
  ], { duration: 800, easing: 'cubic-bezier(0.26, 0.86, 0.44, 0.985)' });
}
```

## Apple's motion principles, applied to slides

From `apple-design` — these are about *feel*, and they transfer.

**Interruptibility is the single most important principle.** Every animation must
be interruptible and redirectable. A user who grabs a moving element mid-flight
must be able to reverse it without waiting for the animation to finish. On the
web: springs animate from the current value by default; CSS transitions and
`@keyframes` cannot be grabbed and reversed.

**Animate from the presentation value, never the target.** On interrupt, read the
element's live on-screen transform and start from there. Starting from the
logical value causes a visible jump.

**Blend velocity on reversal — don't hard-cut it.** Replacing one animation with
another at a reversal creates a velocity discontinuity, a "brick wall."

**Damping ratio and response, not mass/stiffness/damping:**

| Interaction | Damping | Response |
|---|---|---|
| Move / reposition | `1.0` (critically damped) | `0.4` |
| Rotation | `0.8` | `0.4` |
| Drawer / sheet | `0.8` | `0.3` |

**Start at damping 1.0.** Add bounce (damping ~0.8) *only when the gesture itself
carried momentum* — a flick, a throw, a drag release. Overshoot on a menu that
just faded in feels wrong; overshoot on a card you flicked feels right.

```js
import { animate } from 'motion';

// Critically damped default (no overshoot) — correct for slide reveals
animate(el, { y: 0, opacity: 1 }, { type: 'spring', bounce: 0, duration: 0.4 });
```

**Velocity handoff.** When a gesture ends, the animation must continue at the
finger's exact velocity so there's no visible seam:

```
relativeVelocity = gestureVelocity / (targetValue − currentValue)
```

**Momentum projection.** Don't snap to the nearest boundary from the release
point — project the resting position first:

```js
function project(initialVelocity, decelerationRate = 0.998) {
  return (initialVelocity / 1000) * decelerationRate / (1 - decelerationRate);
}
const projectedEndpoint = currentPosition + project(releaseVelocity);
const target = nearestSnapPoint(projectedEndpoint);
```

**Spatial consistency.** "If something disappears one way, we expect it to emerge
from where it came." Enter and exit along the same path. Anchor interactions to
their source — set `transform-origin` to the trigger.

**Rubber-banding, not hard stops.** At an edge, resist progressively:

```js
function rubberband(overshoot, dimension, constant = 0.55) {
  return (overshoot * dimension * constant) / (dimension + constant * Math.abs(overshoot));
}
```

## Performance

**Only `transform` and `opacity`.** Never animate `top`, `left`, `width`,
`height`, `margin`, or `font-size`.

```css
.will-animate {
  will-change: transform, opacity;   /* apply immediately before, remove after */
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
}
```

**`will-change` is not free** — each promotion costs GPU memory. Apply it
immediately before the animation and remove it after, not as a static style.

**Keep per-frame positional change below the perception threshold** to avoid
strobing. For very fast motion, a subtle motion blur or stretch encodes speed and
reads better than a hard sharp streak.

## Reduced motion

Reduced motion doesn't mean *no* feedback — it means a gentler, non-vestibular
equivalent.

```css
@media (prefers-reduced-motion: reduce) {
  .slide,
  .reveal > *,
  .reveal-line span {
    transition: opacity 200ms ease !important;
    transform: none !important;
    transition-delay: 0ms !important;
  }
}
```

Drop elastic/overshoot entirely. Keep opacity and color changes that aid
comprehension. Also avoid full-viewport moving backgrounds, slow looping
oscillations (near 0.2 Hz / one cycle per 5s), and abrupt brightness jumps.

## The rules

1. **Slide transitions are slower than UI** — 800ms, not 200ms.
2. **Fade + ≤30px translate is the default.** No 3D, no rotation, no bounce.
3. **Exit fast, enter slower.**
4. **Stagger 80–120ms, total budget ≤400ms** across a group.
5. **Shared elements glide** (FLIP), never hard-cut.
6. **Only `transform` and `opacity`.**
7. **Start at damping 1.0**; bounce only when momentum preceded it.
8. **Always provide the reduced-motion path.**
