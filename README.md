# Rigid Body Rotation Visualizer

**Live demo:** file:///C:/Users/Dev%20Panchal/Downloads/index.html

An interactive, browser-based tool for visualizing 3D rigid body rotations. It renders two coordinate frames — a **fixed reference frame** (blue, static) and a **rotatable body frame** (orange, attached to a small rigid box) — and lets you drive the body frame's orientation with three real-time sliders (roll, pitch, yaw). The corresponding **3×3 rotation matrix**, its determinant, an orthonormality check (‖RRᵀ − I‖), and the equivalent unit quaternion all update live as you move the sliders.

Built for *Introduction to Robotics*.

## Features

- **Two coordinate frames in one 3D scene**: a static world/fixed frame and a rotating body frame, each with labeled X/Y/Z arrows so the mapping between angle and rotation is visually unambiguous.
- **Three sliders** (−180° to 180°) for roll (φ, about X), pitch (θ, about Y), and yaw (ψ, about Z).
- **Selectable rotation sequence**: intrinsic Z-Y-X (yaw-pitch-roll, the common aerospace/robotics convention), X-Y-Z, or Z-X-Y — lets you see how the *order* of elemental rotations changes the composite matrix, not just the angles.
- **Live rotation matrix readout**, computed from first principles (elementary rotation matrices `Rx`, `Ry`, `Rz` multiplied in the chosen order) — not read out of the graphics engine — so the math and the visualization are two independent checks on each other.
- **Sanity indicators**: determinant (should stay at 1) and Frobenius-norm orthogonality error (should stay ~0), confirming R is a proper rotation (∈ SO(3)) at every configuration.
- **Quaternion readout**, converted from the rotation matrix, for cross-checking against alternative attitude representations.
- **Orbit camera** (drag to rotate view, scroll to zoom, right-drag to pan) so you can inspect the rotation from any angle.
- **Auto-spin toggle** for a hands-free demo sweep, handy for the screen recording.
- **Reset to identity** button.

No build step, no dependencies to install — it's a single `index.html` that pulls Three.js from a CDN.

## Running it

Just open `index.html` in any modern browser (Chrome/Edge/Firefox). Two easy options:

1. **Double-click** `index.html` locally, or
2. Serve it locally (avoids any browser file:// restrictions):
   ```bash
   python3 -m http.server 8000
   # then visit http://localhost:8000
   ```

You can also host it for free with **GitHub Pages** (Settings → Pages → Deploy from branch → `main` / root) and get a shareable link.

## How the math works

Given roll `φ`, pitch `θ`, yaw `ψ`, the elementary rotation matrices are:

```
Rx(φ) = [[1, 0, 0], [0, cosφ, -sinφ], [0, sinφ, cosφ]]
Ry(θ) = [[cosθ, 0, sinθ], [0, 1, 0], [-sinθ, 0, cosθ]]
Rz(ψ) = [[cosψ, -sinψ, 0], [sinψ, cosψ, 0], [0, 0, 1]]
```

For the default intrinsic Z-Y-X sequence (yaw → pitch → roll, applied in the body frame), the composite rotation is:

```
R = Rz(ψ) · Ry(θ) · Rx(φ)
```

This `R` is applied directly to the body frame's Three.js quaternion each frame, and is displayed in the sidebar as a 3×3 grid, so what you see rotating in the scene is provably the same `R` shown in the readout.
