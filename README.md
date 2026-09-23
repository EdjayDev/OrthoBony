# OrthoBony

An interactive, educational orthopedic simulator for upper-limb forearm fractures. It visualizes how the arm's pose, support device, and healing progress affect the load on a radius/ulna fracture.

**Educational visualization only. The healing timeline and splint rules are simplified and are not clinical guidance.**

## Quick start

No build step, no dependencies. Open `index.html` in any modern browser.

## Features

- **Two views** — Anterior (front) and lateral (side) render the humerus, radius/ulna, union site, wrist, and hand.
- **Clinical X-ray vs. anatomical rendering** — X-ray mode uses an inverted, bone-density style render; muscle opacity is fixed in X-ray mode.
- **8 immobilization devices:**
  - Posterior Long Arm Elbow Splint (default)
  - Sugar-Tong Coaptation Splint
  - Volar Short Arm Splint
  - Ulnar Gutter Splint
  - Radial Gutter Splint
  - Thumb Spica Splint
  - Classic Arm Sling
  - No Support
- **Healing timeline** (Day 0–70) — acute fracture/hematoma → soft fibrous callus (day ~14) → hard bony callus (day ~35) → remodeling/healed (day ~56), visualized as the union site changes across the gap.
- **Interactive limb controls** — fracture site (proximal/mid/distal third), arm elevation, elbow flexion (0–140°), zoom, and support/muscle transparency.
- **Digit & tendon controls** — independent thumb, index, middle, ring, and pinky action.
- **Live safety feedback** — a gravity-load meter and status panel rating the current pose as *safe / warning / critical*, with fragment displacement visualized at the fracture.

## How the feedback model works

The status is the worse of two independent checks:

1. **Gravity load vs. callus capacity** — the bending moment at the fracture equals the weight of everything distal to it times the horizontal lever arm. The callus capacity grows along the healing phases `[day 0 → 0.1, day 14 → 0.3, day 35 → 1.25, day 56 → 3.0]` (units of "mid-shaft fracture, forearm horizontal"). Each device carries a fraction of that load—but only where it actually spans enough of the arm above the fracture.
2. **Movement restriction rules** — per-device thresholds (e.g., the posterior long splint tolerates only limited elbow flexion and digit movement before day 21; the sling means no lifting the arm).

Fragment displacement scales with the worst severity, and rule-based violations fade as the callus stiffens.

## Code structure

Everything lives in a single self-contained `index.html`:

- **`SUPPORTS`** — single source of truth for each device: `protect` / `span` / `note` feed the safety model, while the `draw` config (colors, geometry) drives rendering.
- **`SLIDERS`** — one config array drives slider markup, labels, and state key.
- **Safety model** — `distalMoment`, `healingCapacity`, `supportProtection`, `restrictionLevel`, `evaluateSafety`.
- **Rendering** — the limb is drawn as a chain of rotating `pivot()` frames (shoulder → elbow → wrist); bone outline paths, union/click case drawing, and HiDPI-aware canvas sizing.

## Disclaimer

This project is a simplified teaching tool. It is not validated clinical guidance and should never inform real treatment decisions.