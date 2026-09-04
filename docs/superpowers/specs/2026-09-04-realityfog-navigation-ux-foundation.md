# RealityFog — Navigation & UX Foundation

Date: 2026-09-04
Status: Ready for user review
Scope: Product shell, navigation, persistent Explorer Mode, information architecture, visual direction, and implementation gates

> This document supersedes earlier navigation and exploration-lifecycle assumptions in `docs/superpowers/specs/2026-09-04-reality-fog-design.md` where they conflict.
>
> Gameplay details such as reveal radius, travel-mode fairness, revisit value, GPS acquisition rules, distance semantics, and zone goals remain governed by `docs/GAME_LOGIC_BACKLOG.md`.

## 1. Product model

RealityFog has four long-term product jobs:

1. **Map — Explore the world.**
2. **Atlas — Understand the world you have uncovered.**
3. **Journal — Remember your movement through it.**
4. **Collection — Find and earn things hidden inside it.**

Explorer Mode sits above all of them as a global state.

The product should not feel like a fitness tracker that requires the user to start and stop a short activity every time they move. It should feel like turning on an exploration layer for real life.

## 2. Explorer Mode — global persistent state

### Core rule

Explorer Mode is controlled explicitly by the user:

- **OFF** — RealityFog does not build new exploration history or reveal territory.
- **ON** — RealityFog continues legitimate exploration tracking until the user explicitly turns it off, including while the app is backgrounded where the operating system permits.

Explorer Mode must not silently stop merely because the user changes tabs, locks the phone, or switches apps.

This is not passive tracking enabled by default. The user deliberately enables Explorer Mode and can always see that it is active.

### Global visibility

When Explorer Mode is ON, every major app destination exposes a restrained persistent status such as:

`● Exploring`

Tapping it returns to the Map or opens the Explorer Mode status/control sheet.

The active state must never be visually hidden from the user.

### Explorer Mode control sheet

When ON, tapping the status may show concise information such as:

- exploration active;
- today's legitimate travel;
- today's new territory;
- areas visited;
- GPS/location state;
- `Pause / Stop Exploring`.

Stopping should be an intentional action rather than a large button constantly demanding attention.

### Background-location product expectation

The target product requires background location while Explorer Mode is ON.

Implementation must account for platform rules rather than pretending background tracking is guaranteed forever:

- background permission must be explicit;
- Android may require a foreground location service/notification depending on implementation and OS version;
- iOS background location requires appropriate background capability/permission;
- background location requires a development/native build rather than relying on Expo Go for validation;
- force-killing/terminating the app can stop location delivery, with behavior differing by platform/device vendor;
- the durable Explorer Mode preference should survive app restart so the app can recover/resume appropriately when execution is available again.

The product must be honest about these states rather than showing `Exploring` when the OS has made reliable tracking impossible.

### Privacy principle

Explorer Mode being persistent makes transparency more important, not less:

- explicit opt-in;
- explicit active indicator;
- clear permission explanation;
- location history private by default;
- one obvious way to stop tracking;
- no future live-location sharing without a separate explicit action.

## 3. Journeys are not Explorer Mode

Explorer Mode may remain ON for days or weeks. Therefore the app must not store that entire period as one giant "session."

Instead, **Journeys** are automatically segmented history records created beneath Explorer Mode.

Conceptually:

```text
Explorer Mode ON
├── Journey A — morning movement
├── long stationary/inactive gap
├── Journey B — afternoon movement
├── Journey C — evening movement
└── Explorer Mode still ON
```

A Journey can eventually contain:

- start/end time;
- route;
- total journey distance;
- playable-world distance;
- new-exploration contribution;
- areas entered;
- discoveries/collectibles encountered later;
- first visits;
- photos/memories later.

### Journey segmentation is OPEN DESIGN

Do not invent final thresholds yet.

Possible signals include:

- meaningful movement beginning;
- sustained inactivity;
- large time gaps;
- day boundaries;
- trusted GPS availability;
- future activity/transport recognition.

