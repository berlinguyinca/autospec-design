# autospec-design

The machine-readable design contract for [autospec](https://github.com/berlinguyinca/autospec):
what a `DESIGN.md` must contain, and which of those requirements a machine can check.

This repo holds the **schema and the rules**. It does not hold vendor design
languages — those live in
[`berlinguyinca/awesome-design-md`](https://github.com/berlinguyinca/awesome-design-md),
whose per-vendor `DESIGN.md` files are expected to conform to the schema here.

## Why this exists

Autospec could already adopt a vendor design language and enforce token fidelity
against it. Three gaps made that enforcement narrower than it looked:

1. **No motion vocabulary anywhere.** A repo-wide search of autospec for
   `prefers-reduced-motion`, `@keyframes`, `transition-duration`, and `motion-safe`
   returned zero hits, and the vendor docs have no motion section either — the Linear
   doc runs 547 lines across 30+ headings without one. A pipeline cannot enforce
   animation quality against a design language that never mentions animation.
2. **Fidelity was judged against `DESIGN.md`, never the quality of `DESIGN.md`.**
   Adopt a bland design language and the pipeline enforces blandness perfectly. A
   schema with required sections is what makes the input reviewable.
3. **`.autospec/design-gates.yml` expects a baseline pack `rules.yaml`** from a
   companion repo, and no such pack existed. `rules.yaml` here is that pack.

## Contents

| Path | Purpose |
| --- | --- |
| `schema/design-md.md` | Required and optional `DESIGN.md` sections, with the contract for each |
| `rules.yaml` | Baseline pack: rule ids, severity, and how each is checked (`auto` / `vlm` / `review`) |
| `_baseline/DESIGN.md` | Schema-conformant defaults, so a vendor doc that predates a schema section still resolves one |

## Consumers

- **`/autospec-design`** fetches a vendor `DESIGN.md` and writes it to a target repo's
  root. It validates against `schema/design-md.md`.
- **`scripts/autospec-design-gates.sh`** in autospec maps `check: auto` rule ids from
  `rules.yaml` to local commands and writes an evidence report. `check: vlm` and
  `check: review` rules become the critic checklist for the visual-fidelity pass
  rather than being executed.
- **`scripts/lint-ui.sh`** in autospec implements several `check: auto` rules directly;
  the `implements` field on each rule records which.

## The three check classes

`rules.yaml` marks every rule with how it can honestly be verified. This distinction
is the point of the pack — it keeps a machine-checkable subset from being reported as
full conformance:

- **`auto`** — a command decides it. Deterministic, blocking.
- **`vlm`** — needs a vision judgment against a rendered screenshot. Advisory by
  default, because vision judgments are noisy; gate on regression, not absolute score.
- **`review`** — needs a human. Meaningful alt text, link-text meaningfulness, reading
  order, and plain language sit here and cannot be automated away.

## Status

Early. The schema, the pack, and the baseline are seeded from
`docs/superpowers/specs/2026-08-04-autospec-web-ui-design.md` in autospec. Vendor docs
in `awesome-design-md` do **not** yet carry the `Motion` or `Device & Input` sections
this schema requires; until they do, `_baseline/DESIGN.md` supplies the defaults.
