# RealityFog V1 Implementation Plan

> **For agentic workers:** Actual implementation is delegated to Bilt AI. Execute this plan phase-by-phase from the prompts in `prompts/`, and review the resulting app before advancing.

**Goal:** Build a polished React Native + Expo exploration game for Islamabad and Rawalpindi where deliberate real-world movement permanently reveals fog-covered territory and every exploration session becomes personal history.

**Architecture:** Keep the map/fog engine independent from progression, authored content, history UI, and backend sync. Store explored territory as spatial cells/regions derived from trusted GPS points, while storing session routes separately for history/replay. Build local-first so sessions survive connectivity loss; add account sync only after the core loop is proven.

**Tech Stack:** React Native, Expo, TypeScript, Expo Location, a map SDK supported by the Bilt environment, persistent local storage, later authenticated backend/sync selected by Bilt only when Phase 6 begins.

**Spec:** `docs/superpowers/specs/2026-09-04-reality-fog-design.md`

## Global Constraints

- Initial playable geography is Islamabad + Rawalpindi only.
- V1 fog reveals only during deliberate exploration sessions.
- Revealed fog never returns.
- Repeated movement through already explored territory earns little/no exploration XP.
- Location history is private by default.
- Passive 24/7 tracking, live friend location, AR, global expansion, and monetization are deferred.
- The product must look and feel like an exploration game, not a generic navigation utility.
- Simulated-route mode is development-only and must be visibly labeled as test data.
- App code is produced in Bilt, not authored in this control repository.

---

## Intended Bilt Project Structure

Bilt may adapt filenames to its framework, but responsibilities must remain separated:

```text
app/
  _layout.tsx                 navigation/root shell
  index.tsx                   primary map/home screen
  history.tsx                 exploration history
  profile.tsx                 explorer profile/progression
  zones.tsx                   zones/discoveries browser
  session-summary.tsx         completed-session summary

src/
  components/
    map/FogMap.tsx            map rendering + fog presentation
    map/PlayerMarker.tsx      current player position
    map/DiscoveryMarker.tsx   discovery/mystery marker
    game/ProgressHUD.tsx      XP, zone progress, session stats
    common/*                  reusable visual primitives
  domain/
    location/types.ts
    location/filterLocation.ts
    exploration/types.ts
    exploration/sessionEngine.ts
    exploration/revealEngine.ts
    exploration/progression.ts
    history/types.ts
    zones/types.ts
  data/
    zones.ts                  authored Islamabad/Rawalpindi zones
    discoveries.ts            authored discovery content
  storage/
    exploredTerritoryStore.ts
    sessionStore.ts
    settingsStore.ts
  services/
    locationService.ts
    syncService.ts            added only when backend phase starts
  dev/
    simulatedRoutes.ts        clearly dev/test-only routes
  theme/
    tokens.ts
    typography.ts
```

Core domain interfaces should stay conceptually equivalent to:

```ts
export type LatLng = {
  latitude: number;
  longitude: number;
};

export type TrustedLocation = LatLng & {
  timestamp: number;
  accuracyM: number;
};

export type ExplorationSession = {
  id: string;
  startedAt: number;
  endedAt: number | null;
  route: TrustedLocation[];
  distanceM: number;
  newCells: string[];
  zoneIds: string[];
  discoveryIds: string[];
  xpEarned: number;
  source: 'real' | 'simulated';
};

export type Zone = {
  id: string;
  city: 'islamabad' | 'rawalpindi';
  name: string;
  polygon: LatLng[];
  theme: string;
};

export type Discovery = {
  id: string;
  zoneId: string;
  name: string;
  location: LatLng;
  radiusM: number;
  xp: number;
  rarity: 'common' | 'rare' | 'legendary';
};
```

---

### Task 1: Foundation + Fog-Reveal Prototype

**Bilt Prompt:** `prompts/001-foundation-fog-prototype.md`

**Produces:**
- Working Expo app shell.
- Visually strong map/home screen centered on Islamabad/Rawalpindi.
- Dark fog visual covering the playable world.
- Development-only simulated route that visibly reveals a corridor through fog.
- Local persistence of revealed test territory across restart/reload.
- Clear visual distinction between unexplored and explored land.

**Must not include yet:** auth, social, real progression balancing, large discovery dataset, passive tracking.

**Acceptance:**
- [ ] App launches without a blank/error screen.
- [ ] Map can pan/zoom smoothly.
- [ ] Fog is obvious and game-like.
- [ ] Running the simulated route visibly removes fog along the route.
- [ ] Revealed area remains revealed after restart/reload.
- [ ] Reset-test-data action restores fog for development.
- [ ] Simulated data is visibly labeled and cannot be confused with real exploration.

**Review gate:** Do not move to Task 2 until fog reveal is visually convincing and technically stable.

---

### Task 2: Real Exploration Session + GPS Quality Filtering

**Consumes:** fog/reveal engine from Task 1.

**Produces:** foreground location permission flow, deliberate Start Exploration / End Exploration lifecycle, trusted GPS filtering, real route capture.

Required location filtering behavior:

```ts
export function shouldAcceptLocation(
  previous: TrustedLocation | null,
  next: TrustedLocation
): boolean;
```

Rules:
- reject missing/invalid coordinates;
- reject poor accuracy beyond a practical threshold chosen and documented by Bilt;
- reject impossible jumps/speeds;
- do not reveal territory from rejected points;
- preserve session if connectivity drops.

