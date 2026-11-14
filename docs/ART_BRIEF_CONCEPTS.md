# Art Collaboration Brief— UI Redesign Concept Art

Create concept art showing "what the entire screen should look like." Once the team selects a direction, the lead programmer will implement it using HTML/CSS, so the concept art must be **feasible to build**.

## 1. Style Direction: Mission Control Center × Pixel Art

- **References:** NASA Mission Control Center, spacecraft cockpit instrument panels, sci-fi game HUDs.
- **Visual Style:** Retain the pixel art aesthetic (all game icons, components, and rockets are pixel art), while enhancing the UI with console elements such as glow effects, scanlines, gauges, indicator lights, corner brackets, and grid lines.
- **Color Palette:** Follow the existing game palette in `docs/ART_BRIEF.md` (deep blue background, cyan primary accent, gold rewards, red alerts, green nominal indicators).
- **Typography:** Use **Press Start 2P** (pixel font) for titles, buttons, and numbers; use **IBM Plex Sans** for body and descriptive text.
- **Language:** All in-game text is in English.

## 2. Screens to Illustrate

Each desktop screen must be **1440×900**. Please create **two design directions** (A and B) for both the Home Screen and the Satellite Design Screen:

- **Direction A: Dense Console** — Inspired by an authentic Mission Control Center: information-dense, packed with gauges, telemetry readouts, and numerous small indicator lights.
- **Direction B: Sleek HUD** — Inspired by sci-fi movie Heads-Up Displays: generous whitespace, thin crisp lines, and glowing highlights on key focal points.

### 1. Home Screen (1 for Direction A, 1 for Direction B)
Must include:
- **Title:** "Space Actuary", **Subtitle:** "SPACE MISSION DESIGN GAME", **Team Name:** "Mission Impaysible"
- **Main Menu:** Campaign (with progress across 4 stages: Basics / Propulsion / Space weather / Operations), Free Play (grayed out when locked), Custom Mission, Multiplayer, Space Codex, Achievements, Settings
- **Mission Records:** 5 mission types (High-Resolution Imaging, Global Climate Monitoring, Real-Time Typhoon Tracking, Broadband Communications, Ionosphere Probe), each displaying its mission icon, best score (e.g., "S 97"), and 3 medals
- **Key Art Background:** Earth, starfield, and a rocket on the launchpad (you may use rockets from `image/rocket/`)

### 2. Satellite Design Screen (1 for Direction A, 1 for Direction B) — Highest Priority
This is where players spend the most time. It must include:
- **Top Header:** Mission name + requirements (e.g., "High-Resolution Imaging · Low Earth Orbit — resolution ≤ 0.5 m · images reach the ground every day")
- **Step Progress Bar:** Briefing → Design → Launch → Report (currently active: Design)
- **Rocket Panel:** Selected rocket (sprite, name, cost, payload capacity, reliability) and a "Change Rocket" button
- **Satellite Blueprint:** Satellite schematic with 7 clickable component slots (Propulsion, Attitude Control, Structure & Shielding, Flight Computer, Power System, Communications, Payload); selected slots display the corresponding component sprite
- **Component Selection Menu:** Clicking a component slot reveals 3 option cards (each showing component sprite, tier name, stats such as "Handles 4,000 Gbit/day · 5 W · 2 kg · $1.5M", and a one-line description)
- **Mission Budget Table:** Most prominent element is "Expected mission value 84%"; accompanied by 7 resource bars—Cost ($144.8M / $145M), Mass (1,090 / 1,750 kg), Power, Thermal, Daily Data, Reserves, and Δv—each displaying values + a progress bar that turns red when exceeded; below that, display 6 mission value factors and 3 reliability metrics
- **Action Buttons:** "Back to Mission Briefing" and "Launch!" ("Launch!" is the primary CTA and must be the most visually prominent)

### 3. Results Screen
Must include:
- **Header:** "Mission Accomplished!" or "Mission Failed"
- **Rating:** Large rank letter (S / A / B / C / D) + "97 / 100" + a one-line summary
- **Performance Comparison:** Actual delivery vs. expected value (e.g., 58% vs. 50%) and the primary cause of value loss
- **Challenge Medals:** 3 medals (earned medals glowing, unearned medals dimmed)
- **Breakdown Chart:** A bar chart showing "Where the mission value went" (Mission Value Breakdown)
- **Action Buttons:** "Play Again" and "Back to Home"

### 4. Optional: Mobile Layout
One mobile version of the Satellite Design Screen (**390×844**) to explore how the three-column desktop layout stacks into a single column.

## 3. Guidelines for "Buildable" Concept Art

1. **Include only the elements listed above:** Do not invent features that do not exist in the game (such as characters, shops, or world maps). Purely decorative elements (indicator lights, grid lines, scanlines) are welcome.
2. **Keep panels rectangular:** Chamfered corners, corner brackets, and glowing borders are encouraged; avoid 3D perspective tilts, curved screens, or angled/skewed panels.
3. **Keep text short and legible:** Use the exact button labels and numbers specified above rather than blocks of unreadable greeking/placeholder text.
4. **Use existing game assets:** Component sprites are in `image/parts/`, mission and event icons in `image/icons/`, rockets in `image/rocket/`, and the satellite blueprint in `image/rocket/blueprint-sat-optical.png`.
5. **List new decorative assets:** If new decorative graphics (such as gauge frames or background textures) are needed, please list them in your notes so they can be produced in the next round.
