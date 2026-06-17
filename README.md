# Zero Click Bs As — Code Rain over the Obelisk

A Three.js code-rain effect inspired by [Profound's Zero Click](https://www.tryprofound.com/zeroclick/sf), reimagined for Buenos Aires. Falling glyphs — each one Profound's icon — are masked over an SVG silhouette of the Obelisco, with a firefly-like twinkle tracing its edges. The hero text reveals with a left-to-right blur sweep, followed by an animated orange conic border around the date.

https://github.com/user-attachments/assets/a12eb3f8-4eec-4af3-b724-30b805f11797

## What it does

- **Code rain** — a fragment shader renders a grid of cells; each cell draws Profound's icon, modulated by a falling-drop animation with a soft grey→white→grey profile (no harsh Matrix head/tail).
- **Two layers in one** — the rain falls inside the obelisk's filled body, while the contour cells twinkle on and off like fireflies. A per-cell edge-detection pass decides which behavior applies.
- **Masking** — the obelisk SVG is rasterized to a texture and used as a mask, so the whole effect lives inside its silhouette.
- **Hero reveal** — "Zero Click Bs As" appears via an animated `mask-position` sweep with a blur that focuses as it reveals, plus a sliding cursor that blinks when it lands.
- **Animated border** — the `26` box gets an orange conic-gradient border that rotates clockwise, built with the `@property` API for smooth angle interpolation. It fades in only after the text animation completes.
- **Live controls** — a small panel (toggle with the `H` key) to tweak column density, fall speed, drop density, and base glow in real time.

## Tech

- [Three.js](https://threejs.org/) (r160) via CDN + import map — no build step
- Custom GLSL fragment shader for the rain + twinkle + edge detection
- SVG → `CanvasTexture` rasterization for both the mask and the glyph
- Pure CSS for the text reveal and rotating border (`@property`, `mask-composite`, `conic-gradient`)

## Run it

No dependencies, no build. Clone and serve the folder:

```bash
git clone https://github.com/Seque/zero-click-bsas.git
cd zero-click-bsas

# Python
python3 -m http.server 8000

# or Node
npx serve .
```

Then visit `http://localhost:8000`. Opening the file directly with `file://` also works in most browsers, but a local server is recommended for the ES module import map.

## Controls

| Control   | What it does                                              |
|-----------|----------------------------------------------------------|
| Columns   | Grid density — more columns = smaller, denser glyphs      |
| Speed     | How fast the rain falls                                   |
| Density   | Number of drops per column                                |
| Base glow | Ambient brightness of the silhouette between drops        |
| `H` key   | Show / hide the control panel                             |

## Structure

Everything lives in a single `index.html`:

- **`<style>`** — layout, the hero text reveal animation, and the rotating border
- **Embedded SVGs** — the obelisk (mask) and Profound's icon (glyph) as inline strings
- **`<script type="module">`** — Three.js scene, the shader, and the control bindings

## Tuning the shader

A few values worth knowing if you want to adapt it:

- `dropCount` / `dropLen` — drop spacing and length per column
- `edge = smoothstep(0.25, 0.9, edge)` — width of the twinkling edge band
- `blink = sin(uTime * 4.5 + ...)` — twinkle rhythm
- Drop brightness profile: `0.45 + 0.55 * sin(t * PI)` — the grey→white→grey gradient

## Credits

Inspired by [Profound's Zero Click](https://www.tryprofound.com/zeroclick/sf). The Profound icon is used here as a creative homage; all brand assets belong to their respective owners.

Built by [Ezequiel](https://ezequielm.vercel.app) ([@Seque](https://github.com/Seque))

## License

MIT — see [LICENSE](./LICENSE). Note this covers the code only, not any third-party brand assets referenced above.
