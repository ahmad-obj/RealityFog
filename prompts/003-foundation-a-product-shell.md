# Bilt Prompt 003 — Foundation A: Product Shell + Monochrome Navigation

Continue from the current implementation repository:

`ahmad-obj/realityfog-d46c99`

Current expected HEAD before this work:

`7e5cc087c4b9a46e5f176079050c58dc84ddadb0`

If HEAD has moved, inspect the newer commits first and preserve legitimate newer work. Do not blindly reset or overwrite changes.

This prompt implements **Foundation A only** from the revised RealityFog product architecture.

Read and follow these controlling documents conceptually:

- navigation/UX spec: `docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md`
- Foundation A plan: `docs/superpowers/plans/2026-09-04-realityfog-foundation-a-product-shell.md`
- game-logic staging: `docs/GAME_LOGIC_BACKLOG.md`

The old straight-line `GPS → XP → Zones → History` implementation sequence is retired.

## Objective

Replace the accidental single-screen prototype shell with an intentional, usable product shell:

- **Map** — explore the world;
- **Atlas** — understand geographic exploration progress;
- **Journal** — see real completed local exploration history.

At the same time, replace the current navy/amber/cyan prototype look with RealityFog's approved predominantly **black / white / graphite / restrained gray** visual direction.

The result should feel like a premium cartographic exploration product, not a generic Expo template, SaaS dashboard, or gamer/cyberpunk UI.

## Preserve these working systems

Do not rewrite or regress:

- fog masking/reveal engine;
- explored-cell authority;
- fog chunking/culling/LOD work;
- GPS filtering;
- session persistence/recovery;
- finalized-session retry logic;
- completed-session local history;
- development simulation behavior;
- existing tests unless a test must change because presentation/routing moved.

The known map/fog gesture synchronization problem is **not** part of this prompt. Do not disguise it as fixed. Foundation B will address the map architecture separately.

## Scope 1 — Expo Router product shell

Create a real three-destination shell using Expo Router.

Preferred structure:

```text
app/
  _layout.tsx
  (main)/
    _layout.tsx
    index.tsx       # Map
    atlas.tsx       # Atlas
    journal.tsx     # Journal
```

Move the current Map composition into the Map route without changing its domain behavior.

Visible bottom navigation must contain exactly:

- Map
- Atlas
- Journal

Do **not** add a dead/disabled Collection tab yet. Collection is reserved architecturally for future badges/photo finds/awards, but it must only enter visible navigation when real content exists.

Do not make Profile/Settings a permanent bottom tab.

Use a restrained custom navigation treatment with safe-area support. No giant center FAB, no neon selected state, no oversized card dock.

## Scope 2 — Monochrome product system

Replace the decorative prototype palette across the shell/HUD with a predominantly monochrome system.

Target character:

- near-black background;
- black/graphite surfaces;
- white/off-white typography;
- restrained neutral gray hierarchy;
- desaturated map/cartography;
- dark dense fog;
- semantic warning/error color only when state meaning requires it.

Remove the visual identity of decorative navy + amber + cyan from user-facing shell components.

`lib/theme/fog.ts` may keep its existing export names for compatibility, but values should become monochrome/semantic.

Do not change fog geometry, projection, masks, cell logic, LOD math, or reveal authority while recoloring.

Keep Inter for now. Do not spend this phase searching for a new font.

Avoid:

- generic cards everywhere;
- gradients/glows as decoration;
- cyberpunk styling;
- colorful gamer HUD;
- giant dashboards;
- unnecessary floating panels over the map.

## Scope 3 — Global current exploration status

The approved target product eventually uses persistent background **Explorer Mode**, but that lifecycle is **NOT implemented in Foundation A**.

For this prompt:

- keep the current foreground session engine intact;
- expose its current state consistently across Map / Atlas / Journal;
- while the current foreground exploration is active, Atlas/Journal may show a restrained `● Exploring` status that returns to Map when tapped;
- if current tracking needs attention, show that honestly;
- do not claim background tracking or persistent Explorer Mode works yet;
- do not add background location permissions/services in this prompt.

Map controls may use more product-like copy such as `Start Exploring` / `Stop Exploring`, but they must still call the current session-store start/end methods and must not alter persistence semantics.

## Scope 4 — Atlas with real data only

Build a meaningful Atlas screen from the existing explored-cell authority.

Atlas must calculate and show separately:

- **Islamabad explored %**
- **Rawalpindi explored %**
- **Twin Cities overall explored %**

Do not display one city's label with the combined two-city denominator.

