# Interactive Cubic Bézier Curve with Physics & Sensor Control (Web)

**Author:** _Your Name Here_  
**Files:** `index.html` (single-file demo)  
**Platform:** Web (HTML + JavaScript + Canvas) — mouse & touch input  
**Goal:** A real-time, interactive cubic Bézier curve that behaves like a springy rope, visualizes tangents, and accepts mouse/touch input. All Bézier math and physics are implemented from scratch (no built-in Bézier or physics libraries).

---

## Features

- Implements a **cubic Bézier curve** using 4 control points: `P0`, `P1`, `P2`, `P3`.
- `P0` and `P3` are **fixed endpoints**. `P1` and `P2` are **dynamic** and driven by a spring-damping physics model.
- Bézier curve sampled at `t` increments of **0.01** (101 points) for smooth rendering.
- Tangents computed using the **exact derivative** and drawn along the curve.
- Interactive controls:
  - Move the mouse to influence the rope.
  - Click & drag `P1` or `P2` to directly reposition them.
  - Touch support for mobile devices.
  - Double-click to set current positions as the new rest/base positions.
- Runs at ~60 FPS using `requestAnimationFrame`. Physics uses semi-implicit Euler integration with a capped timestep for stability.
- No external libraries or built-in Bézier APIs are used.

---

## Mathematics

### Cubic Bézier (position)
For `t` in `[0,1]`: 
B(t) = (1 − t)^3 P0
+ 3(1 − t)^2 t P1
+ 3(1 − t) t^2 P2
+ t^3 P3

### Cubic Bézier (derivative / tangent)
B’(t) = 3(1 − t)^2 (P1 − P0)
+ 6(1 − t) t (P2 − P1)
+ 3 t^2 (P3 − P2)

Tangents are normalized and drawn as short line segments at several sample points along the curve.

---

## Physics model

Each dynamic control point (`P1`, `P2`) is simulated with a simple spring-damper toward a **target position**:
acceleration = -k * (position - target) - damping * velocity


Integration uses semi-implicit Euler:
1. `v += a * dt`
2. `pos += v * dt`

Constants used in the demo (tweakable):
- `K = 16.0` (spring stiffness)
- `DAMP = 6.0` (damping coefficient)
- `MAX_DT = 0.033` (caps dt for stability)

Mouse movement updates the `target` for `P1` and `P2` using a center-relative influence map:
- horizontal movement affects `P1` and `P2` differently (left influences `P1` more; right influences `P2` more),
- vertical movement affects both,
- effect scales with distance from center.

---

## How to run

1. Save the provided `index.html` (single file) to a local folder.
2. Open `index.html` in any modern browser (Chrome, Firefox, Safari).
3. Interact:
   - Move the mouse over the canvas to see rope movement.
   - Click and drag the magenta control points (P1 or P2).
   - Double-click to set current P1/P2 as new rest positions.

No server is required. For strict local-file security modes, you can serve the file with a minimal static server:

```bash
# Python 3
python -m http.server 8000
# Then open: http://localhost:8000/index.html
