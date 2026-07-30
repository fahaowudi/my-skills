# HTML Design Reference

A default design language to start from when building the HTML deliverable in Stage 5. **This is a starting point, not a fixed template** — adapt colors, typography, and signature elements to the subject. A report on agriculture should not look identical to one on finance.

Read this before Stage 5, then make deliberate choices. Load the `frontend-design` skill for fuller design guidance.

## Design philosophy

The HTML is a **deliverable**, not decoration. Three jobs, in priority order:
1. Make the report scannable — the user should grasp the structure and key findings without reading top-to-bottom.
2. Make data legible — tables, bars, matrices, callout numbers should read instantly.
3. Have one memorable "signature element" per report that embodies the thesis (e.g., a milestone timeline, a heatmap matrix, a funnel diagram).

Spend your boldness on the signature element. Keep everything else disciplined and quiet.

## Default design language (the "field-note" palette we've used)

This warm, paper-based palette works well for research/consulting deliverables and reads as serious-but-not-corporate. Use it as the default when the subject doesn't call for something more specific.

### Color tokens

```
--ink:       #1f1a12   /* primary text — warm near-black */
--soil:      #3d342a   /* secondary text — deep brown */
--paper:     #f5efe1   /* page background — wheat paper */
--paper-2:   #ede4d0   /* block background — slightly deeper paper */
--chaff:     #c9b78f   /* hairlines, dividers — straw */
--ochre:     #b8531a   /* THE accent — ochre red, used sparingly */
--ochre-deep:#8a3a12   /* hover / deep accent */
--grain:     #7a6a4a   /* muted captions, labels — chaff gray-brown */
--warn-bg:   #f4e6d4   /* warning callout background */
--warn-bd:   #b8531a   /* warning callout border (= ochre) */
--ok-bg:     #e8ead9   /* positive callout background */
--ok-bd:     #5c6b3a   /* positive callout border — moss */
```

**Rules:** one accent only (ochre). Use it for eyebrows, key numbers, the signature element, links. Everything else is ink/soil/grain on paper. Resist adding a second accent — it dilutes the signal.

### Typography

Pair a **serif display** for headings with a **sans-serif** for body, plus a **monospace** for data/labels/code. This trio (serif character + sans clarity + mono precision) is what makes the page read as a designed document rather than a default.

```
Headings:   "Noto Serif SC", serif        (weights 400/500/700/900)
Body:       "Noto Sans SC", sans-serif    (weights 300/400/500/700)
Mono/data:  "JetBrains Mono", monospace   (weights 400/500/700)
```

Load from Google Fonts with `<link>`. Set a clear type scale: hero H1 `clamp(32px, 6vw, 60px)`, section H2 `clamp(24px, 4vw, 36px)`, body 16px, mono labels 11-13px with letter-spacing for the "data" feel.

### Layout primitives

- **Max width** ~880-1040px, centered. Wider for matrix-heavy reports.
- **Section rhythm**: each `<section>` padded ~60px vertical, separated by 1px hairline (`--chaff`).
- **Eyebrows**: small mono uppercase labels above headings (e.g. `01 — METRICS`), with a short ochre line. These encode structure.
- **Callout boxes**: `.warn` (warning/risk), `.note` (aside), `.pull` (pull-quote). Left border accent, tinted background.

### Signature element patterns

Pick ONE per report, matching the content's natural structure:

- **Milestone ladder/timeline** — when the report has stages or phases (e.g., growth milestones 100→500→1k→5k fans). Vertical line + nodes, mark "you are here."
- **Heatmap matrix** — when comparing items × dimensions (e.g., track × platform fit). Color cells by score (5 levels from light to ochre). Always include a legend.
- **Bar charts** — for composition breakdowns (traffic sources, age distribution). Horizontal bars, key bar in ochre.
- **Ranked priority cards** — when the report's punchline is a ranked list (top priorities, key metrics). Large numerals + hover shift.
- **Verdict split** — when there's a clear "what went right / what's broken" duality (e.g., single-post diagnosis). Two-cell grid, green vs red tint.

### Paper texture (optional, adds tactility)

A subtle SVG noise overlay via `body::before` with low opacity (~0.35) gives a "printed document" feel without being noisy. Use the turbulence-filter SVG pattern. Skip for data-dense reports where it hurts legibility.

## Adaptation guidance

The default palette is warm/earthy — ideal for agriculture, food, craft, consulting. For other subjects, shift the accent while keeping the structure:

- **Finance/investing** → cooler, more authoritative: ink navy `#1a2438` + a single restrained gold or teal accent. Paper stays light.
- **Tech/SaaS** → cleaner, more whitespace, cooler grays; accent could be electric blue or violet. Drop the paper texture.
- **Health/medical** → calm, trustworthy: soft blues/greens, generous whitespace, no harsh reds (red = warning only).
- **Creative/lifestyle** → can go warmer and more expressive; serif display can be more characterful.

Always keep: one accent only, serif+sans+mono trio, one signature element, scannable structure, scannable data.

## Quality bar before declaring the HTML done

- Responsive down to mobile (test the matrix/bars don't overflow).
- `prefers-reduced-motion` respected (kill transitions/animations).
- Key interactive elements keyboard-focusable.
- Open in browser, verify via DOM snapshot that color tokens applied, fonts loaded, signature element rendered, tables intact. Fix before reporting done — don't ship an unverified file.
