# RealityFog — Product Design

Date: 2026-09-04
Status: Ready for user review
Scope: Islamabad + Rawalpindi only

## 1. Product thesis

RealityFog turns Islamabad and Rawalpindi into an exploration game. The map begins hidden under fog. The only way to reveal the world is to physically travel through it.

The product has two equal reasons to exist:

1. **Game:** uncover the twin cities, complete areas, discover places, earn progression.
2. **Personal history:** build a permanent map of where you have actually been, when you went there, and how your explored world grew over time.

The app should feel like a dense authored game world, not Google Maps with a fog overlay.

## 2. Hard scope

### Included geography
- Islamabad
- Rawalpindi

No global launch map in the first product. The restricted geography is intentional so the experience can be dense and handcrafted.

### Core rule
- Unvisited traversable territory is hidden by fog.
- During an active exploration session, physical movement permanently reveals territory around the verified GPS route.
- Revealed fog does not return.
- Previously explored areas remain visible even when location tracking is off.
- Passive 24/7 reveal is not part of V1.

## 3. Core gameplay loop

1. Open the app and see your explored world surrounded by fog.
2. Start an exploration session with one action.
3. Move through real space.
4. GPS movement reveals map territory around your path.
5. New territory produces immediate visual and haptic feedback.
6. Discoveries, zone progress, XP, and statistics update during the session.
7. End the session and receive a concise exploration summary.
8. The journey is stored permanently in History.
9. Return later to expand the map, complete areas, or chase discoveries.

## 4. V1 systems

### A. Fog map
- Smooth dark fog covering unexplored territory.
- GPS trail reveals territory continuously around the user during an active session.
- Revealed territory should have organic edges rather than obvious square tiles in the UI.
- Current exploration should animate into the permanent explored map.
- Map remains usable underneath the game layer.

### B. Exploration session
A session records:
- start/end time
- route
- distance
- duration
- new territory revealed
- zones entered
- discoveries reached
- XP earned

V1 only records/reveals through deliberate exploration sessions. Passive background history can be evaluated later as a separate opt-in feature.

### C. Explorer History
Every completed session becomes a permanent entry.

History views:
- chronological timeline
- route replay on map
- first-visit dates
- visit counts by area
- distance and exploration statistics
- monthly exploration recap
- personal heatmap

The history is personal value, not only game telemetry.

### D. Zones
Islamabad and Rawalpindi are divided into authored regions instead of one flat percentage.

Examples:
- F-sectors
- Blue Area
- Margalla / trail regions
- Saddar
- old Rawalpindi
- Bahria / DHA areas where appropriate

Each zone can contain:
- explored percentage
- discovery count
- completion badge
- identity/theme
- optional future challenges

Exact zone boundaries belong in content data, not hardcoded UI logic.

### E. Discoveries
Handcrafted places create density.

Possible discovery classes:
- landmarks
- viewpoints
- parks
- markets
- historic places
- unusual streets/spots
- food areas
- trails

A discovery should feel like finding something, not checking into a POI database.

### F. Progression
Initial progression system:
- XP for newly revealed territory
- bonus XP for discoveries
- zone completion progress
- exploration streaks/weekly goals
- explorer level

Repeatedly traveling the same route should generate little or no exploration XP.

## 5. Experience principles

### Dense over broad
Two excellent cities are better than a shallow global map.

### Discovery over navigation
The product is not primarily a navigation app. Directions should never dominate the main experience.

### Game map, not utility map
Visual hierarchy should favor fog, explored territory, discoveries, progression, and the player's position.

### Minimal friction
Starting an exploration should take one action. Ending it should immediately show the reward/progress summary.

### Personal, not creepy
Location history is private by default. Social features must never expose live precise location unless explicitly enabled by the user in a future version.

## 6. Main screens

### 1. Map / Home
Primary screen.
- fog world
- player position
- current exploration progress
- nearby discoveries or mystery indicators
- start/end exploration control
- concise zone status

### 2. Session Summary
After exploration:
- animated route
- distance/time
- newly revealed area
- discoveries
- XP
- zone progress changes

### 3. History
- timeline of journeys
- filters by date/zone
- tap journey to replay

