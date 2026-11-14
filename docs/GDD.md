# Game Design Document — Space Actuary

Official Game Title: **太空精算師** (English: **Space Actuary**). The word "Actuary" reflects the core gameplay loop—calculating and balancing trade-offs across budget, mass, and reliability.

## 1. Challenge Background

Competition Entry: [NASA Space Apps Challenge 2026 — Space Mission Design Game](https://www.spaceappschallenge.org/2026/challenges/space-mission-design-game/)

Official Summary (Excerpt): Designing a space mission requires making trade-offs among mission objectives, spacecraft design, scientific instruments, launch vehicles, budget, power, mass, communications, and orbital constraints. The challenge calls for an interactive game where players experience end-to-end mission decision-making from concept to operations, manage limited resources, and see how every choice impacts mission success or failure.

## v2 Overhaul: Realistic Mission Model

> This section supersedes the abstract numerical designs in Sections 3–8 and Section 12 below (retained as historical record). All real-world figures and sources are documented in `docs/REFERENCES.md`, with code implementation in `js/data.js` (parameters) and `js/mission.js` (calculations).

**Reason for Overhaul:** Exhaustive enumeration of 6,200 configurations in v1 revealed several flaws: science payloads had zero impact on mission success (all top 50 optimal builds chose the economy tier), there was only a single optimal solution, mass limits barely mattered, probabilities were completely hidden, and even the best build only achieved a 69.6% success rate. v2 is guided by two principles: **adhere to real space science** and **ensure players learn real space engineering by playing**.

**Core Objective:** Maximize "Expected Mission Value" = Mission Value × Probability of Success—the exact job of an actuary.

**Mission Value** = Coverage × Performance × Data Downlink × Power × Lifespan (each 0–100%):
- **Coverage:** Determined by orbital geometry (GEO only sees 42% of Earth and cannot view polar regions; a single LEO satellite passes overhead for only ~35 minutes per day; typhoon tracking requires an image every 10 minutes).
- **Performance:** Optical payloads calculate ground resolution via the diffraction limit ($1.22\lambda h / D$) and spectral band count; communications payloads are measured by transponder capacity.
- **Data Downlink:** (Downlink rate × daily contact duration [~36 min/day for LEO; 24 hours for GEO, but signal strength is ~4,200× weaker]) ÷ daily payload data generated.
- **Power:** Available solar array power (LEO × 0.62 due to Earth eclipse; GEO × 0.85 due to 15-year degradation) ÷ power consumption.
- **Lifespan:** Proportion of design life sustainable by the Δv calculated via the Tsiolkovsky rocket equation; electric propulsion takes ~6 months to spiral out to GEO, delaying service entry.

**Probability of Success:**
- **Launch:** Rocket flight heritage estimated using Laplace's rule of succession × structural survival against launch vibration loads.
- **Deployment:** 99% reliability per deployable mechanism.
- **Operations:** Component tier (COTS / Space-grade / Redundant) determines annual failure rate, multiplied by orbital radiation, structural shielding, and event modifiers. Operational failures result in "Partial Success," delivering prorated value based on active operational years.

**Hard Constraints** (prevent launch): Exceeding budget, exceeding rocket payload capacity to the target orbit, or selecting a rocket incapable of reaching the target orbit (e.g., Electron cannot reach GTO). All other parameters are soft constraints that penalize mission value or reliability—reinforcing the "learn from failure" philosophy.

**No Hardcoded Orbital Rules:** Orbital suitability emerges naturally from physics. Exhaustive enumeration confirms that Imaging/Climate missions perform best in LEO, while Typhoon/Comms missions perform best in GEO; choosing the suboptimal orbit still yields partial value (e.g., Climate in GEO yields ~36%).

**Mission Objectives** (4 original types, each modeled on real missions): High-Resolution Imaging (WorldView-3), Global Climate Monitoring (Terra/MODIS), Real-Time Typhoon Tracking (GOES / Himawari), and Broadband Communications (ViaSat / Intelsat). A 5th objective was added on 2026-10-12: Ionosphere Probe (FORMOSAT-5 AIP / FORMOSAT-7), detailed in "Playtest Feedback Overhaul" below.

**Propulsion Systems:** Hydrazine, AF-M315E, MMH/NTO Bipropellant (replacing hydrogen peroxide, as MMH/NTO is standard for GEO apogee engines), Hall-Effect Thruster, and Gridded Ion Thruster. Propellant load is set via a kg slider with real-time Δv readout, alongside an "Auto-Calculate (10% margin)" button. Hazardous propellant fueling safety costs are now mandatory (previously, making it optional was a false choice since buying it was always strictly optimal).

**Events** (9 types): Solar Particle Event, Geomagnetic Storm, Thunderstorm Delay, Meteor Shower, Orbital Debris, Ground Station Maintenance, Mass Growth, Budget Cut, and All Nominal—each grounded in real physical mechanisms and historical precedents. Removed "Upper-Level Wind Shear" and "Extreme Cold" (in reality, both simply cause launch delays, making them redundant with thunderstorms).

**Design Screen:** The right-hand "Mission Budget Table" displays real-time readouts for five engineering budgets—Cost, Mass, Power, Daily Data, and Δv (mirroring real systems engineering mass/power/link/Δv budgets)—alongside mission value breakdowns and reliability metrics. Every row is clickable for a physical explanation.

**Results Screen:** The "Post-Mission Actuarial Report" compares actual delivered value against pre-launch expected value, cites real-world historical analogs for failure modes, and lists all single-change design alternatives, highlighting the single tweak that yields the highest gain in expected value. The radar chart was replaced with 6 meaningful mission metrics.

**Campaign Levels:** Pass condition changed to delivering ≥ 50% mission value. Level 1 auto-configures propulsion (selecting hydrazine or bipropellant based on orbit and auto-calculating propellant mass); Level 2 unlocks propulsion selection; Level 3 introduces space environment events (guaranteed Solar Particle Event); Level 4 unlocks all mechanics.

**Balance Verification:** Used Python scripts to exhaustively enumerate all configurations, tuning budgets (initially $210M for LEO, $230M for GEO; updated on 2026-10-12 to per-mission budgets) and part parameters to ensure every mission has a distinct optimal build with no "all-premium" dominant strategy. Verified via 4,000 Monte Carlo simulations that displayed expected values match actual dice-roll averages (error < 2.1%).

### Operations Phase: Reserves, Anomaly Decisions, & Extended Missions (Level 4+ / Free Play)

- **Reserves** = Budget − Cost. Mirrors NASA's Joint Confidence Level (JCL) policy: major programs must budget at a 70% confidence level against overruns by holding Unallocated Future Expenses (UFE).
- **Component Failures:** 60% are recoverable (game estimate), triggering an interactive decision modal; 40% are catastrophic. Five failure types each offer a "Paid Repair" vs. "Free Degraded Mode" choice, all drawn from real historical incidents: Battery Degradation, Primary Transmitter Failure (leasing NASA Deep Space Network 70 m antennas), Payload Anomaly (Landsat 7 Scan Line Corrector failure—missing 22% of image data yet serving nearly 20 years; repair has a 50% failure chance), Propulsion Valve Anomaly (2016 Juno mission), and Reaction Wheel Failure (Kepler K2 mission; Hubble single-gyro mode).
- **Operational Events** (0.3/year in LEO; 0.1/year in GEO): Collision Avoidance Warning (NASA's 1/10,000 collision probability maneuver threshold), Solar Storm Alert (NOAA SWPC), and Ground Station Outage (leasing KSAT / AWS Ground Station).
- **Extended Mission:** After surviving its design life, remaining reserves can fund mission extensions at $4M/year, up to half the design life, subject to remaining Δv.
- **Value Cap:** Delivered value is capped at the value of completing the full design lifespan. Mission extensions only recover service time lost during anomalies—a low-resolution camera cannot become sharper simply by operating for extra years.
- **Actuarial Estimate:** Every anomaly choice displays "Expected Remaining Mission Value" (accounting for future failure probabilities and potential mission extension funded by remaining reserves), training players in expected-value decision-making.
- **Expected Mission Value Calculation:** Switched to a fixed-seed, 800-run Monte Carlo simulation (assuming optimal actuarial choices during operations); matches analytical formulas within < 0.4% error when operational mechanics are disabled.
- **Verification:** Exhaustive enumeration confirms that when operational mechanics are enabled, optimal builds intentionally retain cash reserves (LEO Imaging retains ~$7M for 91.4% EV vs. 89.9% when spending everything; GEO Typhoon Tracking retains $16–43M), successfully reproducing real-world reserve-holding trade-offs.

### Simulation Feedback & Visuals

- **Launch Suspense:** Launch, Ascent, Orbit Insertion, and Deployment stages each announce their probability of success before running a visual roll bar (green zone = success probability) whose pointer distribution matches the true RNG roll. Rockets now use pixel-art sprites, shake during Max-Q, and play an explosion animation with screen shake upon failure.
- **Operations Orbit View:** Top-down view over the North Pole to accurate scale (LEO orbital radius = 1.09× Earth radius; GEO = 6.62×). Orbital periods follow Kepler's third law, and ground stations rotate with Earth. Downlink beams render only when the satellite is above the local horizon ($\cos\theta > R_\oplus / r$)—LEO passes yield brief contact windows, while GEO stays locked in continuous 24-hour contact.
- **Mission Value Progress Bar:** Accumulates year-by-year during operations, marking the design's theoretical value cap. Extended missions render in a distinct color; satellites blink red during anomalies and turn gray upon total failure.
- **Real Geography & 3D Geometry:** Earth uses real Natural Earth coastlines (Greenland ice sheet, Arctic sea ice). LEO uses a 97.6° Sun-Synchronous Orbit (appearing from above the North Pole as a nearly polar flattened ellipse, turning semi-transparent on the nightside/far side), while GEO hovers fixed over Taiwan's longitude. The ground station is located in Hsinchu, checking line-of-sight via 3D elevation angle > 0°. Geometry naturally yields ~4 passes/day of 8–12 minutes each, matching the ~36 min/day figure in the budget table. To prevent frame drops from skipping brief ~0.15 s contact windows, each animation frame sub-steps through timestamps since the previous frame.
- Extended missions terminate as soon as lost value is recovered up to the design cap, preventing pointless choices where both options yield 0% gain.

### Scoring System

- **Actuarial Score** (0–100) = Player Design Expected Value ÷ Theoretical Maximum Expected Value for the exact same scenario (Objective × Event × Unlocked Mechanics). Evaluates decision quality rather than luck: the same design earns the exact same score regardless of RNG rolls. Ranks: S ≥ 95, A ≥ 85, B ≥ 70, C ≥ 50.
- **Theoretical Optimum:** Precomputed in-browser via `tools/best-designs.html` across ~38,000 configurations (2 orbits × rockets × 5 subsystems × propulsion types × 8 propellant loads), generating 60 scenarios in ~1.5 minutes saved to `js/bestdesigns.js`. Scenarios with operational decisions pre-filter the top 400 builds analytically, rank them via 120 Monte Carlo runs, and finalize the top 15 with full 800-run simulations (matching a 7× wider search identically). Tables store a parameter fingerprint; if balance numbers are edited without regenerating the table, the game falls back to live calculation (~1 second).
- **Campaign Progression:** Pass by delivering ≥ 50% mission value **OR** achieving an Actuarial Score ≥ 85 ("Unlucky outcome, sound engineering—Review Board approves"). Directly addresses team member Yong-Wei Liang's feedback about "repeated failures due to bad luck" without breaking physics (no RPG skill points to artificially boost rocket reliability).
- **Challenge Medals:** 3 per mission type, evaluated strictly on design parameters. All thresholds are verified via scripts to be achievable yet require deliberate trade-offs:
  - *Master Actuary:* Score ≥ 95
  - *Shoestring Miracle:* Cost ≤ 70% of budget, with Score ≥ 75 (Imaging) / 70 (Climate) / 85 (Typhoon) / 45 (Comms)
  - *Bulletproof (Imaging, Climate):* Total success probability ≥ 92%, Score ≥ 65 (reliability isn't free)
  - *All-Electric (Typhoon, Comms):* Use electric propulsion, Score ≥ 90 (Typhoon) / 75 (Comms)
  - *Scrapped Concepts:* "Engineering Margin" medal (optical optimum already had spare margin, making it a freebie) and "SmallSat" medal (Electron could score at most 17 points).
- **Waterfall Chart Replacing Radar Chart:** Starts from an ideal 100% mission, sequentially subtracting losses from Coverage, Performance, Data Downlink, Power, and Lifespan (red bars = design bottlenecks) to arrive at Design Value; then subtracts Launch, Deployment, and Operational risks (orange bars) to arrive at Expected Value; finally displays Theoretical Optimum and Actual Delivered Value side-by-side. Because the 6 radar axes are multiplicative, polygon area was meaningless; the waterfall chart immediately shows "what ate the mission value."
- **Personal Best:** Tracks highest Actuarial Score per mission type.

### Restrictive Events (Adapted from You-Ren Zhou's Proposals)

- **Export Control:** All components restricted to Economy or Standard tiers (reflecting real ITAR/EAR regulations, which most commonly restrict radiation-hardened electronics and high-resolution optics).
- **Crowded Launch Manifest:** Falcon 9 is fully booked with constellation deployments; players may only select Electron or Falcon Heavy.
- **National Flagship Program:** Payload must be Premium tier, but budget increases by +$20M.
- Restricted cards are grayed out with explicit explanations; both Theoretical Optimum and Post-Mission Actuarial Analysis evaluate only valid choices so scoring remains fair. Exhaustive enumeration verified that every mission remains solvable under all three events with distinct optimal builds (e.g., Typhoon Tracking under Export Control caps at 33% value because without a Ka-band high-gain dish, GEO data barely gets downlinked).
- Rejected "International Condemnation: 50% pre-launch failure chance": Offers no player agency, reflects no real mechanism, and teaches nothing.

### Attitude Control Subsystem (Data by Yong-Wei Liang)

- New subsystem **"Attitude Control,"** unlocked alongside Propulsion in Level 2 (auto-equipped with 3 reaction wheels in Level 1):
  - *Economy:* Magnetorquers (~2° pointing accuracy, 3 kg, $0.5M). Geomagnetic field strength scales as $1/r^3$, meaning GEO's field is ~1/226th that of a 550 km LEO—resulting in > 90° equivalent pointing error ("unable to stabilize pointing") and zero mission value.
  - *Standard:* 3 Reaction Wheels + Star Tracker (~0.02° pointing accuracy).
  - *Premium:* 4 Reaction Wheels (pyramid configuration), providing 1 hot spare and using "Redundant" failure rates.
- Added **"Pointing"** multiplier to Mission Value = $\sqrt{\text{Mission Requirement} \div \text{Actual Pointing Error}}$. Requirements: High-Res Imaging 0.05°, Climate Monitoring 0.5° (wide swath), Typhoon Tracking 0.05°, Comms Spot Beam 0.1°.
- Split original spacecraft bus ($8M / 40 kg / 60 W) into "Avionics + Thermal + Harness" ($4M / 20 kg / 20 W) plus Standard Attitude Control, keeping baseline balance intact.
- Added Attitude Control operational anomalies (Reaction Wheel failure modeled on Kepler K2; converts to "Magnetorquer Coil Open-Circuit" if only magnetorquers are fitted). Renamed spacecraft bus anomaly to "Flight Computer Crash" (Single-Event Upset, modeled on Hubble's 2021 payload computer incident).
- Added **"Thermal Dissipation"** readout row to the budget table (informational only; does not penalize value): Virtually all consumed electrical power converts to waste heat. Radiator area = Heat Load ÷ Net Radiative Rejection per m² (250 W/m² in LEO, 330 W/m² in GEO, derived from $\varepsilon\sigma T^4$ minus absorbed Earth IR), indicating whether passive cooling (≤ 1 kW), heat pipes (≤ 5 kW), or loop heat pipes + deployable radiators (> 5 kW) are required. Abstract stats like "Thermal Reliability Bonus: Medium" from team drafts violated the physics-first rule, so thermal was implemented as an educational engineering readout rather than a new purchasable part.
- Rejected "Cold Gas Thrusters" from team drafts: Attitude thrusters and orbit-keeping propulsion are already abstracted into the Propulsion subsystem; splitting them would cause combinatorial bloat.
- **Theoretical Optimum Breakdown:** Expandable on the Results screen, teaching players that "optimal designs don't always spend the entire budget or pick all-premium parts."

## Visual & UI Unification

- **Typography:** Titles, buttons, numbers, and labels switched to Traditional Chinese pixel font **Cubic 11** (俐方體11號, SIL OFL license, loaded via jsDelivr pinned at v1.500); body paragraphs use **Noto Sans TC**. Previously, Press Start 2P only supported Latin characters, causing Chinese characters to fall back to system fonts and mix two typefaces inside a single button.
- **Font Sizes:** Pixel fonts restricted strictly to 11 / 16 / 22 / 33 / 44 px; body text restricted to 12 / 13 / 14 px.
- **Color Palette:** Centralized as CSS variables at the top of `css/style.css`; eliminated hardcoded hex values in components.
- **Frames:** Major panels use 9-slice frames matching the button palette (with HUD corner brackets); cards and list items use 2 px solid borders with 4 px border-radius.
- **Icons:** Replaced all in-game emojis with custom pixel-art icons (55 icons generated via `tools/pixel-icons.py` with automatic 1 px dark outlines and top-left lighting). Rendered strictly at 1x or 2x native resolution to prevent uneven pixel scaling.
- **Home Screen:** Pixel-art title + starfield, planets, and rocket; 4-stage Campaign progress tracker; best scores and medal displays across mission types.
- **Mission Flow:** Top step indicator bar across Briefing, Design, Launch, and Report screens; labeled stage nodes on the Launch timeline.
- **Achievements Screen:** Added Challenge Medal showcase (3 per mission) and unlock counter.

## Playtest Feedback Overhaul

Team playtesting combined with analytical scripts pinpointed one root flaw: **small rockets served no purpose, and budgets were far too generous.** Previously, Electron topped out at 17 points on any mission, and almost every optimal design simply bought all-premium components.

### Rebalancing: New Rocket, New Mission, Tighter Budgets

- **PSLV (Small-Medium Lift):** India's workhorse launcher—1,750 kg to SSO, ~$25M, 59 successes in 64 flights (90.9% Laplace reliability). Serves as a realistic "cheap but less reliable" trade-off: optimal builds for Imaging and Climate now use PSLV, saving $49M for better instruments, whereas switching to Falcon 9 (99.4%) caps optimal score around 89. Mirrors real life—0.25 m resolution Cartosat-3 launched on PSLV. Evaluated Firefly Alpha (3/7 successes, ~44% estimated reliability), but deemed it too risky to ever be rational.
- **Sub-Synchronous Transfer Orbit (Sub-GTO):** PSLV can only inject GEO payloads into a 284 × 20,650 km transfer orbit at ~18° inclination, requiring an extra ~0.16 km/s Δv to circularize into GEO (computed directly from orbital mechanics). This unlocked a new optimal build for Typhoon Tracking: an all-electric PSLV + Hall-effect thruster satellite (Level 2).
- **5th Mission Type — "Ionosphere Probe":** Inspired by FORMOSAT-5's AIP and FORMOSAT-7's radio occultation receivers. Plasma instruments must fly *in situ* through the ionosphere; GEO (35,786 km) sits far outside it, yielding 0 coverage. Because the instrument weighs only a few kilograms and the budget is $40M, **Electron is the optimal launcher**—giving the small rocket a genuine niche. Performance scales with measurable plasma parameters (2 for Langmuir probe, 6 for AIP-class, 7 with GNSS occultation).
- **Per-Mission Budgets:** Tightened so buying all-premium parts always exceeds budget: Imaging & Climate LEO = $145M (down from $210M → $175M); Typhoon GEO = $160M (down from $230M); Comms GEO = $230M (already tight, later adjusted to $250M); Ionosphere = $40M.
- **Minimum Power Threshold for Electric Propulsion:** Electric thrusters can throttle down, but fail to ignite below 1/4 rated power (NSTAR throttles from 0.5–2.3 kW). Previously, LEO builds skipped thruster power checks, allowing a 150 W SmallSat to mount a 1,350 W Hall thruster.
- **Propellant Slider Increment:** Reduced from 25 kg to 5 kg so SmallSats carrying only a few kilograms of propellant aren't forced to carry 3× excess mass.
- **Optimal Expected Values (Level 2 / Free Play):** Imaging & Climate = 86% / 89%; Typhoon = 73% / 83%; Comms = 73% / 82%; Ionosphere = 79% / 84.5%.

### Medal Re-Verification

- *Shoestring Miracle:* Score thresholds updated to Imaging 75 / Climate 70 / Typhoon 70 / Comms 45 / Ionosphere 85.
- *Bulletproof:* Assigned to Imaging (total success probability ≥ 90%, Score ≥ 65) and Ionosphere (≥ 80%, Score ≥ 55).
- *New Medal — "Safe Ascent" (Climate):* Launch on a rocket with ≥ 99% reliability and achieve Score ≥ 75—directly highlighting the PSLV vs. Falcon 9 trade-off.
- *All-Electric:* Retained for Typhoon (90) and Comms (75) (later revised in Playtest Round 2).
- *Crowded Launch Manifest* (banning Falcon 9) now affects GEO missions specifically, since LEO missions naturally favor PSLV or Electron.

### UI & Onboarding Improvements

- **Satellite Blueprints Replacing Rocket Schematics:** Players are designing a satellite, not a rocket. Hotspots now sit at true physical locations (solar wings on the sides, thruster nozzles on the zenith face away from Earth, antennas and payloads pointed nadir toward Earth), with unique schematics for each payload category. Players can begin designing without picking a rocket first.
- **Propellant Slider Cap:** Dynamically set to 3× the auto-calculated propellant mass (previously fixed at 6,000 kg, where a LEO satellite's optimal load sat at 1.7% of the bar, making every drag look like 99%).
- Added back-navigation buttons from Briefing to Home, and from Design to Briefing.
- **Level 2 Propulsion Explanation:** Clarified that liftoff from Earth requires thrust-to-weight > 1 (mandating chemical rockets), whereas once in orbital free-fall, even tiny electric thrust accumulates velocity over time—correcting the misconception that chemical propulsion is always required to overcome gravity.
- **Luck Callout:** Whenever actual delivered value deviates by > 15% from expected value, the score readout explicitly notes "Average value delivered across many flights of this exact design," explaining why the design score remains unaffected by RNG.
- **Operations Time Scale:** Labeled "1 day = 20 s, 1 year = 2.4 s" on-screen, noting that simulating a 5-year mission in real time would take 10 hours; extended operations animation from 9 s to 12 s.
- **Line-by-Line Theoretical Optimum Breakdown:** Expanding the optimal build lists each subsystem choice on its own row, showing the exact EV delta from swapping that single part (or noting if it causes budget/mass overruns or is banned by events).
- **Staged Ascent Events:** Added visual booster separation (PSLV, Falcon Heavy), MECO & Stage 1 separation, and fairing jettison (~110 km), physically splitting and animating the rocket sprite segments.
- **Stuck-Player Hints:** Failing the same level twice prompts ORBIT to hint at the optimal orbit on the Briefing/Design screens; a 3rd failure hints at the optimal rocket, and a 4th hints at the payload tier.
- Rocket failure flavor text now cites real historical failures for each specific launcher (Electron Flight 13 in 2020, PSLV 2025–2026 failures, Falcon 9 CRS-7).

### Ascent Animation Overhaul (Feedback by Yuan-Li Chen)

- **Gravity Turn Trajectory:** Rockets lift off vertically from the pad before pitching east, allowing gravity to curve the trajectory until horizontal at orbital insertion. Trajectory approximates a typical LEO ascent (modeled on Falcon 9: Mach 1 at ~T+1:10, Max-Q at ~T+1:20, MECO at ~T+2:45 around 85 km, reaching ~7.8 km/s horizontal velocity at insertion), with rocket pitch matching the flight path angle. Altitude and downrange distance use logarithmic scaling so both the launchpad and the Kármán line fit cleanly into a single sequence.
- **Telemetry Altimeter:** Live readouts for altitude, velocity, and pitch angle demonstrate that "altitude is reached long before orbital velocity"—showing players that staying in orbit is about horizontal speed.
- **Atmospheric Layers:** Sky transitions from blue to black with altitude, featuring tropospheric clouds and Mach diamonds in the exhaust, transonic vapor cone at Mach 1, throttle-down at Max-Q, stratosphere (ozone layer), exhaust plume expansion as ambient pressure drops, mesospheric meteors, visible Earth curvature above 50 km, the Kármán line, and green airglow at ~95 km. One-line captions explain each layer as it appears.
- Dice rolls align with physical flight events: Ascent check pauses at Max-Q; Orbit Insertion check pauses mid-Stage 2 burn.
- Removed the 8 background solar system planets from the ascent view, as seeing planets lined up during launch violated scientific realism.

### Learning Objectives & Post-Mission Quiz (Suggested by Yuan-Li Chen)

- **Level Learning Objectives:** Campaign briefings list 1–3 verifiable learning goals at the top (Level 1: Orbit selection & GEO comms; Level 2: Rocket equation, electric propulsion, magnetorquers; Level 3: Environmental events alter risk; Level 4: Reserves & anomaly response).
- **"What You Learned" Quiz:** Results screen presents 2 multiple-choice questions (3 options each) with instant feedback and one-line explanations. Campaign levels quiz mechanics taught in that specific stage; Free Play draws from a pool of 10 questions. Questions dynamically insert numbers from the player's actual mission (e.g., their chosen comm system's downlink rate in LEO vs. GEO).
- Answering correctly checks off and saves progress toward the level's learning objectives.
- Quizzes **do not affect scores or level progression**, honoring the "learn from failure" design principle.

### Custom Mission Mode

- Added **"Custom Mission"** to the Home Screen, unlocked from the start (allowing educators and judges to inspect any scenario directly): players freely combine any of the 5 mission objectives with any of the 12 space environment events, with all mechanics enabled.
- Enables direct experimentation on how events reshape optimal engineering builds (e.g., seeing "Crowded Launch Manifest" push Typhoon Tracking onto PSLV). Uses the exact same scoring, medals, and records as Free Play since all combinations are precomputed in the optimum table.

### Score Cap on Mission Failure

- **Playtest Feedback:** Having a satellite fail at 1.6 years with only 31% value delivered yet receiving an "S 98" rank felt like a bug to players.
- **Resolution:** Separated scores into **Design Score** (Expected Value ÷ Theoretical Optimum, luck-independent, used for Review Board pass overrides and actuarial analysis) and **Mission Grade**. Whenever delivered value falls below 50%, the Mission Grade is capped at 84 (B), and challenge medals are only awarded upon mission success.
- Results screen displays both "Your design itself scored 98" alongside the luck explanation, preserving the actuarial lesson that "good decisions do not guarantee good outcomes."

### Playtest Round 2: Demand-Sized Power Systems, Heavy Comms Satellites, & Ground Station Commands

- **Auto-Sized Power Systems Based on Power Demand** (Yong-Wei Liang, JOY): Previously, power tiers had fixed outputs (150 W / 5 kW / 20 kW), allowing a 100 W SmallSat to mount a 20 kW array and mindlessly max out every other part. Real spacecraft engineering sizes the solar array *after* calculating bus and payload power loads. Players now choose **solar array technology**: Body-Mounted Cells (25 W/kg, 300 W cap, COTS), Deployable GaAs Arrays (35 W/kg, Space-grade), and Roll-Out ROSA Arrays (70 W/kg, includes redundant batteries). Required array wattage = (Bus + Payload + Electric Thruster Load) ÷ Orbital Sunlight Factor, scaling mass and cost linearly per watt.
- SmallSats can no longer trivially mount electric propulsion: adding a Hall thruster requires an extra 1,350 W, bloating solar array mass and cost—and body-mounted cells (300 W max) cannot ignite it at all.
- **High-Throughput Transponder Mass Increased from 1,400 → 2,200 kg** (ViaSat-1 launched at ~6.7 metric tons). A chemically propelled premium comms satellite now exceeds Falcon 9's 5.5-ton GTO payload limit, forcing players to either buy Falcon Heavy or switch to an all-electric bus—mirroring the real-world emergence of Boeing's all-electric 702SP platform. Theoretical optimum is Falcon 9 + Hall Thruster, with Falcon Heavy + Bipropellant as a close runner-up.
- **Re-Calibrated Budgets:** Imaging & Climate LEO = $145M; Comms GEO = $250M.
- **Medal Adjustments:** Restricted *All-Electric* to Typhoon Tracking (lowering score threshold from 90 → 80). Since Comms already favors all-electric as its optimal build, replaced its medal with **"Instant Broadcast"**: use chemical propulsion with Score ≥ 75 (effectively requiring Falcon Heavy, giving the heavy launcher a clear purpose).
- **Estimated Propellant Readout on Thruster Cards:** Cards now display "Estimated propellant needed for this mission: ~X kg." LEO requires only ~0.15 km/s, so thruster choice barely changes propellant mass (accurate physics), whereas GEO requires ~2.6 km/s, making efficiency differences immediately obvious.
- **Ground Station Pass QTE** (Suggested by Yan-Yi Li): During operations, whenever a LEO satellite passes over Hsinchu, a "Send Downlink Command" button pops up for ~1.6 s (GEO triggers a routine command prompt every 3 s). Results screen displays successful contact count. Because real satellites buffer data onboard until commanded by a ground station, missed prompts do not penalize mission value or score.
- **Rejected:** Deep-space / interplanetary probes (You-Ren Zhou: mechanics would become overly complex). Pending clarification from Yong-Wei Liang on what "risks feel too simple" entails.
