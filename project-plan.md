# Project Plan: Bottle Deconstruction

*Note: This document serves as the project planning artifact. A GitHub issue could not be created due to tooling limitations.*

## Bottle Decomposition – Spline Components

### 1. Main Body Profile Curve (for lathing/revolving)
- **Purpose**: Defines the overall silhouette of the bottle from base to shoulder.
- **Ideal spline type**: **NURBS curve (degree 3)** – provides smooth curvature and local control while maintaining G2 continuity across the entire height.
- **Estimated control points**: 18–22 points.

### 2. Neck Profile Curve
- **Purpose**: Describes the transition from shoulder to the threaded neck, including the characteristic taper and the ring at the base of the neck.
- **Ideal spline type**: **B‑Spline (degree 3)** – offers smoothness and avoids the “corner pulling” of Bézier curves.
- **Estimated control points**: 10–12 points.

### 3. Base/Dimple Profile Curve
- **Purpose**: Captures the concave dimple at the bottom of the bottle.
- **Ideal spline type**: **Bézier curve (degree 3)** – allows exact placement of start/end points and tangents.
- **Estimated control points**: 6–8 points (mirrored for symmetry).

### 4. Logo/Text Curves (optional stretch goal)
- **Purpose**: Recreate the classic “Coca‑Cola” script and contour label.
- **Ideal spline type**: **Composite Bézier curves (degree 2 & 3)**.
- **Estimated control points**: ~50–80 points per letter (high‑detail vector).

## Modeling Strategy
- Each profile will be modeled as a **2D curve** in a vector‑editing or CAD environment (Rhino 7).
- Curves will be exported as `.svg` or `.dxf` files for version control.
- The main body and neck profiles will be revolved around the central axis to generate the 3D surface.
- The base dimple will be created as a separate revolved surface and Boolean‑unioned (or blended) with the main body.
- Continuity goals:
  - **G1 (tangential)** for transitions between body and neck.
  - **G2 (curvature)** within each major component where smoothness is critical.
- Control points will be placed **only where curvature changes significantly** to minimize data while preserving shape fidelity.

## Efficiency Targets
The challenge is to achieve visual accuracy with the **minimum number of control points** without compromising smoothness. Initial estimates above are conservative; the final model should aim to reduce those counts while maintaining G1/G2 continuity.

## Next Steps
1. Create branch `model/bottle‑construction`.
2. Model each profile curve and export to `profile‑curves/`.
3. Document the modeling rationale in `model‑approach.md`.
4. Generate 3D model and final render.
5. Submit via pull request.

---
*This plan was created as part of the Spline Efficiency Challenge.*