Implement a pure calculation layer, e.g. conceptually:

```ts
export interface GeographicProgress {
  islamabad: { explored: number; total: number; percent: number };
  rawalpindi: { explored: number; total: number; percent: number };
  twinCities: { explored: number; total: number; percent: number };
}

export function calculateGeographicProgress(
  exploredCells: ReadonlySet<string>,
): GeographicProgress;
```

If `world.ts` needs a cached per-city playable-cell total helper, add a focused helper rather than recomputing the entire world every render.

Add tests covering:

- zero-state;
- Islamabad-only explored cell;
- Rawalpindi-only explored cell;
- overall total;
- duplicate cells;
- percentages bounded to 0–100.

### Named areas

Do not fabricate authored zone/area progress yet.

Atlas may reserve a visually meaningful `Areas` section explaining that named exploration regions will appear when authored area data is introduced later, but it must not invent fake area percentages, completion counts, or random sample statistics.

No XP, levels, discoveries, badges, or fake achievements.

## Scope 5 — Journal using real completed local history

Build Journal from the actual completed sessions already stored by the app.

Create a read-only Journal model/store rather than reading AsyncStorage directly inside UI components.

Conceptual model:

```ts
export interface JournalEntry {
  id: string;
  startedAt: number;
  endedAt: number;
  durationMs: number;
  distanceM: number;
  newCellCount: number;
  source: 'real' | 'simulated';
}
```

Rules:

- incomplete records with `endedAt === null` are not Journal entries;
- completed entries are newest-first;
- simulated source remains visibly distinguishable;
- use real stored distance/time/new-cell data only;
- do not create a second history persistence format;
- do not mutate session history from Journal;
- if no history exists, show a polished truthful empty state.

Do not claim automatic Journey segmentation exists yet. Current completed exploration records are being surfaced inside the future Journal destination until Foundation C replaces the explicit short-session lifecycle.

Add pure tests for Journal entry derivation.

## Scope 6 — Map/HUD cleanup

Keep Map dominant.

Map should show only immediate context:

- current geographic/exploration status;
- current exploration control/status;
- transient new-territory feedback;
- current player/fog world;
- clearly labelled development tools in development.

Do not copy Atlas statistics or Journal content onto Map.

Do not remove functionality merely to make it look minimal.

## Explicitly DO NOT implement

Do not add or solve in this prompt:

- persistent background Explorer Mode;
- background location permissions/service;
- auto-Journey segmentation;
- map-engine/provider replacement;
- fog/map synchronization architecture fix;
- Google Maps API-key architecture beyond preserving current behavior;
- smaller reveal radius;
- route interpolation changes;
- travel-mode rules;
- GPS-acquisition redesign;
- XP/levels;
- authored areas/zones;
- discoveries;
- Collection UI;
- badges/awards;
- photo collectibles;
- camera/CV verification;
- social;
- auth/backend/account sync;
- fake demo statistics.

## Automated verification

Run the actual project commands:

```bash
npm test
npm run lint
npm run lint:css
npm run format:check
npm run expo-check
npm run export:web
```

If any command is unavailable or fails due to environment, report the exact command, exit/failure, and reason. Do not summarize a failed command as passing.

## Manual acceptance checks

Verify and report:

1. App launches to Map.
2. Bottom navigation has exactly Map / Atlas / Journal.
3. No visible Collection placeholder tab exists.
4. Map still runs the existing fog/reveal/session/simulator systems.
5. Atlas shows real separate Islamabad / Rawalpindi / Twin Cities progress.
6. Journal shows real completed local history or a truthful real empty state.
7. Current active foreground exploration state is visible across destinations without claiming background tracking.
8. Navigation returns cleanly to Map.
9. Product shell is materially monochrome rather than navy/amber/cyan.
10. No fake XP, zones, discoveries, badges, achievements, social, or statistics were introduced.
11. Known map/fog gesture desynchronization is still documented as unresolved.
12. Background Explorer Mode is still documented as not implemented.

## Completion report

Return:

1. exact commit SHA;
2. files added/modified/removed;
3. route/navigation architecture used;
4. exact visual-system changes;
5. Atlas progress calculation approach;
6. Journal read-model/storage approach;
7. how current exploration status is surfaced across tabs;
8. tests added/changed;
9. exact results for all six verification commands;
10. screenshots/preview evidence for Map, active Map, Atlas, Journal, and one non-Map navigation state if Bilt can provide them;
11. known limitations retained intentionally;
12. anything that deviated from this prompt and why.

Do not proceed into Foundation B/C/D or XP automatically after completing this work.