The segmentation rule must be designed before replacing the current explicit short-session data model.

## 4. Navigation architecture

### Long-term destinations

The complete information architecture reserves four destinations:

- **Map**
- **Atlas**
- **Journal**
- **Collection**

### Current development shell

Do not ship an empty fourth tab merely to reserve space.

Current implementation should expose only destinations that have meaningful content:

- Map
- Atlas
- Journal

Collection is reserved architecturally and added to visible navigation only when its first real feature is ready.

### Profile and settings

Profile/settings is **not** a permanent bottom-navigation destination.

Access it from a compact top-level control/avatar and include later:

- account;
- privacy;
- Explorer Mode/location permissions;
- data/export;
- appearance;
- account sync/settings.

### Contextual surfaces

Use bottom sheets/overlays for contextual details that do not deserve permanent navigation:

- area preview;
- discovery preview;
- GPS state;
- Explorer Mode control;
- map item detail;
- quick filters/actions.

Avoid creating a new tab every time the product gains a feature.

## 5. Map — Explore the world

Map is the default launch destination and remains the emotional center of RealityFog.

It answers:

> Where am I, what have I uncovered, and where can I explore next?

### Core content

- fog/unexplored world;
- explored territory;
- current position;
- current named area;
- Explorer Mode state;
- nearby unexplored territory;
- future nearby discoveries/collectibles;
- minimal recenter/map controls.

### When Explorer Mode is OFF

The primary exploration action should be obvious without dominating the map:

`Start Exploring`

Starting Explorer Mode may require permission/setup flows, including future background-location permission.

### Acquiring location

Starting Explorer Mode must not immediately imply trustworthy permanent reveal.

The product needs an explicit acquisition state such as:

`Acquiring location…`

The exact GPS lock rule remains GL-006 in `docs/GAME_LOGIC_BACKLOG.md` and must not be guessed during UI implementation.

### When Explorer Mode is ON

Map emphasizes:

- active exploration state;
- player position;
- current area;
- new territory feedback;
- concise relevant live information.

Do not turn the Map into a dashboard.

## 6. Atlas — Understand the world you have uncovered

Atlas replaces the vague concept of a generic Progress dashboard.

It is the geographical record of the player's discovered world.

It answers:

> What parts of Islamabad and Rawalpindi have I uncovered, and what should I explore next?

### Atlas overview

Core high-level metrics:

- Islamabad explored %;
- Rawalpindi explored %;
- Twin Cities overall explored %;
- areas discovered;
- areas explored;
- areas mastered.

This directly follows GL-003 and GL-008.

### Named areas

Atlas must expose named geographic areas rather than only anonymous percentage coverage.

Examples may eventually include:

- F-6;
- F-7;
- Blue Area;
- Margalla regions;
- Saddar;
- old Rawalpindi;
- other authored sectors/districts/regions.

Each area should eventually preserve:

- name;
- city/parent geography;
- exploration percentage;
- first visited;
- last visited;
- visit count;
- associated journeys;
- discoveries/collectibles later;
- completion state.

### Area hierarchy

Conceptually:

```text
Atlas
├── Overview
├── Islamabad
│   ├── sectors / districts / special regions
├── Rawalpindi
│   ├── areas / districts / special regions
└── My Areas
    ├── Discovered
    ├── Explored
    └── Mastered
```

Exact geographic taxonomy must be authored deliberately rather than generated casually by Bilt.

### Area states

Reserve the progression vocabulary:

`Unknown → Discovered → Explored → Mastered`

The semantic idea is approved; exact thresholds/rules are not yet locked.

### Area detail

A future area detail can contain:

- area-specific map;
- explored percentage;
- unexplored pockets;
- first/last visit;
- recent journeys;
- discoveries;
- collectibles;
- `Open on Map`.

## 7. Journal — Remember your movement

Journal is the personal-history half of RealityFog.

It answers:

