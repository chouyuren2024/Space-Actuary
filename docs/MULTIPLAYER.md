# Multiplayer Setup (Firebase)

Multiplayer mode uses **Firebase Realtime Database** for real-time room synchronization. The website itself remains hosted on GitHub Pages without requiring a dedicated backend server. Firebase's free Spark plan supports up to 100 concurrent connections—roughly 25 four-player rooms.

Setup is already complete (project `space-c-16630`), and the configuration is stored in `js/netconfig.js`. If `netconfig.js` is set to `null`, the "Multiplayer" button will remain hidden on the live site. For local development and testing, append `?net=local` to the URL to connect multiple tabs within the same browser (no Firebase required).

## 1. Create a Firebase Project (Using a Team Member's Google Account)

1. Go to <https://console.firebase.google.com/> → **"Add project"**, and enter a custom name (e.g., `space-actuary`). You can disable Google Analytics.
2. In the left sidebar, navigate to **"Databases & Storage → Realtime Database"** (labeled **"Build → Realtime Database"** in the legacy console) → **"Create Database"**. Make sure to select **Realtime Database**, *not* Firestore Database.
   - Choose **"Singapore (`asia-southeast1`)"** as the location for lower latency from Taiwan.
   - Start in **"Locked mode"** for security rules; we will replace them in the next step.
3. Once created, tabs for **"Data / Rules / Backups / Usage"** will appear at the top. Click **"Rules"**, replace the entire block with the JSON below, and click **"Publish"**:

```json
{
  "rules": {
    "rooms": {
      "$code": {
        ".read": "$code.matches(/^[A-HJKMNP-Z2-9]{4}$/)",
        ".write": "$code.matches(/^[A-HJKMNP-Z2-9]{4}$/)"
      }
    }
  }
}
```

   These rules ensure that clients can only read and write under `rooms/<4-character room code>` matching the game's code format. Users without a room code cannot list active rooms.

4. Click the gear icon at the top left (**"Project settings → General"**) → scroll down to **"Your apps"** → click the `</>` (**Web**) icon to register a web app (leave Firebase Hosting unchecked).
5. Copy the `const firebaseConfig = { ... }` object displayed on screen and send the contents inside the curly braces to the lead programmer.

## 2. Add Configuration to the Game

Open `js/netconfig.js` and replace `null` with the configuration object from the previous step:

```js
const FIREBASE_CONFIG = {
  apiKey: "...",
  authDomain: "....firebaseapp.com",
  databaseURL: "[https://....asia-southeast1.firebasedatabase.app](https://....asia-southeast1.firebasedatabase.app)",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "...",
};
```

Make sure `databaseURL` is included. If it is missing from the snippet, copy the URL displayed at the top of the Realtime Database page and add it manually.

*(Note: This config object is publicly exposed in the client-side source code and is not a secret key; write permissions are enforced by the database security rules above.)*

## 3. Gameplay Flow

1. One player clicks **"Multiplayer → Create Room"**, generating a 4-character room code.
2. Teammates click **"Multiplayer"** on their own devices and enter the room code to join (up to 4 players).
3. Each player claims a role; all 4 roles must be covered (one player may take on multiple roles in smaller groups):
   - **Mission Director:** Orbit, launch vehicle, overall budget
   - **Payload Scientist:** Scientific/commercial payload
   - **Systems Engineer:** Power system, structure & shielding, attitude control
   - **Comms & Flight Control:** Communications, propulsion; ground station uplinks and anomaly decisions during operations
4. The host selects the mission objective and space environment event (or randomizes them) and clicks **"Start Mission"**.
5. After the Mission Director selects the orbit, everyone enters the design phase; each player can only modify their assigned subsystems while viewing the shared satellite blueprint and budget table.
6. All players must click **"Ready for Launch"** before liftoff; if anyone modifies the design afterward, everyone's ready status resets.
7. Launch and operational outcomes are determined by a shared RNG seed sent by the host, ensuring every player sees the exact same simulation results. The Results screen includes a **"Team Contribution Report"**.
8. The host clicks **"Return to Room Lobby"** to start the next mission.

## Technical Notes

- `js/net.js`: Data abstraction layer providing a unified interface for both Firebase and local multi-tab testing backends.
- `js/multiplayer.js`: Handles rooms, role assignments, permissions, phase transitions, and state synchronization.
- **Room Data Schema:** `rooms/{roomCode}` = `{ hostId, phase, setup, mission, players, roles, design, flight, decisions }`.
- When a player closes their tab or disconnects, Firebase automatically removes them from the room; if the host leaves, host privileges transfer automatically to the earliest-joined remaining player.
- Completed rooms remain stored in the database (only a few KB each). If needed later, you can manually clear the `rooms` node from the Firebase Console.
