# Art Asset Checklist — Space Actuary

This is a working checklist to track which visual elements are currently placeholder color blocks/emojis and which have been replaced with final art assets. Drop completed/generated files into their corresponding folders.

## Folder Structure & Naming Rules
image/
Stars/      Planets (existing; Saturn.png completed)
rocket/     Rocket vehicles, explosion effects
effects/    Exhaust plumes, clouds
parts/      Satellite component icons (5 categories × 3 tiers)
icons/      Event icons, achievement icons
ui/         Button frames, panel frames, title logo
mascot/     ORBIT mascot
All filenames must use **lowercase letters + underscores/hyphens**, matching internal game IDs wherever possible (see the "Filename" column below) to allow seamless integration without manual mapping or naming mismatches.

## Art Asset Sourcing Options

- **Procedural Code Generation:** Ideal for geometric shapes (planets, UI button/panel frames)—zero cost, controllable color palettes, and cheap to iterate. All 8 planets were generated this way.
- **[Kenney.nl](https://kenney.nl/):** Free CC0 ready-made asset packs, downloadable without registration and cleared for commercial use. Relevant packs include [Planets](https://kenney.nl/assets/planets), [Simple Space](https://kenney.nl/assets/simple-space) (rocket/spacecraft references), and [UI Pack - Pixel Adventure](https://kenney.nl/assets/ui-pack-pixel-adventure) (pixel art buttons/panels, though fantasy-themed and may require recoloring). Great for grab-and-go assets without waiting for generation.
- **[Retro Diffusion](https://retrodiffusion.ai/) + PixelLab:** Specialized pixel-art AI generation tools (free trial credits) with grid-aligned outputs and clean color palettes. Ideal for organic shapes or characters (such as the ORBIT mascot) that cannot be easily drawn via code formulas. Requires account registration.

---

## P0 (Highest Priority: Core Visuals, Most Noticeable If Missing)

| Item | Filename | Recommended Size | Notes |
|---|---|---|---|
| ~~Game Title Logo~~ ✅ | — | — | Completed: Uses Traditional Chinese pixel font "Cubic 11" (俐方體11號) at 44 px with pixel drop shadow; no image file required, supports both Chinese and English. |
| ~~Button Frame~~ ✅ | `ui/button-frame.png` | 32×32 | Completed: Procedurally generated mechanical console style (brushed metal + corner screws + cyan LED border), integrated into `.btn` via 9-slice, shared site-wide. |
| ~~Light Rocket~~ ✅ | `rocket/rocket-light.png` | — | Completed: Inspired by Rocket Lab Electron (small black carbon-fiber rocket). |
| ~~Medium Rocket~~ ✅ | `rocket/rocket-medium.png` | — | Completed: Inspired by SpaceX Falcon 9 (white fuselage + grid fins). |
| ~~Heavy Rocket~~ ✅ | `rocket/rocket-heavy.png` | — | Completed: Inspired by SpaceX Falcon Heavy (center core + twin side boosters). |
| ~~Satellite Blueprints ×3~~ ✅ | `rocket/blueprint-sat-optical.png`<br>`rocket/blueprint-sat-transponder.png`<br>`rocket/blueprint-sat-plasma.png` | 320×320 | Completed: Satellite schematics generated via `tools/sat-art.py`, replacing original rocket schematics. Component hotspots sit at realistic positions; distinct silhouettes for each payload type (nadir telescope / transponder bay + reflectors / ram-facing plasma probe + occultation antenna). |
| ~~PSLV Rocket~~ ✅ | `rocket/rocket-pslv.png` | 160×320 | Completed: Generated via `tools/sat-art.py`, inspired by ISRO PSLV-XL (core stage + side solid strap-on boosters). |
| ~~Plasma Instruments ×3~~ ✅ | `parts/payload-plasma-economy.png`<br>`parts/payload-plasma-standard.png`<br>`parts/payload-plasma-premium.png` | 64×64 | Completed: Generated via `tools/sat-art.py` (multi-needle Langmuir probe / FORMOSAT-5 AIP-class plasma probe / AIP + GNSS radio occultation antenna). |
| ~~Power Systems ×3~~ ✅ | `parts/power-economy.png`<br>`parts/power-standard.png`<br>`parts/power-premium.png` | — | Completed: Procedurally generated (body-mounted cells / deployable solar arrays / ROSA roll-out solar arrays). v2 replaced premium RTG with ROSA since Earth-orbiting satellites use solar power and RTGs only output ~100 W. |
| ~~Communications Systems ×3~~ ✅ | `parts/comm-economy.png`<br>`parts/comm-standard.png`<br>`parts/comm-premium.png` | — | Completed: Procedurally generated (S-band small antenna / X-band dish / Ka-band high-gain dual dish). |
| ~~Optical Payloads ×3~~ ✅ | `parts/payload-optical-economy.png`<br>`parts/payload-optical-standard.png`<br>`parts/payload-optical-premium.png` | — | Completed (renamed from `science-*.png` in v2): CubeSat camera / multispectral imager / large-aperture telescope. Premium currently still uses a JWST-style gold hexagonal mirror, which doesn't match WorldView-3's actual look; can be redrawn later as a long-barrel telescope. |
| ~~Comms Payloads (Transponders) ×3~~ ✅ | `parts/payload-transponder-economy.png`<br>`parts/payload-transponder-standard.png`<br>`parts/payload-transponder-premium.png` | 64×64 | Completed: Procedurally generated (gold MLI-wrapped payload bay + feed horns; premium adds two large reflectors) for communications missions. |
| ~~Propulsion Systems ×5~~ ✅ | `parts/fuel-hydrazine.png`<br>`parts/fuel-af_m315e.png`<br>`parts/fuel-biprop.png`<br>`parts/fuel-ion.png`<br>`parts/fuel-hall.png` | 64×64 | Completed: Procedurally generated (Hydrazine = spherical tank with orange hazard stripe / AF-M315E = green spherical tank / Bipropellant = dual fuel + oxidizer spherical tanks + bell nozzle / Ion = gridded face + blue-violet plasma beam / Hall = annular channel + cyan plasma glow). v2 replaced hydrogen peroxide with bipropellant. |
| ~~Attitude Control ×3~~ ✅ | `parts/attitude-economy.png`<br>`parts/attitude-standard.png`<br>`parts/attitude-premium.png` | 64×64 | Completed: Procedurally generated (three copper-coiled magnetorquers + magnetic field lines / 3 reaction wheels + star tracker / 4 reaction wheels + star tracker). |
| ~~Structure & Shielding ×3~~ ✅ | `parts/structure-economy.png`<br>`parts/structure-standard.png`<br>`parts/structure-premium.png` | 64×64 | Completed: Procedurally generated, updated to realistic satellite structures—6061 open-frame chassis (with visible internal PCBs) / aluminum honeycomb sandwich panel cross-section / 7075 monolithic hull + Whipple sacrificial shield with micrometeoroid impact sparks. |
| ~~Earth~~ | ~~`Stars/earth.png`~~ | — | **Not needed**—rockets launch from Earth's surface, so placing another Earth in the space background makes no logical sense; removed from list. |

The remaining 8 planets of the solar system (Mercury, Venus, Mars, Jupiter, Saturn, Uranus, Neptune, Pluto) are all completed ✅, generated directly as pixel art via Python/Pillow scripts (circular mask + shaded bands + noise/stripes). Neither hand-drawn nor AI-generated, their palettes and styles can be tweaked and regenerated anytime at minimal cost.

---

## P1 (High Priority: Noticeably Elevates Overall Polish Upon Completion)

| Item | Filename | Recommended Size | Notes |
|---|---|---|---|
| ~~Panel Frames~~ ✅ | `ui/panel-frame.png`, `panel-frame-alert.png`, `panel-frame-gold.png` | 16×16 9-slice (displayed at 2x) | Completed: Generated via `tools/ui-frames.py` matching the button frame palette, featuring HUD corner brackets; red variant used for anomaly alerts. |
| ~~ORBIT Mascot~~ ✅ | `icons/mascot-orbit.png` | 32×32 (displayed at 64×64) | Completed: Robot head with cyan visor and red antenna light. |
| ~~Site-Wide Starfield Background~~ ✅ | `ui/starfield.png` | 256×256 tileable | Completed: Pixel star dots + occasional bright stars with cross glints. |
| ~~Explosion Effects~~ ✅ | `rocket/explosion-1.png`<br>`rocket/explosion-2.png`<br>`rocket/explosion-3.png` | 64×64 | Completed: Procedurally generated 3-frame sequence (bright core → fireball → smoke & debris), played on launch/ascent failure. |
| ~~Event Icons ×12~~ ✅ | `icons/events/solar_flare.png`<br>`geomagnetic_storm.png`<br>`bad_weather.png`<br>`small_debris.png`<br>`orbital_congestion.png`<br>`limited_ground_station.png`<br>`mass_growth.png`<br>`budget_cut.png`<br>`export_control.png`<br>`launch_manifest.png`<br>`flagship.png`<br>`nominal.png` | 48×48 | Completed: Saved as `icons/event-*.png`, generated via `tools/pixel-icons.py`. |
| ~~Achievement Icons ×13 + Locked~~ ✅ | `icons/achievement-*.png`, `icons/locked.png` | 32×32 | Completed: Generated via `tools/pixel-icons.py`. |
| ~~Rocket Exhaust Plume~~ ✅ | `rocket/exhaust.png` | 3 frames × 8×14 (displayed at 3x) | Completed: 3-frame pixel flame animation generated via `tools/pixel-icons.py`; heavy rocket aligns three plumes to its three cores. |
| ~~Other Pixel Icons~~ ✅ | `icons/objective-*.png`, `medal-*.png`, `orbit-leo/geo.png`, `alert.png`, `glyph-*.png` (16×16) | 32×32 / 16×16 | Completed: Replaced all emojis across the game (55 icons total). |

---

## P2 (Nice-to-Have: Extra Polish When Bandwidth Allows)

| Item | Filename | Recommended Size | Notes |
|---|---|---|---|
| Clouds | `effects/cloud.png` | 96×36 | Current CSS clouds are passable; a hand-drawn sprite could be tiled/placed across multiple positions. |
| ~~Launchpad~~ ✅ | `ui/launchpad.png` | 360×90 (displayed at 2x) | Completed: Procedurally generated via `tools/launchpad-art.py`—red service tower with swing arms, launch mount and flame trench, lightning towers, bunker, and distant mountains; foreground uses CSS grass, beach, and ocean (since real launchpads are almost always coastal), with the deck aligned by code to the rocket base. |
| Stars | `effects/star.png` | 8×8 or smaller | Current CSS light dots are sufficient unless more stylized stars are desired. |
| Button Hover Frame | `ui/button-frame-hover.png` | Same as button frame | Visual state change on mouse hover. |
| Meter / Progress Bar Frame | `ui/progress-bar-frame.png` | Layout-dependent | Decorative frame for budget and mass bars. |

---

## Progress Tracker

- [x] 8 Solar System planets (Mercury/Venus/Mars/Jupiter/Saturn/Uranus/Neptune/Pluto, procedurally generated)
- [x] Button frame (mechanical console style, procedurally generated)
- [x] 3 rocket tiers (inspired by Electron / Falcon 9 / Falcon Heavy, procedurally generated)
- [x] Real-world hardware descriptions for rockets + 4 subsystems (Power / Comms / Science / Structure), integrated into the component selection screen
- [x] 4 subsystems × 3 tiers = 12 component icons (procedurally generated, integrated into the selection screen)
- [x] 3 rocket schematics (procedurally generated pure-line technical drawings); updated selection screen flow to "Select Rocket → Blueprint Appears Below → Click Component Hotspots to Open Menu"
- [x] 5 propellant/thruster icons (procedurally generated, integrated into the selection screen)
- [x] v2 realistic hardware additions: ROSA solar arrays, bipropellant propulsion, communications transponders ×3 (procedurally generated)
- [ ] Redraw premium optical payload as a long-barrel telescope (current JWST hexagonal mirror doesn't match WorldView-3)
- [x] 3-frame explosion effect (procedurally generated); Earth during operations phase is rendered in real time via HTML5 Canvas (North Pole top-down view, accurate scale, real Natural Earth coastlines stored in `js/earthmap.js`), requiring no image file
- [x] Title (pixel font), panel frames, ORBIT mascot, event icons ×12, achievement icons ×13, mission/medal/orbit/inline glyph icons
- [x] Rocket exhaust plume (3-frame pixel flame loop)
- [x] 3 BGM tracks (original compositions by You-Yan Hu): `audio/bgm/space.mp3` (space ambience: Home, Results, Operations), `lab.mp3` (laboratory: Briefing, Design), `launch.mp3` (Launch). Converted from raw WAV to 160 kbps MP3 (26 MB → 3 MB); each track spans 16 bars + reverb tail that blends into the next loop at the end of bar 16 for seamless looping
- [ ] P2 items (clouds, ground decorations, etc.)
