# Art Brief — Space Actuary

## 1. Art Specifications

| Item | Requirement |
|---|---|
| Style | Pixel art. No gradients, no antialiasing, no blurred edges, and no photorealistic textures. |
| Format | PNG with **transparent background** (RGBA). |
| Dimensions | Final dimensions for each asset are listed below and **must match exact pixel counts**. |
| Delivery Method | Since AI struggles to generate native 32×32 pixel art directly, please draw on an **8x scaled canvas**: each "pixel" must be an 8×8 solid-colored square aligned strictly to the grid (e.g., deliver a 64×64 asset at 512×512). We will downscale them using Nearest Neighbor interpolation; if the blocks are not grid-aligned, the downscaled image will become blurry. |
| Color Count | Maximum of ~16 colors per asset. Use flat color blocks; no gradients. |
| Outline | Add a 1-pixel dark outline (`#070a24`) around the main subject so it stands out clearly against dark blue backgrounds. |
| Lighting | Consistent top-left light source across all assets. |
| Trademarks | No real-world agency logos (e.g., NASA), company trademarks (e.g., SpaceX), national flags, or emblems. |
| Composition | Center the subject with approximately 1–2 pixels of padding on all sides; do not touch the edges. |

### Game Color Palette (Please use these colors primarily)

| Usage | Hex Code |
|---|---|
| Background (Darkest) | `#070a24` |
| Background | `#0b0e2b` |
| Panels | `#141838`, `#1b2050` |
| Lines / Borders | `#262c63`, `#3a4380` |
| Primary Text / Bright White | `#e8ecff` |
| Secondary Slate Blue | `#9aa3d1`, `#6b73a3` |
| Accent Cyan | `#4fd1ff`, `#1f7fb3` |
| Gold (Rewards, Solar Panels, MLI Blankets) | `#ffd166`, `#d18b1f` |
| Red (Warning, Failure) | `#ff5566` |
| Green (Success) | `#57e389` |
| Orange | `#ff9f43` |
| Purple (Plasma, Ion) | `#b388ff` |

Metallic components may use gray tones (e.g., `#c9cfe8`, `#8a93bd`, `#4b5385`), provided the overall tone harmonizes with the palette above.

## 2. Do Not Replace (Code-Dependent Shapes)

The following assets are hardcoded to specific coordinates in the game logic; replacing them will misalign animations or clickable hitboxes:

- `image/rocket/rocket-*.png` (4 rockets): Stage separation animations and exhaust positions are aligned to their current silhouettes.
- `image/rocket/blueprint-sat-*.png` (3 satellite blueprints): Component hotspot buttons are positioned based on the parts drawn in these blueprints.
- `image/rocket/exhaust.png` (Rocket exhaust plume)
- `image/ui/*` (Button frames, panel frames, launchpad, starfield): 9-slice sprites with strict dimension and border rules.
- `image/Stars/*` (Planets)

If you believe any of these require a redesign, please submit a proposal first rather than replacing them directly.

## 3. Asset List (In Priority Order)

### Priority 1: Known Issues

| Filename | Dimensions | Description |
|---|---|---|
| `parts/payload-optical-premium.png` | 64×64 | 1.1 m aperture long-barrel space telescope (reference: WorldView-3): Cylindrical optical tube wrapped in white or gold MLI (Multi-Layer Insulation) blanket, lens hood facing downward, with a star tracker mounted on the side. |

### Priority 2: Satellite Components (29 Assets, 64×64, `image/parts/`)

Each component category has three tiers: **economy**, **standard**, and **premium**. Within the same subsystem, the three tiers must visually progress in size and complexity ("larger and more sophisticated").

