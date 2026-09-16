# Short Report — Volume and Shape (Cubic Volume Measurement, Form Factor, Form Quotient)
### Stand 8, Eucalyptus — 36 cubed trees

---

## Part A — Comparison of cubic volume methods (Smalian, Huber, Newton)

### Results summary

All three methods were applied to each of the 36 trees. Per-tree volumes are in
`resultado_final_cubagem_forma.xlsx`; the comparison statistics below summarize
the differences.

| | Smalian vs. Huber |
|---|---|
| Mean difference | +0.063% |
| Median difference | +0.057% |
| Minimum difference | +0.046% |
| Maximum difference | +0.124% |

**Stand-level totals** (sum across all 36 trees):

| Method | Total volume (m³) |
|---|---|
| Smalian | 8.297 |
| Newton | 8.293 |
| Huber | 8.292 |

Stand-level difference, Smalian vs. Huber: **0.07%**.

### Is there a relevant difference? In which direction?

**A small one, consistent in direction across every tree: Smalian ≥ Newton ≥
Huber, in all 36 of 36 trees, without exception.**

This is not incidental variation — it is a direct mathematical consequence of how
each method estimates the shape of each ordinary (non-tip) section:

- **Smalian** takes the two end **areas** of a section and averages them directly.
- **Huber and Newton** compute area **from an interpolated (averaged) diameter**
  at the section midpoint.
- Cross-sectional area is a **convex (quadratic) function** of diameter
  (`A = π/4 × d²`). By Jensen's inequality, the area calculated from an *averaged
  diameter* is always less than or equal to the *average of the two end areas*.
  Equality only holds when the two end diameters are identical (no taper in that
  section).

The practical result: Smalian systematically produces the slightly largest
volume estimate, Huber the slightly smallest, and Newton — a weighted blend of
both endpoints plus the midpoint (`(A0 + 4×Am + A1)/6`) — falls consistently in
between.

### The tip section: treated as a cone

Every tree's **last section** ends at the treetop, where diameter reaches 0.
Since a section tapering all the way to a single point is a **cone**, not a
frustum, its volume is calculated using the analytical cone formula,
`(1/3) × A0 × L`, for Smalian and Huber. Newton's formula (Simpson's rule)
already equals this exact value automatically, given a linearly-interpolated
midpoint diameter, so no special handling was needed there — this is a genuine
property of Simpson's rule for a cone shape, not an approximation.

Handling the tip section this way keeps the three methods' agreement tight
across the whole dataset, including the two shortest trees (Trees 1586 and
1587, HT 13.6 and 14.4 m), where the tip section represents a proportionally
larger share of total stem length. Their Smalian-vs-Huber differences (0.06%
and 0.11% respectively) stay in line with the rest of the dataset rather than
standing out as outliers.

### Is this expected, given how the data was collected?

**Yes, for a specific reason worth stating clearly in this report.** The Huber
and Newton midpoint diameters in this analysis are not independently measured —
they are *derived* from the same two endpoint diameters that Smalian already
uses (via linear interpolation, since the data was collected in the Smalian
pattern only). This means the comparison here is not really testing three
independent field methods; it is testing how three different **mathematical
shape-assumptions** (trapezoidal for ordinary sections; the analytical cone
formula at the tip) reshape the *same* underlying two-point measurements. A
genuine field comparison would require an actual measured middle diameter for
Huber and Newton — information this dataset does not contain (see `README.md`
for the full justification of the interpolation strategy adopted).

### Where the differences are largest

Across the dataset, Smalian-vs-Huber differences range from 0.046% to 0.124% —
a narrow band with no dramatic outliers. The largest relative differences occur
among the mid-sized trees rather than following any obvious pattern by tree
size, consistent with the differences being driven by ordinary section-level
taper rather than any single structural factor.

---

## Part B — Tree shape: form factor and form quotient

### Overall results

| Indicator | Mean | Min | Max | Std. dev. |
|---|---|---|---|---|
| Form factor (f) | 0.478 | 0.408 | 0.540 | 0.025 |
| Girard form quotient (q) | 0.884 | 0.765 | 0.923 | 0.037 |

A mean form factor of 0.478 means these eucalyptus stems, on average, fill just
under half of the cylinder that bounds them (DBH × HT) — a completely normal
range for young eucalyptus. A mean form quotient of 0.88 means the trunk has,
typically, only narrowed to 88% of its DBH by the time it reaches 5.2 m up —
relatively modest taper in that lower section.

### How form factor and form quotient vary with DAP and height

| DAP quartile | Mean DAP (cm) | Mean form factor | Mean form quotient |
|---|---|---|---|
| Q1 (thinnest) | 10.93 | 0.492 | 0.856 |
| Q2 | 14.46 | 0.478 | 0.889 |
| Q3 | 16.56 | 0.468 | 0.893 |
| Q4 (thickest) | 17.91 | 0.474 | 0.898 |