### 4. Explorer Profile
- level
- Islamabad explored %
- Rawalpindi explored %
- total distance
- discoveries
- zone completions
- streaks/achievements

### 5. Zones / Discoveries
Browsable progression view for the two cities.

## 7. Data model — conceptual

### User
Identity, profile, progression totals, settings.

### ExplorationSession
Start/end timestamps, GPS route, distance, duration, stats, associated discoveries/zones.

### ExploredTerritory
Compact representation of territory already revealed by the user. Implementation may use spatial cells/tiles internally, but the UI should render organic-looking reveal boundaries.

### Zone
Authored polygon/region with metadata, city, theme, completion rules.

### Discovery
Authored point/area with type, coordinates, rarity/XP, reveal rules, content.

### Visit
First visit, latest visit, count, linked session/zone/discovery.

## 8. Technical direction for Bilt

Bilt should build this as a real React Native + Expo mobile application.

Recommended architecture:
- Expo location APIs for foreground GPS tracking initially.
- A map implementation compatible with Bilt/Expo and capable of custom overlays, polygons/shapes, route lines, and smooth camera movement.
- Persistent local cache for map reveal/history so the experience survives weak connectivity.
- Backend for authenticated persistence/sync of user progress and history.
- Spatial reveal representation designed to avoid storing every GPS point forever as the authoritative fog state.

Important technical rules:
- Filter impossible GPS jumps and low-quality readings.
- Do not reveal large territory from a single inaccurate GPS point.
- Do not award exploration for obviously spoofed/impossible movement in later production hardening; full anti-cheat is not required for the first visual prototype.
- GPS tracking must stop cleanly when a session ends.
- Session data should not be lost if the app is briefly backgrounded or connectivity drops.
- Development/test builds may expose a clearly labeled simulated-route mode so the core loop can be validated without physically traversing the city; this must never count as legitimate production exploration.

## 9. Error/edge handling

- Location permission denied: explain why GPS is core and allow retry/settings path.
- Weak GPS: show degraded accuracy state; avoid revealing territory until confidence improves.
- Connectivity loss: continue session locally and sync later.
- App interruption: preserve active session state and recover safely.
- Outside Islamabad/Rawalpindi: show the location, but no world progression or fog-unlocking gameplay.
- Teleport/impossible GPS movement: ignore suspicious segment for reveal and XP.

## 10. V1 acceptance criteria

A usable first product must prove the loop:

1. User opens a map of Islamabad/Rawalpindi covered by fog.
2. User grants location permission.
3. User starts an exploration session.
4. Physical or explicitly labeled test/simulated movement reveals territory around a GPS route.
5. Revealed territory persists after restart and account sync.
6. User can end an exploration session cleanly.
7. Session history records route, time, distance, and newly revealed territory.
8. User can replay an old route.
9. At least several authored zones and sample discoveries make the experience feel game-like.
10. XP/progress changes only for meaningful new exploration.
11. UI feels intentionally designed as an exploration game rather than a generic map utility.

## 11. Deliberately deferred

Not part of the first implementation unless needed for demonstration:
- live friend location
- crews/teams
- competitive leaderboards
- AR camera gameplay
- citywide multiplayer events
- passive 24/7 tracking
- user-generated discoveries
- advanced anti-cheat
- monetization
- expansion outside Islamabad/Rawalpindi

These remain possible later without distorting the first build.

## 12. Product risks to validate early

- Fog reveal must look satisfying enough to make exploration addictive.
- GPS accuracy must not make reveal feel random or unfair.
- Map performance must remain smooth as explored territory grows.
- The app needs enough authored discoveries/zones to feel dense.
- History must feel personally valuable enough to retain users after novelty fades.

## 13. Current decisions

Locked:
- mobile-first
- Islamabad + Rawalpindi only
- exploration-game direction
- fog permanently reveals through verified real movement during active sessions
- strong personal history of when/where the user traveled
- dense authored experience over broad geographic coverage

Open for later iteration:
- final name/brand treatment
- exact map SDK/provider
- exact territory-cell resolution
- progression balancing
- exact zone list
- social mechanics
