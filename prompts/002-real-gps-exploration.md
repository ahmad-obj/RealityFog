# Bilt Prompt 002 — Real GPS Exploration Sessions

Continue from the existing RealityFog implementation in:

`ahmad-obj/realityfog-d46c99`

Do **not** rebuild or redesign Phase 1. Preserve its fog engine, spatial-cell model, dark map, DEV simulation, and local explored-territory persistence.

Your objective is to add the first **real foreground exploration session** using device GPS.

## Core product rule

Fog may be permanently revealed by real location **only while the user has deliberately started an Exploration session**.

No passive 24/7 tracking.
No background location service.
No location collection when exploration is inactive.

## 1. Add foreground location support

Use Expo's supported location package/API for this project.

Configure:
- required package/dependency;
- iOS foreground/When-In-Use permission text;
- Android foreground location permission;
- Expo config/plugin changes required by the chosen implementation.

Do not request background-location permission.

## 2. Real exploration lifecycle

Add a primary map control:

- `Start Exploration`
- while active: clearly show session is running;
- `End Exploration`

Behavior:

### Start
1. If DEV simulation is running, stop it first.
2. Request foreground location permission if not already granted.
3. If permission is denied, do not start tracking or reveal anything.
4. Start a real exploration session.
5. Begin foreground GPS updates.
6. Show the real player position.

### During session
- accepted real GPS points update player position;
- accepted points reveal fog using the existing `revealAround()`/cell engine;
- collect a filtered route separately from explored fog state;
- update live session distance and duration;
- display a small GPS accuracy state without turning the UI into a dashboard;
- remain map-first.

### End
- stop the location watcher cleanly;
- persist/flush fog state;
- persist the completed session record locally;
- clear active tracking state;
- show a small completion result only; do **not** build the polished Session Summary screen yet.

## 3. Trusted-location filtering

Create a separate location filtering module rather than putting this logic in the UI.

Keep an interface conceptually equivalent to:

```ts
export type TrustedLocation = {
  latitude: number;
  longitude: number;
  timestamp: number;
  accuracyM: number;
};

export type LocationRejectionReason =
  | 'invalid-coordinate'
  | 'poor-accuracy'
  | 'stale-or-duplicate'
  | 'impossible-speed';

export function evaluateLocation(
  previous: TrustedLocation | null,
  next: TrustedLocation
):
  | { accepted: true; location: TrustedLocation }
  | { accepted: false; reason: LocationRejectionReason };
```

Use sensible documented initial thresholds. Recommended V1 defaults:

- coordinates must be finite and within normal latitude/longitude bounds;
- reported horizontal accuracy must be positive and **≤ 50 m**;
- timestamp must move forward;
- calculate implied speed from previous accepted point and reject clearly impossible jumps above **60 m/s**;
- rejected points must never reveal fog, add route distance, or grant progression.

If you choose slightly different thresholds because of Expo/device behavior, explain why in your completion report.

Do not over-engineer Kalman filtering yet.

## 4. Playable-world behavior

Existing rule remains:

- Islamabad + Rawalpindi are the playable world.

When an accepted GPS point is outside the playable world:

- the player's location may still display;
- route capture may record it for continuity;
- **do not reveal fog**;
- **do not count it as new territory/progression**;
- show a restrained `Outside playable area` state.

Do not add more cities.

## 5. Session data model

Add a real session model separate from the fog-cell store.

Conceptually:

```ts
export type ExplorationSession = {
  id: string;
  source: 'real' | 'simulated';
  startedAt: number;
  endedAt: number | null;
  route: TrustedLocation[];
  distanceM: number;
  newlyRevealedCells: string[];
};
```

You may add fields that are useful, but do not add fake zone/discovery/XP data yet.

Important:

- raw route is **history data**, not authoritative fog state;
- explored cell state remains authoritative for fog;
- simulated exploration must never be mixed into real session history as if it were genuine.

## 6. Local session persistence + interruption recovery

Add local persistence for:

- current active session draft;
- completed sessions.

Requirements:

- if the app briefly backgrounds or React remounts, do not silently lose an active session;
- when returning to foreground, restore the session state and resume the foreground location subscription if the user had an active session;
- no background tracking while the app is actually backgrounded;
- flush critical state on session end and lifecycle transitions where practical;
- storage failures affecting session data should be surfaced in a controlled way rather than silently pretending everything saved.

Do not add backend/auth yet.

## 7. DEV simulation coexistence

Keep the Phase 1 simulator.

But enforce:

- real exploration and simulation cannot run simultaneously;
- simulation remains unmistakably labelled as test data;
- real sessions have no `TEST DATA` badge;
- reset-dev-data must not accidentally wipe real session history unless the UI explicitly says it will.

If the current reset implementation wipes only explored fog, rename/reword it accurately.

## 8. UI direction

Do not redesign the entire screen.

Preserve:
- map dominance;
- dark exploration-game feel;
- minimal HUD;
- existing fog appearance.

Add only what the real session needs:

- primary `Start Exploration` / `End Exploration` control;
- session elapsed time + distance in a compact treatment;
- GPS accuracy indicator/state;
- permission/error/outside-world messaging.

Do not add tabs, dashboards, giant cards, history screens, profiles, social UI, achievements, or onboarding carousels.

## 9. Tests / verification

At minimum verify these behaviors in code/tests where practical:

### Location filter
- valid accurate point is accepted;
- >50 m accuracy point is rejected;
- stale timestamp is rejected;
- impossible GPS jump is rejected;
- rejected point cannot call the reveal path.

### Session lifecycle
- starting creates one active session;
- ending finalizes it once and stops the watcher;
- distance uses only accepted points;
- outside-world accepted point does not reveal fog;
- simulation and real exploration are mutually exclusive;
- restored active session does not duplicate route points or create a second session.

Run the project's actual lint/type-check command and any tests you add.

## Do NOT build yet

Do not add:

- background 24/7 tracking;
- location sharing;
- friends/social;
- auth/backend;
- leaderboards;
- zones/discoveries;
- XP/levels;
- polished history UI;
- AR;
- monetization;
- cities outside Islamabad/Rawalpindi.

## Completion report

When finished, report only:

1. files/modules added or materially changed;
2. exact Expo location configuration;
3. GPS acceptance/rejection thresholds;
4. real session lifecycle behavior;
5. interruption/recovery behavior;
6. local session-storage format;
7. tests/lint/type commands actually run and their exact result;
8. known limitations;
9. exact manual test steps I should perform on a physical phone before Phase 3.

Do not claim a check passed unless you actually ran it in this project state.