| Height quartile | Mean HT (m) | Mean form factor | Mean form quotient |
|---|---|---|---|
| Q1 (shortest) | 20.56 | 0.490 | 0.851 |
| Q2 | 26.13 | 0.473 | 0.891 |
| Q3 | 27.31 | 0.475 | 0.895 |
| Q4 (tallest) | 28.14 | 0.473 | 0.900 |

**Overall correlations:**

| | vs. DAP | vs. Height |
|---|---|---|
| Form factor | r = −0.40 | r = −0.34 |
| Form quotient | r = +0.55 | r = +0.64 |

### Do thinner/taller trees have a different shape? Is there a clear trend?

**Yes — a clear trend exists, and it moves in *opposite directions* for the two
indicators, which is a genuinely important, non-obvious result.**

**Form factor decreases as trees get larger** (both thicker and taller). This
reflects a real biomechanical pattern: taller trees experience greater
wind-driven bending stress at the base (a longer lever arm), so trees allocate
disproportionately more wood near the base as they grow — this is a well
documented structural response in stem growth. More base-thickening means more
taper overall, which shows up as a lower form factor. The trend is present but
moderate (r = −0.40 vs. DAP), with the clearest break between the thinnest
quartile and the rest, rather than a perfectly smooth gradient across all four
groups.

**Form quotient increases as trees get larger** — the opposite direction. This
is not a contradiction; it reflects a methodological property of using a
**fixed absolute height** (5.2 m) as the reference point. For a short tree,
5.2 m may represent a large fraction of its total height — deep into the
heavily-tapered part of the stem. For a tall tree, 5.2 m is still very close to
the base, barely past the point of maximum diameter. So taller trees "look"
less tapered at that one fixed 5.2 m mark, even though — per the form factor,
which accounts for the *entire* stem — they are not necessarily less tapered
overall. This is a recurring theme in forest mensuration: a metric anchored to
one fixed height doesn't scale consistently across trees of very different
sizes.

**Practical implication:** form factor is the more reliable shape indicator for
comparing trees of different sizes, precisely because it integrates the whole
stem rather than anchoring to one fixed point. Form quotient is easier to
collect (no felling or full cubic-volume measurement needed — just two diameter
readings on a standing tree), but its apparent trend with tree size is partly an
artifact of the fixed-height convention, not purely a shape difference.

### What would happen if a single average form factor were applied to the whole stand?

This was tested directly: the stand's mean form factor (**f̄ = 0.478**) was
applied to every tree's own cylinder volume (`f̄ × g_DAP × HT`), and the result
was compared against each tree's real (Smalian) volume.

**Individual-tree error is large:** ranging from **−11.6% to +17.2%**, with the
error **systematically correlated with tree size** (r = 0.36 vs. DAP) — not
random noise. Specifically:

| DAP quartile | Mean error using average f |
|---|---|
| Q1 (thinnest) | **−2.24%** (underestimated) |
| Q2 | ≈0.00% |
| Q3 | **+2.34%** (overestimated) |
| Q4 (thickest) | +1.00% |

This pattern follows directly from the finding above: since the thinnest trees
have a *higher* true form factor than the stand average, applying the (too-low)
average systematically **underestimates** their real volume. Since mid-to-large
trees (especially Q3) have a *lower* true form factor than the average, applying
the (too-high) average systematically **overestimates** their volume.

**Stand-level total error is small:** despite the large individual-tree swings,
the *total* stand volume estimated with the single average form factor
(8.357 m³) differs from the true total (8.297 m³) by only **+0.72%**. This
happens because the underestimates on small trees and the overestimates on
larger trees **partially cancel out** when summed across a stand with a roughly
balanced size distribution.

### In which situations would this simplification be more or less problematic?

**Less problematic:**
- When the only goal is **total stand volume** (e.g., overall yield estimation,
  broad growth-and-yield modeling) — the cancellation effect keeps the aggregate
  error small, as shown above (~0.7%).
- In **narrow, even-aged, single-clone plantations** like this one, where the
  DAP range is relatively tight (5.6–18.7 cm) — less room for the size-related
  bias to compound.

**More problematic:**
- Whenever volume is needed **by size class or individual tree** — for example,
  billing timber by diameter class (large sawlogs vs. small pulpwood), where a
  systematic ±2–17% bias by size class directly translates into over- or
  under-payment for specific timber grades.
- In **stands with a wider size range** (mixed ages, uneven-aged management,
  multiple clones/species) — the correlation between form factor and size would
  likely be even stronger, and the cancellation effect that kept the stand-level
  total error small here would be less reliable, since the size distribution
  driving that cancellation might not be as balanced.
- Any application sensitive to the **direction** of the bias, not just its
  average magnitude — e.g., a forest-carbon or biomass estimate that
  systematically underestimates small/young trees and overestimates larger ones
  would misrepresent the stand's true size structure, even if the grand total
  looked approximately right.

**Recommendation:** where feasible, fit form factor (or a full volume equation)
as a function of DAP and/or height, rather than using one stand-wide constant —
this directly corrects the systematic size-related bias documented above, at
relatively low additional analytical cost given the cubic volume measurement
data already collected for this stand.
