# Waste Warrior Design System

**Less waste, more table.**

The reservation system for restaurants that don't want to over-order. A warm, mineral palette anchored by deep teal, with a high-contrast pop for moments that need to sing.

---

## Architecture

The system ships as a single CSS file — `colors_and_type.css` — that defines tokens against `:root` (the default palette) and a `.contrast` class that swaps the same token surface for a deep-teal scheme. Apply `.contrast` to any container to flip an entire section without changing the DOM.

```html
<section class="contrast">
  <button class="btn btn-primary">Add reservation</button>
</section>
```

The button still reads `var(--primary)` but now resolves to phosphor green on deep teal.

---

## Color

### Default palette — `:root`

Sun-bleached cream meets mineral teal.

| Role | Token | Value (oklch) |
|---|---|---|
| Primary | `--primary` | `.42 .053 190` — deep mineral teal |
| Secondary | `--secondary` | `.70 .110 68` — warm copper / tan |
| Ring | `--ring` | `.74 .056 131` — muted sage focus ring |
| Destructive | `--destructive` | `.55 .190 18` — deep red |
| Background | `--background` | warm cream |
| Card | `--card` | lighter cream surface |
| Muted | `--muted` | warm light grey |
| Accent | `--accent` | warm beige |
| Border | `--border` | warm grey |
| Foreground | `--foreground` | near-black, warm |

### Contrast palette — `.contrast`

Wrap a section in `.contrast` to flip into deep teal with bright phosphor-green primary.

| Role | Token | Value (oklch) |
|---|---|---|
| Primary | `--primary` (flipped) | `.85 .150 124` — phosphor green |
| Secondary | `--secondary` (shared) | `.70 .110 68` — same copper |
| Accent | `--accent` | `.52 .060 170` — dark muted sage |
| Destructive | `--destructive` | `.74 .260 18` — hot pink |
| Background, card, muted, border | inherited | deep teal shades |
| Foreground | `--foreground` | cream |

### Contrast rule

Text **must** clear WCAG AA against its background. Safe pairs:

- `--foreground` on `--background` / `--card` / `--muted` / `--accent`
- `--primary-foreground` on `--primary`
- `--secondary-foreground` on `--secondary`
- `--destructive-foreground` on `--destructive`

Never invent ad-hoc colour-on-colour combinations — always use a token's matched `*-foreground` pair.

---

## Typography

Asap Condensed for display & headings; Asap for body. **Locally hosted** via `fonts/`.

| Role | Family | Size | Weight | Token group |
|---|---|---|---|---|
| Display | Asap Condensed | 40 / 1.05 | 700 Bold | `--fs-display` |
| Headline | Asap Condensed | 28 / 1.15 | 600 SemiBold | `--fs-headline` |
| Subhead | Asap Condensed | 20 / 1.25 | 500 Medium | `--fs-subhead` |
| Body | Asap | 17 / 1.5 | 400 Regular | `--fs-body` |
| Label | Asap | 14 / 1.4 | 500 Medium | `--fs-label` |
| Eyebrow | Asap | 11 / 1.2 | 500, tracked +0.08em | `--fs-eyebrow` |

### Casing rule

**Sentence case everywhere.** No uppercase headings, subheadings, labels, chips, nav items or meta text. The eyebrow style is tracked but NOT uppercase. Product names ("Waste Warrior") and acronyms stay capitalised inline; everything else reads naturally.

---

## Spacing & radii

`--space-1` … `--space-20` on a 4-unit scale (4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80 px).

Radii: `--radius-xs` (4) · `--radius-sm` (6) · `--radius` (10, default) · `--radius-md` (12) · `--radius-lg` (16) · `--radius-xl` (24) · `--radius-pill` (999).

## Elevation

`--elevation-0` (flat, default) · `--elevation-1` (subtle) · `--elevation-2` (floating).

**Cards prefer borders to shadows.** Reserve elevation for floating UI: toasts, popovers, dragged states. A static card on a cream surface should use `1px solid var(--border)`, not a shadow.

---

## Components

- **Buttons** — `.btn` + `.btn-primary` / `.btn-secondary` / `.btn-ghost` / `.btn-destructive`. Add `.btn-sm` for compact tables. Hover dims with `filter: brightness(0.95)`.
- **Form fields** — text input + label + optional hint. Focused state lights `--ring`. Error state lights `--destructive` and pinks the field background slightly.
- **Reservation cards** — bordered card with eyebrow / title / chips / note / actions. Works identically inside `.contrast`.
- **Charts** — categorical palette `--chart-1` … `--chart-5` + `--chart-ink`. Use the donut for single-metric breakdowns with a center stat, and the ranked podium for top-N rankings paired with a table. Bars use `--chart-5` (phosphor green) for "wins"; rank badges use `--chart-ink`.
- **No squiggles, no doodles, no hand-drawn marks** anywhere. The previous decorative squiggle layer has been retired.

---

## Imagery — ingredient clusters

Photographic ingredient cut-outs are the brand's only decorative element. They are composed into **clusters**, never scattered:

1. **Circle-pack layout** — each ingredient sits tangent to a neighbour with a tiny visual gap proportional to the smaller of the two. Place the largest first, then anchor each next ingredient to an existing one at a random angle, retrying until non-overlapping. **No overlap, ever** — every ingredient is fully visible.
2. **Random rotation** — each ingredient gets a random rotation in roughly ±15°. Variation reads as "tossed", not arranged.
3. **Preserved size ratios — driven by the source PNG.** Each ingredient cut-out is exported at its canonical brand size in pixels. The cluster algorithm uses `max(naturalWidth, naturalHeight)` as the bounding circle and applies a *single uniform scale factor* to the whole composition. This means the ratio between any two ingredients is permanently locked: if parsley is 421 px tall and the dark berry is 30 px, the parsley will *always* render ~14× the berry, regardless of cluster size. Never override an ingredient's intrinsic size — re-export the PNG instead.
4. **Cluster, don't scatter** — the cluster sits as a single visual group, not strewn across the canvas.

### Adding new ingredients

Drop a new PNG into `assets/ingredient-*.png`, sized at its brand-canonical pixel dimensions relative to the existing assets (e.g. a tomato should be exported larger than a peppercorn). Add the path to the `INGREDIENT_SRCS` array in `preview/brand-ingredients.html`. No ratio math needed — the algorithm reads it directly.

---

## Voice

Warm operator-to-operator. Dutch primary, English secondary. Direct second person (je/jouw, you/your). Outcome-first, numbers do the heavy lifting, no emoji, no all-caps, no preachy sustainability language. Examples: *"Less waste, more table."* · *"Tonight at 19:30."* · *"Saved 2 portions."*

---

## Caveats & substitutions flagged

- **Ingredient ratios** are driven directly by source PNG dimensions — no manual ratio table.
- **`oklch()` colour syntax** — supported in modern browsers (Chrome 111+, Safari 15.4+, Firefox 113+). If you need older-browser support, plan to ship hex fallbacks via build tooling.

---

## Index

- `colors_and_type.css` — tokens, `@font-face`, `.contrast` variant, utility classes.
- `fonts/` — Asap Condensed family (local).
- `assets/` — logos (green, white, icon, stamp), ingredient cut-outs.
- `preview/` — design-system tab cards (colors, type, components, brand).
- `README.md` — this file.
- `SKILL.md` — agent skill entrypoint.
