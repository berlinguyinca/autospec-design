# Baseline design language

Schema-conformant defaults. A vendor `DESIGN.md` that predates a schema section
resolves that section from here rather than leaving the pipeline with nothing to check
against. These are deliberately plain — the baseline exists to be *checkable*, not to
be anyone's brand.

## Overview

A neutral, restrained language: generous whitespace, one accent, and motion used only
to explain what changed. Character: quiet and precise.

## Colors

| Token | Value | Role |
| --- | --- | --- |
| `--color-surface` | `#ffffff` | Page background |
| `--color-surface-raised` | `#f6f7f9` | Cards, wells |
| `--color-border` | `#e3e6ea` | Hairlines |
| `--color-text` | `#12151a` | Body text |
| `--color-text-muted` | `#5b6472` | Secondary text |
| `--color-accent` | `#2f6feb` | Primary action |
| `--color-accent-hover` | `#2559c4` | Primary action, hover |
| `--color-success` | `#1a7f4b` | Semantic |
| `--color-warning` | `#8a5a00` | Semantic |
| `--color-danger` | `#b3261e` | Semantic |

Text on `--color-surface` and on `--color-accent` must meet 4.5:1; large text and
non-text boundaries must meet 3:1.

## Typography

Families are tokens: `--font-sans`, `--font-mono`. The baseline binds neither to a
specific face; a vendor doc naming one overrides this section.

| Step | Size / line-height | Weight |
| --- | --- | --- |
| `display` | 40 / 44 | 600 |
| `title` | 28 / 34 | 600 |
| `heading` | 20 / 28 | 600 |
| `body` | 16 / 24 | 400 |
| `caption` | 13 / 18 | 400 |

## Layout

Spacing scale, 4px base: `4, 8, 12, 16, 24, 32, 48, 64`. Container max-width 1200px
with 24px gutters, 16px below 640px. Whitespace is the primary grouping device.

## Shapes

Border radius: `4` (controls), `8` (cards), `12` (dialogs), `999` (pills).

## Components

- **Buttons** — 40px tall, 16px horizontal padding, radius 4. Primary uses
  `--color-accent`; hover moves to `--color-accent-hover`. Focus-visible draws a 2px
  outline offset 2px, and is never removed without an equally visible replacement.
- **Inputs** — 40px tall, 1px `--color-border`, radius 4. Every input has a persistent
  visible label; placeholder text is never the only label.
- **Cards** — `--color-surface-raised`, 1px border, radius 8, 16px padding.

## Motion

**Duration scale**

| Token | Value | For |
| --- | --- | --- |
| `--motion-instant` | 80ms | State echo on press |
| `--motion-fast` | 150ms | Hover, focus, small state change |
| `--motion-base` | 240ms | Enter and exit |
| `--motion-slow` | 400ms | Layout change across a region |

**Easing**

| Token | Value | For |
| --- | --- | --- |
| `--ease-standard` | `cubic-bezier(0.2, 0, 0.2, 1)` | Most transitions |
| `--ease-enter` | `cubic-bezier(0, 0, 0.2, 1)` | Entrances, decelerating |
| `--ease-exit` | `cubic-bezier(0.4, 0, 1, 1)` | Exits, accelerating |

**Choreography**

- `enter` — fade `0→1` with a 4px upward translate, `--motion-base`, `--ease-enter`.
- `exit` — fade `1→0`, no translate, `--motion-fast`, `--ease-exit`. Exits are faster
  than entrances; a slow exit reads as lag.
- `stagger` — 40ms between siblings, capped at 6 items; beyond that animate the
  container, not the children.
- `layout-shift` — animate the changing axis only, `--motion-slow`, `--ease-standard`.

**Never animates**: body text reflow, focus outlines, error message appearance (it must
be immediate), scroll position on load, and anything driven by user scroll position.

**Reduced-motion fallback, per pattern.** Under `prefers-reduced-motion: reduce`:

| Pattern | Fallback |
| --- | --- |
| `enter` | Opacity only, `--motion-fast`, no translate |
| `exit` | Opacity only, `--motion-instant` |
| `stagger` | No stagger; all items resolve together |
| `layout-shift` | No animation; final state applied directly |

Fallbacks reduce motion rather than removing feedback: an element that simply appears
with no transition can read as a rendering glitch.

## Device & Input

- **Pointer** — under `pointer: coarse`, controls grow to a 44px minimum and hover-only
  reveals become always-visible. No affordance depends on `any-hover: hover`.
- **Hover equivalents** — every hover treatment is mirrored on `:focus-visible`, and any
  hover-revealed action is reachable by tab and by tap.
- **Safe-area** — fixed and sticky elements pad by `env(safe-area-inset-*)`.
- **Orientation** — both orientations are supported; nothing is locked, and rotation
  loses no content or entered data.
- **Asset density** — raster assets ship at 1x and 2x; icons are vector.

## Responsive Behavior

**Breakpoints**: 640, 768, 1024, 1280. **Touch targets**: 44px minimum on coarse
pointers, never below 24×24 anywhere. **Collapsing**: multi-column becomes single column
at 768; tables become stacked rows or gain a scroll container, never a squeezed grid.
Content must reflow at 320 CSS px without two-dimensional scrolling, and remain usable
at 200% zoom.

## Do's and Don'ts

**Do** reference tokens for every color, space, duration, and easing. Do give every
interactive element a visible focus state. Do ship a reduced-motion fallback with every
animation. Do state the reduced-motion behaviour when adding a pattern.

**Don't** use raw hex in components. Don't remove focus outlines. Don't animate for
longer than 400ms. Don't put an action behind hover alone. Don't set `user-scalable=no`
or `maximum-scale=1`. Don't rely on color alone to carry meaning.

## Known Gaps

No dark theme is specified; a vendor doc supplying one overrides this section. No
data-visualization palette. No illustration or photography direction.
