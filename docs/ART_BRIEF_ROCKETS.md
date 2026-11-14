# Art Collaboration Brief — Four Rockets

## 1. How Rockets Are Used In-Game

1. **Rocket Selection Card:** Displayed at approximately 72 px high.
2. **Launch Animation:** The rocket is displayed at a fixed height of 96 px, flying along a gravity-turn arc. The game code **slices** the single rocket sprite to animate stage separations:
   - **Side Booster Separation:** Slices off vertical strips from the left and right sides of the image and animates them falling outward.
   - **Stage 1 / Stage 2 Separation:** Slices off the bottom section (First Stage) and animates it falling downward.
   - **Payload Fairing Separation:** Slices off the top section (Fairing).
3. **Exhaust Plume:** Rendered separately by code, attached to the very bottom edge of the sprite and aligned with the engine nozzles.

Therefore, each rocket only requires a single complete sprite, but **the composition must allow clean separation with simple straight cuts**.

## 2. The Four Rockets to Illustrate (All Real-World Vehicles)

| Filename | Real-World Rocket | Visual Key Points | Aspect Ratio (W:H) |
|---|---|---|---|
| `rocket/rocket-light.png` | Rocket Lab Electron | Slender black carbon-fiber body, two stages; 9 small engines at the bottom of the first stage; relatively short payload fairing at the top. | Approx. 1:2 (e.g., 110×220) |
| `rocket/rocket-pslv.png` | ISRO PSLV-XL | White core stage with **a row of solid strap-on boosters on each side** (visually appearing as one booster on the left and one on the right); bulbous payload fairing at the top, slightly wider than the core body. | Approx. 1:2 (e.g., 160×320) |
| `rocket/rocket-medium.png` | SpaceX Falcon 9 | Slender white body; first stage features 4 folded grid fins near the top, folded landing legs and 9 engines at the bottom; black interstage between Stage 1 and Stage 2; white payload fairing on top. | Approx. 1:2 (e.g., 156×312) |
| `rocket/rocket-heavy.png` | SpaceX Falcon Heavy | **Three Falcon 9 first-stage cores side-by-side:** taller center core topped with the second stage and payload fairing; two side boosters each capped with a conical nose cone. | Nearly 1:1 (e.g., 260×270) |

- **No Text, Trademarks, or Flags:** Do not include SpaceX, ISRO, or Rocket Lab logos/lettering, or any national flags. Replace any real-world markings with plain/blank surfaces.
- Do not draw exhaust plumes, smoke, or launchpads; use a transparent background.
- Upright and centered, with **the very bottom tip of the engine nozzles aligned flush with the bottom edge of the image**.

## 3. Composition Requirements for Stage Separation Animations

1. **Stage 1 / Stage 2 Seam:** There must be a clear horizontal dividing line between the first and second stages (via interstage color contrast or a panel seam). Everything below this line belongs to Stage 1; everything above belongs to Stage 2 + Fairing.
2. **Fairing Seam:** There must be a clear horizontal dividing line between the base of the payload fairing and the second stage.
3. **Side Boosters (PSLV, Falcon Heavy):** The left and right boosters must each form clean vertical columns, separated from the center core by a distinct vertical seam (or a tiny transparent gap). Left and right sides must be symmetrical.
4. **Falcon Heavy Three Nozzle Clusters:** Each of the three cores has an engine cluster at the bottom; all three clusters must be equal in width, evenly spaced, and symmetrical.
5. The seams simply need to be "clearly visible and straight"—they do not need to follow fixed proportions. When submitting the files, please **note the coordinates/percentages of each seam** (e.g., "Stage 1/2 seam is at 64% height from the bottom" or "Left booster occupies 33% of image width"). If omitted, we can measure them ourselves.
