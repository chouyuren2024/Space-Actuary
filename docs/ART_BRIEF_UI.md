# Art Collaboration Brief — Button & Panel Frames

## 1. Objective

Create **button frames** and **panel frames** for the game UI to give the interface a more polished, tactile feel. Typography will be handled separately by the lead programmer—do not draw any text.

Current frames in `image/ui/`:
- `button-frame.png` (32×32): Mechanical console style featuring brushed metal, corner screws, and cyan LED borders; shared across all buttons site-wide.
- `panel-frame.png`, `panel-frame-alert.png`, `panel-frame-gold.png` (16×16): HUD corner brackets in cyan, red (anomaly alerts), and gold (rewards), respectively.

Style Direction: **Space Mission Control Console × Pixel Art**. You may reference real spacecraft instrument panels and NASA Mission Control consoles. Keep the designs refined yet restrained—buttons will have text overlaid on top, and panels will contain dense text and telemetry numbers.

## 2. Most Important Rule: 9-Slice Scaling

These frames are applied using CSS `border-image` (9-slice scaling): the sprite is divided into 9 regions where **the four corners remain unscaled, the four edges are stretched, and the center region is stretched to fill the interior**. Buttons may be stretched very wide and flat, while panels may be stretched very tall and large. Therefore:
1. **Four Corners:** Fixed pixel dimensions (see table below); suitable for intricate details.
2. **Top & Bottom Edges:** Stretched horizontally, so **every vertical column within these strips must be identical**. Patterns may only consist of horizontal lines—no screws, notches, or details that will distort when stretched.
3. **Left & Right Edges:** Stretched vertically, so **every horizontal row within these strips must be identical**.
4. **Center Region:** Stretched both horizontally and vertically; **must be a single, solid, 100% opaque color** (acting as the button or panel fill).
5. The outermost perimeter of the sprite may use fully transparent pixels (to create rounded or chamfered corners), but **no semi-transparent pixels** are allowed.

## 3. Asset List

| Filename | Native Size | Corner Size | Usage |
|---|---|---|---|
| `ui/button-frame.png` | 24×24 | 6 px | Standard buttons (Back, Settings, secondary actions). Use a dark center fill, e.g., `#0b0f25`. |
| `ui/button-frame-primary.png` | 24×24 | 6 px | Primary action buttons (Launch, Start Mission). Must stand out more than standard buttons—e.g., brighter cyan borders or gold accents. |
| `ui/button-frame-danger.png` | 24×24 | 6 px | Destructive actions (Reset Progress); red color scheme. |
| `ui/panel-frame.png` | 16×16 | 4 px | Standard panels; cyan color scheme. Center fill: `#141838`. |
| `ui/panel-frame-alert.png` | 16×16 | 4 px | Anomaly/alert panels; red color scheme with a stronger warning feel. |
| `ui/panel-frame-gold.png` | 16×16 | 4 px | Reward and negotiation tip panels; gold color scheme. |
| `ui/card-frame.png` (Optional) | 16×16 | 4 px | Component cards (currently drawn with a 2 px CSS border). Should be more subtle than standard panels. |
| `ui/card-frame-selected.png` (Optional) | 16×16 | 4 px | Selected component cards; clearly glowing/highlighted. |

- The game renders these sprites at **2x scale** (e.g., a 6 px corner renders at 12 px), so use bold, crisp pixel clusters and avoid 1-pixel noisy dithering.
- Hover, active/pressed, and disabled button states are handled via code (brightening, shifting downward, desaturating)—do not draw separate state sprites.
- Frames **must not contain any text**.
