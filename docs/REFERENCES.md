# References

This document records the external data sources cited throughout the project, primarily the space history trivia paired with each achievement on the "Achievements" showcase screen.

## The Challenge Itself

- [NASA Space Apps Challenge 2026 — Space Mission Design Game](https://www.spaceappschallenge.org/2026/challenges/space-mission-design-game/) — Official challenge page; serves as the basis for the project's requirements and difficulty calibration.

## Real-World Data in the Mission Model

Every mechanic in the game is grounded in real space engineering principles. The figures below are hardcoded in `js/data.js`, with parenthetical notes explaining their in-game usage.

### Official NASA Resources Used (Compiled by You-Ren Zhou)
- **Design and Development of Science Missions** [Official Resource](https://www.nasa.gov/wp-content/uploads/2025/04/250323-design-and-develop-science-mission.pdf?emrc=a454cf): Outlines the systems engineering methodology used to develop scientific space missions, explaining how missions progress from initial science requirements through definition, design, verification, and implementation. Used as a development guide for this project.
- **Mission Design Lab** [Official Resource](https://etd.gsfc.nasa.gov/capabilities/integrated-design-center/mission-design-lab/): NASA's Mission Design Lab is a collaborative environment where engineers and scientists rapidly develop and evaluate space mission concepts, turning initial ideas into technically vetted mission designs. Used as a design guide during this hackathon.
- **NASA Mission Visualization** [Official Resource](https://nasa.github.io/mission-viz/): Referenced for visualization approaches.
- **Scope It Out! (Telescope Learning Game)** [Official Resource](https://science.nasa.gov/mission/webb/game-scope-it-out/): Referenced for educational game design logic.
- **CubeSat 101: Basic Concepts and Processes for First-Time CubeSat Developers** [Official Resource](https://www3.nasa.gov/sites/default/files/atoms/files/nasa_csli_cubesat_101_508.pdf): Referenced for satellite subsystem design.
- **Canadian Space Agency (CSA) Digital Games and Activities** [Official Resource](https://www.asc-csa.gc.ca/eng/youth-educators/digital-games-and-activities.asp): Referenced for game design inspiration.

### Launch Vehicles: Cost, Payload Capacity, & Flight Heritage

- **Falcon 9:** [Falcon 9 — Wikipedia](https://en.wikipedia.org/wiki/Falcon_9) — Reusable configuration: 17,500 kg to LEO, 5,500 kg to GTO; ~2026 commercial launch price ~$74M; 690 launches, 687 successes (two failures: CRS-7 and Starlink Group 9-3; one partial failure: CRS-1).
- **Falcon Heavy:** [Falcon Heavy — Wikipedia](https://en.wikipedia.org/wiki/Falcon_Heavy) — 8 t to GTO with three-core recovery, ~57 t to LEO with side-booster recovery; reusable launch price $97M; 13 launches, 100% success rate.
- **Electron:** [Rocket Lab Electron — Wikipedia](https://en.wikipedia.org/wiki/Rocket_Lab_Electron) — 200 kg to SSO (all in-game LEO missions are Earth observation, so SSO capacity is used), ~$7.5M per launch; 97 total launches (including 9 suborbital HASTE flights) with 4 failures. The game counts only orbital launches: 88 launches, 84 successes.
- **2025 Flight Record Verification:** [Rocket Lab ends 2025 with 21 launches — Barchart](https://www.barchart.com/story/news/36733390/rocket-lab-successfully-launches-for-iqps-ends-2025-with-21-launches-and-100-mission-success)
- **Reliability Estimation:** Laplace's rule of succession ($(s + 1)/(n + 2)$), a standard statistical method used to demonstrate why low-sample rockets cannot be treated as 100% reliable.

> Falcon 9 and Falcon Heavy have slightly lower SSO capacities than LEO capacities, but both still vastly exceed any satellite mass in the game, so LEO figures are used directly.

### Orbital Mechanics

- **GTO → GEO Circularization Δv:** 1,831 m/s (from Cape Canaveral at 28.5° inclination): [MIT 16.512 Rocket Propulsion, Lecture 34](https://ocw.mit.edu/courses/16-512-rocket-propulsion-fall-2005/768501e0e6a9ea6ef794c2866ea1bd63_lecture_34.pdf)
- **GEO Station-Keeping:** ~50 m/s/year North-South, ~2–5 m/s/year East-West: [Orbital station-keeping — Wikipedia](https://en.wikipedia.org/wiki/Orbital_station-keeping) (in-game: 15 years × 50 m/s = 0.75 km/s).
- **All-Electric GEO Orbit Raising (~6 months):** [Eutelsat 115 West B — Wikipedia](https://en.wikipedia.org/wiki/Eutelsat_115_West_B) (Boeing 702SP, dual-stacked with ABS-3A on a single Falcon 9 in 2015).
- **Low-Thrust Spiral Transfer Δv Penalty:** Multiple studies note higher Δv requirements compared to impulsive transfers (e.g., [Optimised GTO-GEO transfer using low-thrust propulsion — Strathclyde](https://pureportal.strath.ac.uk/en/publications/optimised-gto-geo-transfer-using-low-thrust-propulsion/)). **The game uses a 1.3× multiplier** as an estimate; actual values vary by trajectory optimization.
- **LEO 500 km Sunlight Factor:** ~95-minute orbital period, ~35-minute eclipse; average available solar array power is ~62% of rated output (e.g., 340.5 W → 212 W): [EPS sizing tutorial — Valispace](https://www.valispace.com/wp-content/uploads/2018/12/EPS-sizing-tutorial-1.pdf)
- **LEO Ground Contact Windows:** ~3–4 passes per day over a single ground station, ~10 minutes per pass: [Ground station — Orbital Radar](https://orbitalradar.com/glossary/ground-station)
- **GEO Earth Coverage:** Sees ~42% of Earth's surface, up to ±81° latitude: [Geostationary orbit — Wikipedia](https://en.wikipedia.org/wiki/Geostationary_orbit)
- **Free-Space Path Loss (65× distance → ~4,200× weaker signal):** $(35,786 \div 550)^2 \approx 4,233$ (~36 dB), calculated directly via the inverse-square law.
- **FCC 5-Year Deorbit Rule (Adopted September 2022):** [FCC Adopts 5-Year Rule for Deorbiting Satellites — Via Satellite](https://www.satellitetoday.com/government-military/2022/09/30/fcc-adopts-5-year-rule-for-deorbiting-satellites/)

### Propulsion Systems (Specific Impulse)

- **Hydrazine (~220 s):** [A Survey of Hydrazine Monopropellant Thrusters — AIAA](https://arc.aiaa.org/doi/10.2514/6.2026-3090) (steady-state ~215–235 s).
- **AF-M315E (~245 s, ~12% higher than hydrazine):** [Green Propellant Infusion Mission (GPIM) Overview — NASA](https://www.nasa.gov/missions/tech-demonstration/green-propellant-infusion-mission-gpim-overview/)
- **MMH/NTO Bipropellant (~320 s):** [Liquid apogee engine — Wikipedia](https://en.wikipedia.org/wiki/Liquid_apogee_engine)
- **SPT-100 Hall-Effect Thruster (1,350 W, 1,600 s $I_{sp}$, ~83 mN thrust):** [SPT-100 — Wikipedia](https://en.wikipedia.org/wiki/SPT-100)
- **NSTAR Gridded Ion Thruster (~1,900–3,100 s; set to 3,100 s):** [NSTAR — Astronautix](http://www.astronautix.com/n/nstar.html); [Deep Space 1 — Wikipedia](https://en.wikipedia.org/wiki/Deep_Space_1)

> Propulsion dry masses and the 2,300 W power draw (NSTAR max power) are conceptual design-level estimates.

### Payloads & Subsystems

- **Planet Dove:** ~9 cm aperture, 4 bands (RGB + NIR), 3.7–4.2 m GSD: [PlanetScope — Planet Docs](https://docs.planet.com/data/imagery/planetscope/)
- **WorldView-3:** 1.1 m aperture, 29 bands, 31 cm panchromatic resolution, 617 km altitude, 2,800 kg: [WorldView-3 — eoPortal](https://eoportal.org/satellite-missions/worldview-3)
- **Diffraction Limit Verification:** $1.22 \times 550\text{ nm} \times 617\text{ km} \div 1.1\text{ m} \approx 0.38\text{ m}$, matching WorldView-3's actual 0.31 m order of magnitude; 9 cm at 475 km $\approx 3.5\text{ m}$, matching Dove.
- **GOES-R ABI:** 16 bands, 0.5–2 km resolution, full-disk imagery every 10 minutes: [ABI — NOAA GOES-R](https://drupal-prod.goesr.woc.noaa.gov/spacesegment/abi.html)
- **WorldView X-Band Downlink (800 Mbps):** [WorldView-1 — eoPortal](https://www.eoportal.org/satellite-missions/worldview-1)
- **CubeSat S-Band Downlink (~4–14 Mbps):** [ISIS High Data Rate S-band Transmitter](https://www.cubesatshop.com/product/isis-txs-s-band-transmitter/)
- **SDO (Solar Dynamics Observatory):** Uses an inclined geosynchronous orbit for continuous downlink at 1.4 TB/day via Ka-band at ~130 Mbps: [SDO — NASA NTRS](https://ntrs.nasa.gov/api/citations/20090027804/downloads/20090027804.pdf)
- **ROSA (Roll-Out Solar Array):** 3× the specific power of traditional rigid solar arrays; iROSA delivers 20+ kW per wing: [ROSA — Redwire](https://rdw.com/product-archive/power/rosa/)
- **Vanguard 1 (Body-mounted solar cells, 1958):** [Vanguard 1 — Wikipedia](https://en.wikipedia.org/wiki/Vanguard_1)
- **ViaSat-1 (~140 Gbps):** Publicly available data (representative High-Throughput Satellite).
- **Whipple Shield:** [Whipple shield — Wikipedia](https://en.wikipedia.org/wiki/Whipple_shield)

### Reliability & Engineering Margins

- **CubeSat Failure Statistics:** ~25% dead-on-arrival upon orbit insertion, plus another 12% early mortality (Swartwout dataset): [The Space Review](https://thespacereview.com/article/3047/1)
- **Mass Margins:** > 30% in conceptual design phase, 12–30% at "Layout" phase: AIAA S-120 / R-020A, summarized in [NASA NTRS](https://ntrs.nasa.gov/api/citations/20120013284/downloads/20120013284.pdf)

> Component-tier survival rates (5-year LEO survival: COTS 90% / Space-grade 97% / Redundant 99.5%), 99% deployment reliability, 1.5× GEO radiation multiplier, and structural shielding multipliers are **game estimates**: directionally and quantitatively consistent with engineering intuition (COTS parts are most vulnerable in GEO; redundant hardware is most reliable), without tying to a single citation. Component costs ($M) are likewise order-of-magnitude estimates calibrated to present meaningful trade-offs.

### Operations Phase: Reserves & Anomaly Response

- **NASA Joint Cost and Schedule Confidence Level (JCL) 70% Policy & Unallocated Future Expenses (UFE):** [GAO-16-620](https://www.gao.gov/assets/680/678704.pdf); [SPD-19 — NASA](https://discovery.larc.nasa.gov/PDF_FILES/SPD-19_PI-ledmissions_70JCL.pdf)
- **Collision Probability Threshold ($1/10,000$):** Industry standard maneuver threshold (NASA CARA, ISS red-line threshold): [NASA NTRS 20180007756](https://ntrs.nasa.gov/api/citations/20180007756/downloads/20180007756.pdf)
- **Juno 2016 Helium Check Valve Anomaly:** Decision to remain in a 53-day science orbit: [NASA's Juno to Remain in Current Orbit — NASA JPL](https://www.jpl.nasa.gov/news/nasas-juno-to-remain-in-current-orbit-at-jupiter/)
- **Landsat 7 Scan Line Corrector (SLC) Failure (2003):** Unrecoverable hardware fault causing ~22% data loss per scene while continuing productive science for nearly 20 years: [Landsat 7 — USGS](https://www.usgs.gov/landsat-missions/landsat-7)
- **Historical Analogs:** Kepler K2 mission (2014–2018, balancing solar radiation pressure after losing a second reaction wheel in 2013), Hubble's transition to single-gyro mode (2024), Opportunity rover (designed for 90 sols, operated until 2018), and the 2009 Iridium 33 / Kosmos 2251 collision: widely documented space history.

> The 60% recoverable failure ratio, response option costs/effects, operational event frequencies, and $4M/year mission extension cost are **game estimates**.

### Real-World Event Precedents

- **February 2022 Geomagnetic Storm:** Drag increase caused 38 Starlink satellites to re-enter: [Starlink to lose about 40 satellites — Light Reading](https://www.lightreading.com/satellite/starlink-to-lose-about-40-satellites-in-wake-of-geomagnetic-storm/d/d-id/775200)
- **1993 Perseid Meteor Shower Anomaly:** Suspected micrometeoroid impact caused Olympus-1 to lose attitude control: [Shooting Stars Can Shoot Down Satellites — IEEE Spectrum](https://spectrum.ieee.org/shooting-stars-can-shoot-down-satellites)
- **2016 Sentinel-1A Impact (700 km):** Solar array struck by a < 5 mm particle creating a ~40 cm damaged zone, causing a minor power drop without interrupting operations: [Copernicus Sentinel-1A satellite hit by space particle — ESA](https://www.esa.int/Applications/Observing_the_Earth/Copernicus/Sentinel-1/Copernicus_Sentinel-1A_satellite_hit_by_space_particle)
- **Space Debris Size Regimes:** Objects > 10 cm are tracked by ground radar for avoidance maneuvers; objects < 1 cm cannot be tracked and require shielding (the basis for distinguishing "Orbital Congestion" vs. "Micrometeoroids / Small Debris"): public documentation by the ESA Space Debris Office.
- **Kinetic Energy Calculation:** A 1 cm aluminum sphere (~1.4 g) impacting at 10 km/s carries $\frac{1}{2}mv^2 \approx 70\text{ kJ}$, equivalent to a 250 kg object moving at 85 km/h (direct calculation).
- Other referenced events and failure cases (Apollo 12 lightning strike, ADEOS-II, CRS-7, AsiaSat 3, Galileo high-gain antenna) are widely documented spaceflight history.

### Space Weather (Compiled by Yan-Yi Li)

- **NOAA Space Weather Scales:** Per 11-year solar cycle, R3 (X1 flares) occur ~175 times, R4 (X10) ~8 times, R5 (X20) < 1 time; G5 extreme geomagnetic storms occur ~4 times per cycle: [NOAA Space Weather Scales — SWPC](https://www.swpc.noaa.gov/noaa-scales-explanation)
- **May 2024 G5 Geomagnetic Storm:** [“Extreme” G5 geomagnetic storm reaches Earth — CBS News](https://www.cbsnews.com/amp/texas/news/severe-geomagnetic-storm-watch-issued-unusual-solar-event)

> In-game "Solar Storm Alert" frequencies (~0.09/yr in LEO, ~0.07/yr in GEO) are lower than the real-world G5 average (~0.36/yr). This pacing adjustment filters down to events severe enough to require active operator intervention.

### Taiwan Space Missions (Compiled by Yan-Yi Li)

- **FORMOSAT-5 Remote Sensing Instrument (RSI):** 450 mm aperture, 720 km SSO, 2 m panchromatic / 4 m multispectral resolution, 24 km swath, launched on Falcon 9 in 2017: [FORMOSAT-5 — CATALYST](https://catalyst.earth/catalyst-system-files/professional-help/references/gdb_r/Formosat-5.html); [RSI (FORMOSAT-5) — WMO OSCAR](https://space.oscar.wmo.int/instruments/view/rsi_formosat_5)
- **Diffraction Limit Verification:** $1.22 \times 550\text{ nm} \times 720\text{ km} \div 0.45\text{ m} \approx 1.1\text{ m}$; actual 2 m resolution is detector-pixel-limited, consistent with the theoretical diffraction floor.
- **FORMOSAT-7 (COSMIC-2) GNSS Radio Occultation:** Measures refracted navigation signals passing through Earth's atmosphere to invert vertical profiles of temperature, pressure, and humidity unaffected by cloud cover, feeding global numerical weather prediction models: [TASA](https://www.tasa.org.tw/en-US/space-knowledge/b9a512b9-deac-416b-a114-4ed87974209a)

### Structural Materials (Compiled by Yan-Yi Li)

- **6061-T6 vs. 7075-T6 Aluminum:** Yield strength ~276 MPa vs. ~503 MPa (~1.8× stronger): standard aerospace material properties.
- **Trade-offs among CubeSat Open Frames, Aluminum Honeycomb Sandwich Panels, and Monolithic Load-Bearing Structures:** Based on structural comparison tables compiled by Yan-Yi Li.

### Attitude Control (Compiled by Yong-Wei Liang)

- **Dipole Geomagnetic Field Scaling ($\propto 1/r^3$):** $(42,157\text{ km} \div 6,921\text{ km})^3 \approx 226$ (direct calculation).
- **NASA FASTSAT-HSV01 (~180 kg):** Controlled exclusively using three orthogonal magnetorquers: *Design and Integration of an All-Magnetic Attitude Control System for FASTSAT-HSV01's Multiple Pointing Objectives* (provided by Yong-Wei Liang).
- **James Webb Space Telescope Reaction Wheels:** 6 reaction wheels, each delivering 68 N·m·s angular momentum at 6,000 rpm: [The Design, Verification, and Performance of the James Webb Space Telescope](https://ntrs.nasa.gov/citations/20230009106)
- **Kepler Space Telescope:** 4 reaction wheels (3 required for fine pointing); transitioned to K2 mission after losing a second wheel in 2013 (widely documented).
- **Hubble June 2021 Payload Computer Anomaly:** Entered safe mode for ~1 month before switching to redundant backup hardware (NASA press releases).
- In-game pointing accuracies (2° for magnetorquers, 0.02° for reaction wheels) and mission pointing requirements are **game estimates**, scaled realistically to match SmallSat magnetic control (a few degrees) versus star tracker + reaction wheel precision (hundredths of a degree).

### Thermal Control (Compiled by Yong-Wei Liang)

- **Radiative Heat Rejection:** Stefan–Boltzmann law ($\varepsilon\sigma T^4 \approx 355\text{ W/m}^2$ at $\varepsilon = 0.85$ and $20^\circ\text{C}$); net rejection figures accounting for Earth IR and albedo (250 W/m² in LEO, 330 W/m² in GEO) are **game estimates**.
- **FORMOSAT-5 Rear Solar Panel Radiator:** [Solar Panel Temperature Reduction Study of FORMOSAT-5 Satellite](https://www.airitilibrary.com/Article/Detail/19907710-201106-201108030008-201108030008-111-117)
- **Loop Heat Pipes:** [Propylene Loop Heat Pipe Design and Thermal Performance](https://tfaws.nasa.gov/wp-content/uploads/TFAWS23-PT-19-Paper.pdf) (NASA PACE); use of heat pipes and deployable radiators on large communications satellites is standard aerospace practice.
- **Satellite Thermal Control Review:** [Advances in satellite thermal management systems: Challenges, innovations, and future directions](https://www.sciencedirect.com/science/article/pii/S0094576526000883)

### Operations Orbit View Earth Rendering

- **Coastlines & Ice Sheets:** [Natural Earth](https://www.naturalearthdata.com/) 1:110m `ne_110m_land` and `ne_110m_glaciated_areas` (public domain), converted into a 192×48 Northern Hemisphere pixel map via `tools/earth-texture.py`.
- **Arctic Sea Ice Extent:** Annual maximum ~14–15 million km² in March, minimum ~4 million km² in September ([NSIDC Arctic Sea Ice News](https://nsidc.org/arcticseaicenews/)). The game approximates annual mean ice cover as ocean north of 77°N (**game estimate**). Google Earth uses cloud-free stitched imagery with bathymetric shading and omits drifting sea ice; daily true-color imagery is viewable on [NASA Worldview](https://worldview.earthdata.nasa.gov/).
- **Sun-Synchronous Orbit (SSO):** ~97.6° inclination at 550 km altitude; Earth observation satellites commonly use a 10:30 AM descending node.
- **Ground Station:** Taiwan Space Agency (TASA) in Hsinchu (24.8°N, 121.0°E). A satellite at 550 km has a ground visibility horizon radius of $\arccos(R_\oplus / r) \approx 22.6^\circ$, meaning an equatorial LEO satellite never rises above Taiwan's horizon—necessitating an inclined orbit.

### PSLV & Ionosphere Probe Mission

- **PSLV-XL:** 1,750 kg to 620 km SSO, 1,425 kg to Sub-GTO (284 × 20,650 km): [Polar Satellite Launch Vehicle — Wikipedia](https://en.wikipedia.org/wiki/Polar_Satellite_Launch_Vehicle). Estimated cost ₹130–200 crore (~$16–29M); NSIL does not publish list prices, so **the game uses $25M**.
- **PSLV Flight Heritage:** PSLV-C62 (January 12, 2026) was its 64th launch and failed due to a third-stage anomaly, following a prior failure on PSLV-C61 (May 2025): [PSLV-C62 / EOS-N1 Mission — ISRO](https://www.isro.gov.in/Mission_PSLV_C62.html); [ISRO's first 2026 mission PSLV-C62 fails — Deccan Herald](https://www.deccanherald.com/science/space/isros-first-2026-mission-pslv-c62-fails-3859678). Accounting for 4 failures (1993, 2017, 2025, 2026) and 1 partial failure (1997), the game records 59 successes out of 64 launches. As of October 2026, the next mission (C63) had not yet launched.
- **Sub-GTO Δv Penalty:** Circularizing from 284 × 20,650 km at 17.9° inclination (GSAT-12's actual transfer orbit) into GEO requires ~1.99 km/s—roughly 0.16 km/s more than the 1.83 km/s from a Cape Canaveral GTO, computed directly via Hohmann transfer and apogee plane-change equations.
- **Evaluated but Rejected — Firefly Alpha:** 7 launches, 3 successes, 2 failures, 2 partial failures: [Firefly Alpha — Wikipedia](https://en.wikipedia.org/wiki/Firefly_Alpha); [Firefly Alpha FLTA007 launches successfully — NASASpaceFlight](https://www.nasaspaceflight.com/2026/03/firefly-flta007-launch/).
- **Electron Flight 13 Failure (July 2020):** Failed due to an electrical connection fault on Stage 2, losing 7 satellites (widely documented).
- **FORMOSAT-5 AIP (Advanced Ionospheric Probe):** Sensor head 0.58 kg + boom 0.92 kg + electronics box ~2 kg + harness ~1 kg, ~4.7 W average power; measures ion composition, total ion density, 2-axis ion velocity, and ion/electron temperatures across 4 modes (PLP, RPA, IT, IDM): [FormoSat-5 — eoPortal](https://www.eoportal.org/satellite-missions/formosat-5).
- **FORMOSAT-7 TGRS (TriG GNSS-RO Receiver):** ~6 kg, 50 W, > 4,000 occultation profiles/day; satellite wet mass ~278 kg: [FormoSat-7 / COSMIC-2 — eoPortal](https://www.eoportal.org/satellite-missions/stp2-formosat-7).
- **Orbital Parameters:** FORMOSAT-5 operates in a 720 km SSO (10:30 AM/PM local time); FORMOSAT-7 operates 6 satellites at 24° inclination (same eoPortal sources).
- **NASA GOLD:** Observes the upper atmosphere in far-ultraviolet from GEO (hosted on SES-14), cited to illustrate remote sensing vs. *in situ* plasma measurements.
- **Electric Thruster Throttling:** NSTAR operates across 0.5–2.3 kW: [NSTAR — Astronautix](http://www.astronautix.com/n/nstar.html). The game's "fails to ignite below 1/4 rated power" cutoff is a **game estimate**.
- **Launch Event Milestones:** Payload fairing jettison at ~110 km altitude (where free-molecular heating drops to acceptable levels), PSLV-XL's 6 solid strap-on boosters (2 air-lit), and Falcon Heavy's simultaneous dual side-booster landings on its 2018 maiden flight: standard launch vehicle documentation.
- **Solar Array Specific Power:** ROSA delivers ~3× the specific power of rigid panels (see Redwire source above). In-game system specific powers (including batteries and power control)—25 W/kg for body-mounted, 35 W/kg for deployable, 70 W/kg for ROSA—along with per-kW costs and the 300 W cap on body-mounted cells are **game estimates**.
- **ViaSat-1 Launch Mass:** Launched on Proton in 2011 (~140 Gbps) at ~6.7 metric tons (public data), used to set the high-throughput transponder payload mass at 2,200 kg.
- Hall thruster and NSTAR power consumption figures are cited under "Propulsion Systems" above.

> Plasma instrument costs, daily data volumes, Langmuir probe mass/power, the "7-parameter" scoring formula, and the Ionosphere mission budget are **game estimates**.

### Flight Computers (Compiled in "rocket game Computer selection", Added 2026-11-09)

Team members compared five real-world spaceflight computers (sourced from manufacturer datasheets, SatCatalog, and satsearch):

| Model | Power | Durability / Radiation | Mass | Price | In-Game Role |
|---|---|---|---|---|---|
| GomSpace NanoMind A3200 | 0.17–0.9 W | −30 to +85 °C, flight heritage, COTS components, no published TID | 24 g | Undisclosed | Prototype for "Lightweight COTS" tier |
| ISISPACE iOBC | 0.40 W typical, ~0.55 W max | Flight heritage since 2012–2014, −25 to +65 °C, no formal TID spec | 94 g | ~US$5,200–8,090 (SatCatalog, FY2021) | Space Codex entry only |
| AAC Clyde Space Kryten-M3 | 0.40 W typical, 1 W max | 20 krad TID, 5-yr LEO design life, SEL protection & EDAC, −40 to +80 °C; Cortex-M3 50 MHz | 61.9 g | Undisclosed | Space Codex entry only |
| EnduroSat OBC | Listed as "Low power" on public page | 40 krad TID, ~5-yr LEO, TRL 9; 32 GB-class storage, ECC FRAM, optional GNSS | 123 g (173 g with GNSS) | US$12,400 list price | Prototype for "High-Capacity" tier |
| BAE RAD750 3U CompactPCI | < 10.8 W (~11 W at 132 MHz, ~12.2 W at 198 MHz) | > 100 krad board-level TID, 200 krad–1 Mrad processor TID, latch-up immune; Mars rover & deep-space heritage; ~200 MHz clock | 549 g | Commonly cited at > US$200,000 per processor | Prototype for "Radiation-Hardened" tier (dual-redundant) |

> In-game flight computers represent the entire Command & Data Handling (C&DH) subsystem (processor board + solid-state recorder + harness + flight software V&V). Consequently, the three tiers' masses (0.5 / 2 / 10 kg), power draws (1 / 5 / 25 W), costs ($0.3M / $1.5M / $8M), daily data throughput limits (100 / 4,000 / 1,000 Gbit), and the accounting split where "Standard computer specs are factored out of the baseline spacecraft bus" are **game estimates**.

### Font & Art Asset Licenses

- **Cubic 11 (俐方體11號):** [ACh-K/Cubic-11 — GitHub](https://github.com/ACh-K/Cubic-11), licensed under SIL Open Font License 1.1, loaded via jsDelivr pinned at v1.500.
- **Noto Sans TC:** Google Fonts, licensed under SIL Open Font License 1.1.
- All in-game pixel icons, panel frames, starfield backgrounds, launchpad graphics, components, and rocket sprites are procedurally generated via Python scripts in this repository (`tools/` directory); `image/Stars/Saturn.png` was contributed by a team member.

## Space History Behind Each Achievement

### 🏆 First Orbit — Apollo 11
- [Apollo 11 Mission Overview — NASA](https://www.nasa.gov/history/apollo-11-mission-overview/)
- [Apollo 11 — NASA](https://www.nasa.gov/mission/apollo-11/)
- [July 20, 1969: One Giant Leap For Mankind — NASA](https://www.nasa.gov/history/july-20-1969-one-giant-leap-for-mankind/)

### 💰 Under Budget — Mangalyaan (Mars Orbiter Mission)
- [India's $74 million Mars mission cost less than 'Gravity' movie — CNN](https://www.cnn.com/2014/09/25/news/india-mars-cost)
- [Mars Orbiter Mission — Wikipedia](https://en.wikipedia.org/wiki/Mars_Orbiter_Mission)

> Non-NASA resource — an Indian Space Research Organisation (ISRO) mission highlighting "succeeding on the first attempt on a shoestring budget."

### 🛡️ Storm Survivor — August 1972 Solar Storm
- [NASA Space Weather Living History — 1972](https://spaceweatherlivinghistory.org/timeline/31/)
- [How NASA Will Protect Astronauts From Space Radiation — NASA Goddard](https://svs.gsfc.nasa.gov/13275)

> Occurred between Apollo 16 (returned April 1972) and Apollo 17 (launched December 1972); had mission schedules shifted slightly, astronauts on the lunar surface could have absorbed lethal radiation doses.

### ⭐ All-Rounder — Voyager 1 & Voyager 2
- [Voyager — NASA JPL Mission Overview](https://voyager.jpl.nasa.gov/mission/)
- [Voyager 1: The Farthest Spacecraft — NASA Science](https://science.nasa.gov/mission/voyager/voyager-1/)
- [Interstellar Mission — NASA Science](https://science.nasa.gov/mission/voyager/interstellar-mission/)

> Voyager 2 is the only spacecraft to have visited Uranus and Neptune; Voyager 1 flew past Saturn and headed directly toward interstellar space. In-game flavor text was corrected accordingly to avoid implying both probes visited Uranus and Neptune.

### 💥 Glorious Failure — Vladimir Komarov & Soyuz 1
- [Vladimir Komarov — Wikipedia](https://en.wikipedia.org/wiki/Vladimir_Komarov)
- [The First Fatal Spaceflight — Smithsonian Magazine](https://www.smithsonianmag.com/air-space-magazine/first-fatal-spaceflight-180963019/)

> Non-NASA resource (Soviet mission). Launched April 23, 1967, and perished during re-entry on April 24 due to a parachute failure—becoming the first human fatality during a spaceflight. In-game text intentionally adopts a solemn, respectful tone.

### 🔁 Never Give Up — SpaceX Falcon 1
- [Falcon 1 — Wikipedia](https://en.wikipedia.org/wiki/Falcon_1)
- [SpaceX Successfully Launches Falcon 1 Rocket Into Orbit — Space.com](https://www.space.com/5905-spacex-successfully-launches-falcon-1-rocket-orbit.html)

> Non-NASA resource (private aerospace). After three consecutive launch failures (2006–2008), Falcon 1 reached orbit on its fourth flight in September 2008—saving SpaceX from near-bankruptcy.

### 🛠️ Bare Bones — Sputnik 1
- [Dawn of the Space Age — NASA](https://www.nasa.gov/history/dawn-of-the-space-age/)
- [Sputnik and the Origins of the Space Age — NASA History Office](https://www.nasa.gov/history/sputnik/sputorig.html)

> Non-NASA mission (Soviet), citing historical retrospectives from the NASA History Office.

### 💎 Top of the Line — James Webb Space Telescope (JWST)
- [How much did NASA's James Webb Space Telescope cost? — USAFacts](https://usafacts.org/articles/how-much-did-nasas-james-webb-space-telescope-cost/)
- [How much does the James Webb Space Telescope cost? — The Planetary Society](https://www.planetary.org/articles/cost-of-the-jwst)

> Official NASA lifecycle cost is ~$9.7 billion (development plus initial operations), commonly rounded in media coverage to the "$10 billion telescope," which the in-game text adopts.

### 💔 So Close — Mars Climate Orbiter
- [Mars Climate Orbiter — NASA JPL](https://www.jpl.nasa.gov/missions/mars-climate-orbiter/)
- [Mars Climate Orbiter Mishap — NASA Safety Center](https://sma.nasa.gov/news/safety-messages/safety-message-item/lost-in-translation)

> Lost in 1999 when one ground team calculated thruster impulse in metric units (newton-seconds) while another used imperial units (pound-force seconds), causing the probe to enter Mars' atmosphere too low and disintegrate—a classic engineering cautionary tale on unit conversion.

### 🧯 Crisis Management — Apollo 13
- [NASA Commemorates 50th Anniversary of Apollo 13, 'A Successful Failure' — NASA](https://www.nasa.gov/news-release/nasa-commemorates-50th-anniversary-of-apollo-13-a-successful-failure/)

### 🎯 Flawless Mission — Perseverance's "Seven Minutes of Terror"
- [7 Minutes to Mars: NASA's Perseverance Rover Attempts Most Dangerous Landing Yet — NASA JPL](https://www.jpl.nasa.gov/videos/7-minutes-to-mars-nasas-perseverance-rover-attempts-most-dangerous-landing-yet/)
- [Perseverance Team Overcomes Seven Minutes of Terror — NASA APPEL](https://appel.nasa.gov/2021/02/23/perseverance-team-overcomes-seven-minutes-of-terror/)

### 👑 Space Polymath
Does not correspond to a single historical event; it is an original hidden meta-achievement unlocked by earning all other 11 achievements, featuring a unifying congratulatory message without external citations.
