# Data Challenge 03 — Volume and Shape (Cubagem, Form Factor, Form Quotient)

## Files

| File | Purpose |
|---|---|
| `00_funcoes.R` | Shared functions (must be present alongside both scripts below) |
| `01_cubagem_volumes.R` | Part A — Smalian, Huber, Newton volumes + comparison |
| `02_forma_arvore.R` | Part B — form factor, Girard form quotient + FINAL consolidated output |
| `resultado_final_REFERENCIA_python.xlsx` | Independently-computed reference values (Python), to check your R output against |

## How to run

1. Download all three `.R` files into the same folder.
2. Open `01_cubagem_volumes.R`, edit the three CONFIG lines at the top (`INPUT_FILE`, `OUT_DIR`, `FUNCOES_FILE`) to point to your actual `cubagem.xlsx`, your desired output folder, and the `00_funcoes.R` file's path.
3. Do the same in `02_forma_arvore.R`.
4. Install the two required packages once: `install.packages(c("readxl", "writexl"))`
5. Run each script (`Rscript 01_cubagem_volumes.R`, then `Rscript 02_forma_arvore.R`, or run inside RStudio).

`02_forma_arvore.R` is self-contained — it recalculates all three volumes itself, so it can run independently even if you never run `01_cubagem_volumes.R`. **The file it produces, `resultado_final_cubagem_forma.xlsx`, is the actual required deliverable** (single consolidated output, exact column order per the assignment spec). `01_cubagem_volumes.R` is provided separately because the assignment explicitly asks for a distinct comparison-of-methods analysis (Part A, item 5) with its own discussion.

## Key methodological decisions (as required by the assignment)

### 1. Section midpoint diameter (Huber and Newton methods)

The data was collected in the Smalian pattern — diameters measured only at section **ends**, never at the middle. Since each section is bounded by exactly two measured points, we estimate the midpoint diameter via **linear interpolation between the two endpoint diameters**, evaluated at the section's own midpoint height. This is implemented through a single general-purpose function, `diametro_interpolado()`, rather than a special-cased shortcut.

Because the target height is precisely the interval's midpoint, this reduces mathematically to the simple average of the two endpoint diameters: `dm = (d0 + d1) / 2`.

**Limitation, documented explicitly:** real stem taper is usually slightly convex rather than perfectly linear between two points, so this likely *underestimates* the true midpoint diameter somewhat — meaning our Huber volumes are probably slight underestimates relative to what true, independently-measured middle diameters would give.

### 2. The ground segment (0 m → first measurement at 0.10 m)

No diameter was measured at ground level (hi = 0). Extrapolating one would be unreliable. We treat this short segment as a **cylinder**, using the diameter measured at the first point (0.10 m) applied over the full 0.10 m length. This avoids silently discarding real volume while keeping the assumption simple and low-risk, given how short the segment is (taper over 10 cm is negligible regardless of true shape). The same base-cylinder volume is added identically under all three methods, so it doesn't affect the *comparison* between methods — only each tree's absolute volume level.

### 3. Girard height diameter (5.2 m)

Estimated with the same `diametro_interpolado()` function used for the section midpoints, interpolating between the two measured points immediately below and above 5.2 m — exactly as instructed by the assignment. Verified: 5.2 m falls within the measured height range for all 36 trees (no extrapolation needed for this dataset).

### 4. The tip section — treated as a cone, not a frustum

Every tree's **last section** ends at the treetop, where diameter reaches 0. Every other section is reasonably approximated as a paraboloid frustum, which Smalian's formula is exact for — but a section that tapers all the way to a single point is a **cone**, not a frustum, and a frustum formula is not exact for a cone shape.

Deriving each method's tip-section estimate relative to the true cone volume `(1/3) × A0 × L`:

| Method | Tip-section formula, if treated generically | vs. true cone volume |
|---|---|---|
| Smalian | (1/2) × A0 × L | +50% too high |
| Huber (linear-interpolated midpoint) | (1/4) × A0 × L | −25% too low |
| Newton | (1/3) × A0 × L | exact |

Newton's formula (Simpson's rule) is already mathematically exact for the cone-shaped tip, given a linearly-interpolated midpoint diameter — no special handling needed there. **Smalian and Huber's tip sections are computed using the analytical cone formula directly**, `(1/3) × A0 × L`, rather than their general-purpose section formula. This is implemented in `vol_smalian()` and `vol_huber()` in `00_funcoes.R`, and is the standard treatment for this well-known limitation of frustum-based cubic volume methods.

## Key finding — the method comparison (Part A, item 5)

**Volumes follow the order Smalian ≥ Newton ≥ Huber in all 36 of 36 trees, without exception, though the differences are small.**

This is a direct mathematical consequence of how each method estimates the shape of an ordinary (non-tip) section:
- **Smalian** takes the two end **areas** of a section and averages them directly.
- **Huber and Newton** compute area **from an interpolated (averaged) diameter**.
- Cross-sectional area is a convex (quadratic) function of diameter, so — by Jensen's inequality — the area of an averaged diameter is always ≤ the average of the two end areas.
- Result: Smalian systematically gives the largest volume estimate, Huber the smallest, and Newton — a weighted blend, `(A0 + 4×Am + A1)/6` — falls in between.

Stand-level totals across all 36 trees: Smalian 8.297 m³, Newton 8.293 m³, Huber 8.292 m³ — a spread of only 0.07%, indicating the three methods agree closely once the tip section is handled correctly.

**Important framing for the report:** because our Huber/Newton midpoint diameters are themselves *derived* from the same two endpoint measurements Smalian uses (not independently measured), this comparison isn't really testing three independent instruments — it's testing how three different mathematical shape-assumptions (trapezoidal for ordinary sections, plus the analytical cone formula for the tip) reshape the *same* underlying data. A true field comparison would require an actual measured middle diameter for Huber/Newton, which this dataset does not contain.
