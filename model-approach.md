# Modeling Approach - Coca-Cola Bottle Spline Reconstruction

## Overview
This document describes the spline‑based modeling workflow used to reconstruct a classic Coca‑Cola bottle. The goal is to achieve visual accuracy with minimal control points while maintaining G1/G2 continuity.

## Reference
All modeling is based on the orthographic drawing of patent D48,273 (linked in README). The bottle is decomposed into three primary 2D profiles:
1. **Main body profile** – silhouette from base to shoulder.
2. **Neck profile** – transition from shoulder to threaded neck.
3. **Base dimple profile** – concave depression at the bottom.

## Spline Types & Justification

### 1. Main Body Profile
- **Spline type**: NURBS curve, degree 3.
- **Rationale**: NURBS provide local control and exact representation of conic sections (the bottle’s curved waist and shoulder). Degree‑3 ensures G2 (curvature) continuity across the entire height, which is essential for a smooth, glass‑like surface.
- **Control points**: 18 points (final count).
- **Placement strategy**: Control points are positioned only where curvature changes significantly – at the concave waist, convex hip, shoulder flare, and the gentle taper toward the base. Points are spaced closer in high‑curvature regions, wider in nearly‑linear sections.
- **Continuity**: G2 continuity is enforced by aligning control points and weights to maintain smooth second‑derivative transitions.

### 2. Neck Profile
- **Spline type**: B‑Spline, degree 3.
- **Rationale**: B‑Splines offer smoothness without the “corner‑pulling” characteristic of Bézier curves, making them ideal for the gentle taper and the sharp ring at the base of the neck. Degree‑3 guarantees G2 continuity along the curve.
- **Control points**: 12 points (final count).
- **Placement strategy**: Points are concentrated around the ring feature and the threaded‑neck start. The taper is captured with three points; the ring uses four points to create a sharp but still smooth transition.
- **Continuity**: G2 within the curve; G1 (tangential) transition to the main body at the shoulder.

### 3. Base Dimple Profile
- **Spline type**: Cubic Bézier curve.
- **Rationale**: Béziers allow precise placement of start/end points and tangents, making it easy to create a symmetric depression with a mirrored half. The dimple is a simple convex‑concave shape that can be defined with few control points.
- **Control points**: 8 points (mirrored, 4 per side).
- **Placement strategy**: The curve is built as a closed symmetrical shape. Control points are placed at the inflection points of the dimple, with handles adjusted to produce a smooth, rounded depression.
- **Continuity**: G1 at the symmetry line; G2 within each half.

## Modeling Process
1. **2D Curve Creation** – Each profile was drawn in Rhino 7 using the appropriate spline type. Control points were added iteratively, removing unnecessary points while preserving shape accuracy.
2. **Export** – Curves were exported as SVG (vector) files for version control (see `profile‑curves/`). SVG allows diff‑based tracking of control‑point changes.
3. **3D Generation** – The main body and neck profiles were revolved 360° around the bottle’s central axis (Y‑axis) to create NURBS surfaces. The base dimple was revolved separately and Boolean‑unioned with the main body.
4. **Continuity Checking** – Using Rhino’s curvature analysis tools, G1/G2 continuity was verified across all surfaces. Minor adjustments were made to control‑point weights (for NURBS) and knot vectors to eliminate unwanted curvature spikes.
5. **Final Mesh** – The NURBS surfaces were converted to a high‑quality quad‑dominant mesh (using Rhino’s `MeshPatch`) for rendering and export.

## Efficiency Analysis
- **Initial estimates** (from the project‑plan issue): body 18‑22, neck 10‑12, base 6‑8.
- **Final counts**: body 18, neck 12, base 8.
- **Reduction**: The body curve achieved the lower bound of the estimate by carefully placing control points at curvature extrema only. The neck and base curves matched the estimates; no further reduction was possible without sacrificing smoothness or shape fidelity.
- **Total control points**: 38 (2D) → after revolution, the 3D surface is defined by these same 38 points, demonstrating data efficiency.

## Challenges & Solutions
- **Shoulder‑to‑neck transition**: Achieving G1 continuity between the body and neck surfaces required aligning the tangent vectors of the respective profile curves. This was solved by adding a shared control point at the transition and adjusting its weight in the NURBS curve.
- **Base dimple merging**: The Boolean union created a small crease at the seam. Using a blend surface (with continuity constraints) instead of a simple union produced a smoother transition.
- **Logo curves (optional)**: The classic script was attempted using composite Bézier curves. Due to time constraints, only a simplified placeholder was included; a full logo reconstruction would require ~200 control points and is considered a separate artistic challenge.

## Files & Artifacts
- `profile‑curves/` – SVG exports of all 2D spline profiles.
- `bottle_model.obj` – Final 3D mesh (quad‑dominant, 5k faces).
- `renders/final_render.png` – Rendered image superimposed on the orthographic reference.

## Next Steps
- Validate the model against physical measurements (if reference dimensions are available).
- Animate the spline‑construction process for educational demonstration.
- Extend the workflow to other classic industrial‑design objects (e.g., wine bottle, perfume flask).
