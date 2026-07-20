---
name: tufte-data-viz
description: >-
  Make data visualizations potent in the tradition of Edward Tufte's "The Visual
  Display of Quantitative Information." Use BEFORE building ANY chart, graph, plot,
  table, diagram, dashboard, coverage bar, stat callout, or node-link graphic — in
  HTML/SVG, a plotting library, or an artifact. Triggers: "chart", "graph", "plot",
  "visualization", "dashboard", "make it potent / Tufte-style", "data-ink", "small
  multiples", "sparkline", "bipartite", "coverage bars", or any request to show
  quantities visually.
---

# Potent Data Visualization (Tufte)

Graphical excellence is the well-designed presentation of interesting data — substance,
statistics, and design working together. The goal: **give the viewer the greatest number
of ideas in the shortest time with the least ink in the smallest space.** Above all else,
show the data.

Load this before choosing a chart type or writing the first line of chart code. Then follow
the checklist and cut everything that isn't data.

## The five principles

1. **Maximize the data-ink ratio.** Data-ink is the non-erasable core that shows the
   numbers. Every other pixel is a candidate for deletion. Iterate: draw it, then erase
   ink that carries no information, then erase again. Redundant ink (a number *and* a bar
   *and* a gridline all saying "40%") keeps only the clearest one.

2. **Erase chartjunk.** No moiré patterns, no heavy gridlines, no 3-D, no gradients-for-
   decoration, no drop shadows, no boxed legends, no ducks (graphics that decorate rather
   than inform). A faint grid is usually deletable; a bounding box usually is; tick marks
   usually thin out to a few.

3. **Direct-label; kill the legend.** Put the series name at the end of its line, the value
   at the end of its bar, the category beside its dot. A legend forces the eye to
   ping-pong between a color key and the marks. Direct labeling removes the lookup.

4. **Small multiples.** Repeat the same small chart across a variable (time, category,
   provider). Once the viewer learns to read one frame, they read all of them. Consistent
   scales across frames make differences pop.

5. **Show comparison, and show absence.** Data becomes meaning through comparison — against
   a baseline, a target, another series, or zero. Make the missing datum as legible as the
   present one: an empty track, a "0 · not sent", a node with no edge. Absence is data.

## Supporting rules

- **Graphical integrity.** The representation of numbers is proportional to the numbers.
  Start bar axes at zero. The Lie Factor (size-of-effect-shown / size-of-effect-in-data)
  stays near 1. Don't encode one number with area when you mean length.
- **Layering and separation.** Distinguish elements with the lightest effective difference
  — a slightly heavier weight, a muted vs. saturated tone, foreground vs. 20%-gray
  background — not with loud, competing colors. Most marks should be quiet; emphasis is
  scarce and earned.
- **Word–graphic integration.** Labels, numbers, and a one-line takeaway live *inside* the
  graphic, not in a caption far away. A sentence naming what the chart shows beats a title
  that only names the axes.
- **High data density.** Prefer many small honest marks to a few big decorated ones.
  Sparklines, dot-strips, and dense tables respect the reader's intelligence and the page.
- **The table is often the answer.** For a handful of numbers, a well-set table (right-
  aligned, tabular figures, no vertical rules) beats any chart. Reach for a chart only when
  the shape of the comparison carries the meaning.

## Checklist (run before shipping any visual)

- [ ] Can I delete this without losing information? (grid, box, legend, 3-D, shadow, fill) → delete it
- [ ] Is every series/bar/dot directly labeled, so there's no legend lookup?
- [ ] Bars start at zero; lengths/areas are proportional to the numbers (Lie Factor ≈ 1)?
- [ ] Is there an explicit comparison or baseline? Is absence shown, not just omitted?
- [ ] One accent carries emphasis; everything else is quiet (neutral or 20% gray)?
- [ ] Do digits use tabular figures and align in their column?
- [ ] Is the takeaway readable *in* the graphic, without hunting for a caption?
- [ ] Would a small table say it more plainly? If yes, use the table.

## Practical patterns (HTML / SVG / print)

- **Coverage / proportion bars:** one row per item, sorted by value; a thin track with a
  single-hue fill; the count and % direct-labeled at the end; a zero row rendered as an
  empty track with the "0 / not sent" label in a warning tone. No axis, no gridlines.
- **Bipartite / mapping graph:** two columns of directly-labeled dots, thin 1px edges only
  between matches; unmatched nodes on each side grouped under a small subhead ("beyond
  scope", "not sent") and left edgeless — the gaps are the point.
- **Stat callouts:** the number large in a display face with tabular figures; a plain
  one-line gloss beneath. No icon, no colored tile, no border.
- **Sparklines / trends:** small, wordless in-line lines; mark only the endpoint or the
  extreme; no axis.
- **Color:** brand-neutral quiet neutrals for structure, ONE accent for emphasis, plus a
  single semantic warning hue (used only for gaps/errors, never as decoration). Design for
  light and dark via tokens; keep contrast legible in both.
- **Type in charts:** tabular-nums for anything that lines up in a column; uppercase mono
  micro-labels with letter-spacing for axis/section tags; balance headings.
- **Print/PDF:** set `print-color-adjust: exact` so fills survive; keep each visual
  `break-inside: avoid` so it never splits across a page.

## Anti-patterns (delete on sight)

Pie charts with many slices · dual y-axes · rainbow categorical palettes · legends that
could be direct labels · gridlines darker than the data · 3-D bars · gradient fills for
decoration · truncated bar axes · icons inside stat tiles · a chart where a five-row table
would be clearer.

## Source

Edward R. Tufte, *The Visual Display of Quantitative Information* (2nd ed.), and
*Envisioning Information*. When in doubt: show the data, and erase everything else.