**Acceptance:**
- [ ] Permission denial has a clear recovery path.
- [ ] Start Exploration begins foreground GPS capture.
- [ ] End Exploration stops capture cleanly.
- [ ] Real accepted GPS points reveal fog.
- [ ] Rejected low-confidence points do not reveal fog.
- [ ] Session survives brief app interruption.
- [ ] Outside Islamabad/Rawalpindi, location can display but no progression/reveal is granted.

---

### Task 3: Spatial Territory Model + Exploration XP

**Produces:** stable representation of explored territory separate from raw route history.

Required conceptual interface:

```ts
export function locationToCell(location: LatLng): string;
export function getRevealCells(location: LatLng, radiusM: number): string[];
export function getNewCells(candidateCells: string[], explored: Set<string>): string[];
export function xpForNewCells(newCells: string[]): number;
```

Rules:
- spatial cell resolution must be documented;
- rendering should hide the cell/grid nature with smooth/organic visual treatment;
- revisiting explored cells gives no base exploration XP;
- one bad GPS point must not unlock a huge region.

**Acceptance:**
- [ ] Explored state stays compact as routes grow.
- [ ] Same route twice gives essentially no new exploration XP.
- [ ] Adjacent legitimate movement reveals continuously without ugly large gaps.
- [ ] Map remains responsive with a meaningful amount of explored territory.

---

### Task 4: Authored Zones + Discoveries

**Produces:** first dense content layer for the twin cities.

Seed a small but intentional set rather than hundreds of generic POIs.

Minimum initial zone examples:
- F-6 / F-7 region
- Blue Area
- Margalla / Trail region
- Saddar
- old Rawalpindi / Raja Bazaar region

Minimum discovery categories represented:
- landmark
- viewpoint/trail
- park
- market/historic place
- food/social area

Required discovery behavior:

```ts
export function isDiscoveryReached(
  user: LatLng,
  discovery: Discovery
): boolean;
```

**Acceptance:**
- [ ] Zones show explored percentage.
- [ ] Entering/revealing territory updates zone progress.
- [ ] Reaching a discovery creates a satisfying one-time reveal moment.
- [ ] Discovery gives bonus XP once.
- [ ] Content feels curated, not like raw map search results.

---

### Task 5: Session Summary + Explorer History

**Produces:** permanent personal history value.

Each ended session must save:
- start/end time;
- route;
- distance;
- duration;
- newly revealed territory count/area proxy;
- zones entered;
- discoveries found;
- XP earned;
- real vs simulated source.

**Acceptance:**
- [ ] Ending a session immediately opens a polished summary.
- [ ] History timeline lists past sessions chronologically.
- [ ] Tapping a session opens detail.
- [ ] Route replay animates that historical route on the map.
- [ ] First-visit information can be derived for visited zones/discoveries.
- [ ] Simulated sessions are clearly separated from real history.

---

### Task 6: Explorer Profile + Progression Loop

**Produces:** reasons to return beyond map novelty.

Initial progression:
- explorer level;
- total real distance;
- Islamabad explored %;
- Rawalpindi explored %;
- discovery count;
- zone completions;
- weekly new-territory target;
- exploration streak only when actual new territory is revealed.

**Acceptance:**
- [ ] XP/level rules are deterministic and documented.
- [ ] Repeated commute cannot farm meaningful XP.
- [ ] Profile clearly communicates overall exploration identity.
- [ ] Weekly target is understandable at a glance.
- [ ] No fake social leaderboard data.

---

### Task 7: Local-First Persistence + Account Sync

**Produces:** authenticated persistence without making network availability a dependency for an active session.

Architecture rules:
- local state is written first during exploration;
- sync is asynchronous;
- backend never becomes the only copy of an in-progress session;
- merge behavior must not accidentally re-fog already explored territory;
- simulated/test data must not enter production progression.

**Acceptance:**
- [ ] Sign-up/sign-in works if auth is added.
- [ ] Existing local exploration can attach/migrate to account safely.
- [ ] App works through temporary network loss.
- [ ] Progress syncs across reinstall/device after login.
- [ ] Sync conflict cannot reduce explored territory.

---

### Task 8: Product Polish + Hardening

**Produces:** demo-ready V1.

Focus:
- fog reveal animation/haptics;
- session-start/session-end transitions;
- map performance;
- empty/error/permission states;
- visual identity consistency;
- GPS accuracy feedback;
- battery-conscious foreground location behavior;
- accessible contrast and touch targets;
- removal of placeholder/demo copy not intentionally labeled as test data.

**Acceptance:**
- [ ] No major visual screen looks like default Expo/template UI.
- [ ] Fog reveal is the visual centerpiece.
- [ ] Core loop can be understood without instructions.
- [ ] Location-permission, no-GPS, offline, outside-city, interrupted-session paths are tested.
- [ ] A fresh install can complete one full simulated demo flow and one real exploration flow.

---

## Execution Order

1. Foundation/fog prototype.
2. Real deliberate GPS sessions.
3. Spatial territory + XP.
4. Zones/discoveries.
5. History/replay.
6. Progression/profile.
7. Auth/sync.
8. Polish/hardening.

Do not collapse these into one giant Bilt prompt. Each phase must be reviewed against its acceptance checklist before the next prompt is issued.

## Review Protocol After Every Bilt Run

Record in `docs/PROGRESS.md`:

```text
Prompt: NNN
Bilt result: completed / partial / failed
Verified working:
- ...
Not verified / broken:
- ...
Decision changes:
- ...
Next prompt objective:
- ...
```

Never update progress from Bilt's claim alone; use the actual preview, screenshots, exported code, errors, or user test result.