| Filename | Real-World Hardware Reference |
|---|---|
| `power-economy.png` | Body-mounted solar cells: Cubic bus exterior covered in dark blue solar cells, no deployable panels. |
| `power-standard.png` | Deployable triple-junction GaAs solar arrays: Two rigid dark blue panels deployed from the spacecraft bus. |
| `power-premium.png` | Roll-Out Solar Array (ROSA): Long, flexible solar wings unrolled like a tape measure. |
| `comm-economy.png` | S-band CubeSat small antenna: Small box with one or two short whip/monopole antennas. |
| `comm-standard.png` | X-band steerable dish antenna: Medium-sized parabolic dish. |
| `comm-premium.png` | Ka-band high-gain antenna: Large parabolic dish, optionally accompanied by a smaller backup dish. |
| `attitude-economy.png` | Magnetorquers: Three mutually perpendicular rods wrapped in copper coils. |
| `attitude-standard.png` | 3 reaction wheels (disc-shaped flywheels) + 1 star tracker (small optical barrel). |
| `attitude-premium.png` | 4 reaction wheels arranged in a pyramid configuration + 1 star tracker. |
| `structure-economy.png` | 6061 aluminum open-frame chassis: Four corner rails with stacked internal PCBs visible (CubeSat style). |
| `structure-standard.png` | Aluminum honeycomb sandwich panels + central thrust cylinder (honeycomb grid texture visible). |
| `structure-premium.png` | 7075 aluminum monolithic thick hull with an outer Whipple shield layer (may include 1–2 micrometeoroid impact marks). |
| `computer-economy.png` | Lightweight COTS computer (GomSpace NanoMind A3200 class): Small green CubeSat PCB with a single black processor chip. |
| `computer-standard.png` | High-capacity computer (EnduroSat OBC class + solid-state recorder): Several stacked PCBs alongside a silver mass-storage unit. |
| `computer-premium.png` | Radiation-hardened computer (BAE RAD750 3U class, dual-redundant): Two thick aluminum-housed chassis side-by-side with heatsink fins and gold connectors. **Note: Each computer tier has distinct trade-offs; "rad-hard" does not mean sleeker/more futuristic, but rather bulkier and more rugged.** |
| `payload-optical-economy.png` | 9 cm aperture CubeSat camera (Planet Labs Dove class: small box with a small lens). |
| `payload-optical-standard.png` | 50 cm aperture multispectral imager (medium optical barrel). |
| `payload-optical-premium.png` | See Priority 1. |
| `payload-transponder-economy.png` | Low-capacity Ku-band transponder: Gold MLI-wrapped payload bay + a single feed horn. |
| `payload-transponder-standard.png` | Multi-beam Ka-band transponder: Payload bay + multiple feed horns. |
| `payload-transponder-premium.png` | High-Throughput Satellite (HTS): Payload bay + two large deployable reflector dishes. |
| `payload-plasma-economy.png` | Multi-needle Langmuir probe: Slender boom with several thin needles at the tip. |
| `payload-plasma-standard.png` | FORMOSAT-5 AIP-class plasma probe: Small box with a circular sensor aperture on the front. |
| `payload-plasma-premium.png` | Plasma probe + GNSS radio occultation antenna (flat patch antenna array). |
| `fuel-hydrazine.png` | Hydrazine: Spherical propellant tank with an orange hazard stripe and a small thruster nozzle. |
| `fuel-af_m315e.png` | Green propellant (AF-M315E): Spherical propellant tank with a green identification stripe. |
| `fuel-biprop.png` | Bipropellant (MMH/NTO): Dual tanks (fuel + oxidizer) + bell-shaped thruster nozzle. |
| `fuel-hall.png` | Hall-effect thruster: Annular channel emitting cyan plasma. |
| `fuel-ion.png` | Gridded ion thruster: Circular perforated grid panel emitting a blue-violet ion beam. |

### Priority 3: Icons (`image/icons/`)

