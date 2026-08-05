# DESIGN.md schema

A `DESIGN.md` describes one design language well enough that an implementer can build
to it and a gate can check the result. Sections are matched by their `##` heading text.

## Required sections

| Section | Must state |
| --- | --- |
| `Overview` | What the language is for, and the one-line character it is going for |
| `Colors` | Named tokens with values. Surface, text, brand/accent, and semantic roles |
| `Typography` | Font families as tokens, plus a size/weight hierarchy |
| `Layout` | A spacing scale and a grid/container contract |
| `Shapes` | Border-radius scale |
| `Components` | Per-component treatment for at least buttons, inputs, and cards |
| `Motion` | See below — the section this schema was created to add |
| `Device & Input` | See below |
| `Responsive Behavior` | Breakpoints, touch-target minimum, and a collapsing strategy |
| `Do's and Don'ts` | Concrete, checkable statements, not sentiment |

## `## Motion`

Motion is the section no vendor doc currently carries, and its absence is why nothing
in the pipeline could judge animation. It must state:

- **Duration scale** — named tokens with values (e.g. `instant`, `fast`, `base`,
  `slow`). Implementations must reference tokens, never raw milliseconds.
- **Easing curves** — named tokens with their `cubic-bezier` or spring parameters, and
  what each is for (entrances differ from exits).
- **Choreography patterns** — at minimum `enter`, `exit`, `stagger`, and `layout-shift`:
  what moves, in what order, over which duration and easing.
- **What must never animate** — an explicit list. Absent this, "tasteful motion" is
  unfalsifiable.
- **Reduced-motion fallback, per pattern.** Not a blanket "disable animations": each
  pattern states its `prefers-reduced-motion: reduce` behaviour, which is usually an
  opacity change rather than nothing at all.

A doc whose reduced-motion guidance is a single global note does **not** satisfy this
section. Motion and accessibility are not separable: WCAG 2.2.2 (pause/stop/hide),
2.3.1 (three flashes), and 2.3.3 (animation from interactions) all constrain what a
motion system may specify, so a motion section without fallbacks is an accessibility
defect rather than a documentation gap.

## `## Device & Input`

Viewport width is not a device. This section must state:

- **Pointer behaviour** — what changes under `pointer: coarse`, and what must not
  depend on `any-hover: hover`.
- **Hover availability** — every hover affordance's non-hover equivalent.
- **Safe-area insets** — how fixed and sticky elements respect them.
- **Orientation** — what reflows on rotation.
- **Asset density** — DPR expectations for raster assets.

## Optional sections

`Elevation & Depth`, `Iteration Guide`, `Known Gaps`. `Known Gaps` is encouraged: a doc
that admits what it has not decided is more useful than one that implies completeness.

## Validation

A conforming doc has every required `##` heading, non-empty. The `Motion` and
`Device & Input` sections additionally require every bullet above to be present —
their whole purpose is to be specific, and a heading with prose under it does not give
an implementer or a gate anything to act on.