> What did I actually do, where did I go, and how has my real-world history accumulated?

### Journal timeline

Automatically segmented Journeys appear chronologically, for example:

```text
Today
  F-7 → Blue Area
  3.4 km · 42 min · new territory

  FAST → F-10
  8.7 km · 31 min · areas visited
```

The user should not have to manually create each Journey.

### Journey detail

Eventually contains:

- route map;
- route replay;
- date/time;
- total journey distance;
- playable-world distance;
- new exploration contribution;
- areas entered;
- first visits;
- discoveries/collectibles;
- future photos/memories.

This directly follows GL-007.

### Journal statistical role

Journal owns personal-history statistics such as:

- total travel while Explorer Mode was active;
- exploration distance;
- active exploration days;
- journey count;
- longest journey;
- first-time areas;
- new territory by week/month.

Do not duplicate all of these into one giant global dashboard.

## 8. Collection — reserved future destination

Collection has a permanent place in the product architecture but is **not part of the current implementation phase**.

It will eventually answer:

> What rare things, awards, badges, and physical-world finds have I earned?

Possible sections later:

- badges;
- photo finds;
- area awards;
- rare discoveries;
- completed sets;
- special/seasonal events.

### Location + photo collectibles

A future collectible may require the player to:

1. reach a specific real-world location;
2. find a specified physical subject/view/feature;
3. capture a photo;
4. satisfy location plus visual verification;
5. unlock a badge/award/collectible.

Potential verification can later combine:

- GPS/geofence confidence;
- camera/photo evidence;
- computer-vision recognition;
- authored composition/subject rules.

This is intentionally **reserved, not implemented now**.

Do not build placeholder badge logic, fake collectible data, camera verification, or a dead `Coming Soon` tab in the next phase.

## 9. Statistics architecture

Statistics should live where they answer a user question rather than accumulating in a generic dashboard.

### Atlas — geographic statistics

- Islamabad %;
- Rawalpindi %;
- Twin Cities %;
- unique explored territory;
- areas discovered/explored/mastered;
- zone/area progress.

### Journal — personal-history statistics

- travel distance;
- exploration distance;
- time spent exploring;
- journey count;
- active days;
- monthly movement/history;
- first visits.

### Collection — collectible statistics later

- badge count;
- photo finds;
- sets completed;
- rare finds;
- event awards.

### Map — only immediate context

Map should show only information relevant to what the user is doing now.

## 10. Visual direction

The current navy/amber/cyan prototype palette is not the target visual identity.

### Locked direction

RealityFog should be predominantly monochrome:

- black / near-black;
- white / off-white;
- graphite;
- restrained gray;
- desaturated cartography;
- dense black fog;
- crisp white typography.

Color is reserved for semantic meaning rather than decoration, such as:

- warning/error;
- location/GPS state where necessary;
- future rare discovery/event states.

### Character

Aim for:

- premium cartographic object;
- mysterious;
- precise;
- quiet;
- modern;
- game-like through interaction and progression, not gamer visual clichés.

Avoid:

- generic SaaS cards;
- neon cyberpunk;
- excessive gradients/glows;
- default Material-looking dashboard UI;
- large floating panels obscuring the map;
- decorative color with no meaning.

### Bottom navigation

Use a restrained compact bottom navigation treatment.

Current meaningful destinations:

`Map · Atlas · Journal`

Future:

`Map · Atlas · Journal · Collection`

Do not create a giant center FAB by default.

## 11. Map/fog interaction is a foundation gate

The current implementation has a known architectural interaction problem: the native base map moves immediately during gestures while the separate React/SVG fog overlay derives its transform through region updates, producing visible lag/desynchronization during active pan/zoom.

This must be solved before visual polish or progression is layered on top.

### Required product outcome

During pan, pinch, zoom, animation, and camera movement:

- fog and map must behave as one spatial surface;
- revealed territory must remain visually locked to geography;
- no obvious catch-up/snap after finger release;
- performance must remain acceptable as explored territory grows.