| Filename | Dimensions | Description |
|---|---|---|
| `objective-imaging.png` | 32×32 | High-res imaging mission: Top-down view of city streets (may include a camera focus reticle). |
| `objective-climate.png` | 32×32 | Global climate monitoring: Earth with a thermometer, or polar ice caps. |
| `objective-weather.png` | 32×32 | Typhoon tracking: Top-down view of a swirling typhoon. |
| `objective-comms.png` | 32×32 | Communications broadcast: Ground dish antenna emitting signal waves. |
| `objective-ionosphere.png` | 32×32 | Ionosphere sounding: Colorful aurora bands above Earth's horizon. |
| `event-solar_flare.png` | 32×32 | Solar particle event: Sun erupting with a solar flare. |
| `event-geomagnetic_storm.png` | 32×32 | Geomagnetic storm: Earth surrounded by distorted magnetic field lines. |
| `event-bad_weather.png` | 32×32 | Launch delay (weather): Storm cloud with lightning. |
| `event-small_debris.png` | 32×32 | Micrometeoroids / small debris: Cluster of tiny debris fragments. |
| `event-orbital_congestion.png` | 32×32 | Orbital congestion: Larger space debris with a warning indicator. |
| `event-limited_ground_station.png` | 32×32 | Ground station maintenance: Dish antenna with a wrench. |
| `event-mass_growth.png` | 32×32 | Mass growth: Scale or weight with needle tipping toward the heavy side. |
| `event-budget_cut.png` | 32×32 | Budget cut: Banknote or money bag being cut by scissors. |
| `event-export_control.png` | 32×32 | Export control: Locked crate or shipping container. |
| `event-launch_manifest.png` | 32×32 | Crowded launch manifest: Overbooked calendar. |
| `event-flagship.png` | 32×32 | National flagship program: Medal or pennant (**do not draw any real national flags**). |
| `event-nominal.png` | 32×32 | All nominal: Calm sun or a checkmark. |
| `medal-s_rank.png` | 32×32 | Medal "Master Actuary": Gold star-shaped medal. |
| `medal-frugal.png` | 32×32 | Medal "Shoestring Miracle": Medal with a small coin. |
| `medal-robust.png` | 32×32 | Medal "Bulletproof": Shield-shaped medal. |
| `medal-all_electric.png` | 32×32 | Medal "All-Electric": Medal with a lightning bolt. |
| `medal-fast_service.png` | 32×32 | Medal "Instant Broadcast": Medal with a flame or stopwatch. |
| `medal-sure_launch.png` | 32×32 | Medal "Safe Ascent": Medal with a rocket. |
| `achievement-first_orbit.png` | 32×32 | First successful orbit insertion. |
| `achievement-under_budget.png` | 32×32 | Under budget (cost-saving). |
| `achievement-storm_survivor.png` | 32×32 | Storm survivor (weathered severe space weather). |
| `achievement-all_rounder.png` | 32×32 | All-rounder (balanced spacecraft specs). |
| `achievement-glorious_failure.png` | 32×32 | Glorious failure (mission failed despite great engineering). |
| `achievement-never_give_up.png` | 32×32 | Never give up (perseverance after repeated failures). |
| `achievement-bare_bones.png` | 32×32 | Bare bones (completed using only economy-tier parts). |
| `achievement-top_of_the_line.png` | 32×32 | Spare no expense (built using only premium-tier parts). |
| `achievement-so_close.png` | 32×32 | So close (missed objective by a tiny margin). |
| `achievement-crisis_management.png` | 32×32 | Crisis management (successfully resolved an anomaly). |
| `achievement-flawless_mission.png` | 32×32 | Flawless mission. |
| `achievement-extended_mission.png` | 32×32 | Extended mission (operated beyond design lifespan). |
| `achievement-space_polymath.png` | 32×32 | Space polymath (completed every mission type). |
| `locked.png` | 32×32 | Locked state: Gray padlock. |
| `mascot-orbit.png` | 32×32 | Mascot "ORBIT": Small robot head with a cyan visor and a red antenna light on top (retain these key features). |
| `orbit-leo.png`, `orbit-geo.png` | 48×48 | Low Earth Orbit & Geostationary Orbit: Earth encircled by an orbital ring and a tiny satellite. The GEO ring must be noticeably larger than the LEO ring. |

### Priority 4: Inline Glyphs (16×16, `image/icons/glyph-*.png`)

Displayed inline with text; must be ultra-simple and instantly readable at small sizes, using at most 4–5 colors:
`add` (plus sign), `ban` (prohibition sign), `book` (book / codex), `check` (checkmark), `cost` (coin), `cross` (X mark), `data` (data/file), `dv` (delta-v arrow), `mass` (weight), `power` (lightning bolt), `reserve` (safe / reserve fund), `signal` (signal antenna), `star` (star), `thermal` (thermometer), `tip` (lightbulb), `wait` (hourglass), `warn` (warning triangle).

## 4. Delivery & Acceptance Criteria

1. Deliver PNG files using the exact filenames listed above at 8x resolution, matching the game's folder structure (`parts/`, `icons/`).
2. Include a "sprite sheet / lineup preview" image for each group so we can inspect visual consistency at a glance.
3. Assets will be downscaled and tested in-game against dark backgrounds to verify silhouette clarity, clear visual distinction between the three component tiers, and overall harmony with existing UI/art. Revisions will be requested if issues are found.