### Technology decision

Do not blindly keep or replace the current map stack.

The next technical work should first evaluate whether:

1. the current `react-native-maps` architecture can host/synchronize the fog in a truly map-native way; or
2. a different map/rendering architecture better supports RealityFog's custom spatial layer requirements.

The decision must be based on a focused prototype/technical spike, not preference.

### Map provider/configuration

The current Android map/API-key behavior must also be treated as a foundation requirement. Production/development native builds using Google Maps require deliberate provider/API-key configuration and should not surface raw provider error messaging to users.

## 12. UX states that must eventually be designed

Explorer Mode and the Map must distinguish at least:

- Explorer Mode OFF;
- permission setup required;
- acquiring trustworthy location;
- Explorer Mode ON + tracking healthy;
- GPS degraded/unreliable;
- background tracking active;
- OS/permission prevents reliable background tracking;
- outside playable geography;
- app resumed after interruption/termination;
- user intentionally stops Explorer Mode.

The system must never claim healthy exploration when permanent reveal cannot be trusted.

## 13. Current implementation vs target architecture

The current Bilt implementation is useful prototype work, not the final product shell.

Keep what has genuine value:

- fog/explored-territory model;
- GPS filtering work;
- persistence/recovery work;
- fog scaling/chunking/LOD experiments;
- source tests.

But do not preserve accidental product decisions simply because code already exists:

- explicit foreground-only short sessions are superseded by persistent Explorer Mode + auto-Journey direction;
- the single-route app shell is superseded by Map/Atlas/Journal architecture;
- current navy/amber visual treatment is superseded by monochrome direction;
- current overlay synchronization is not accepted as the final map architecture.

## 14. Implementation gates and order

Do not return to the old `GPS → XP → Zones → History` sequence.

The revised order is:

### Foundation A — product shell

- navigation structure;
- Map / Atlas / Journal responsibilities;
- global Explorer Mode indicator/control;
- monochrome design system primitives.

### Foundation B — map architecture

- map provider/configuration;
- solve continuous fog/map camera synchronization;
- validate physical-device interaction/performance;
- decide map stack through evidence.

### Foundation C — persistent exploration lifecycle

- foreground + background Explorer Mode;
- transparent permissions/status;
- durable ON/OFF preference;
- transition from explicit short sessions toward automatic Journeys;
- journey segmentation design before implementation.

### Foundation D — staged exploration rules

Reconcile relevant items from `docs/GAME_LOGIC_BACKLOG.md`, including:

- GL-001 smaller reveal radius;
- GL-002 continuous segment reveal;
- GL-003 separate city/Twin Cities progress;
- GL-004 travel-mode fairness — OPEN DESIGN;
- GL-005 revisit value — OPEN DESIGN;
- GL-006 initial GPS trust — OPEN DESIGN;
- GL-007 distance semantics;
- GL-008 zone goals.

### Only then expand gameplay

- authored areas/zones;
- progression/XP;
- discoveries;
- richer Journal;
- profile/account sync;
- Collection later;
- polish/hardening.

## 15. Explicit non-goals for the next implementation phase

Do not implement yet:

- collectible/badge/photo-find system;
- camera/CV verification;
- social features;
- leaderboards;
- AR;
- broad XP economy;
- giant discovery dataset;
- account/backend migration unless required by a foundation decision;
- expansion beyond Islamabad/Rawalpindi.

## 16. Open design decisions retained intentionally

The following are not silently resolved by this UX spec:

- exact journey auto-segmentation rules;
- exact GPS acquisition/lock rule;
- exact travel-mode fairness mechanics;
- exact revisit-value mechanics;
- exact area-state thresholds for Discovered/Explored/Mastered;
- exact map SDK/provider;
- exact final reveal radius/cell resolution;
- exact Collection mechanics/verification model;
- exact progression/XP economy.

These must be decided deliberately at the phase where they become necessary.