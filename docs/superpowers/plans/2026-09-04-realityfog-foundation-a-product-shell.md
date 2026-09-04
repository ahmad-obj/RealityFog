# RealityFog Foundation A — Product Shell Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
>
> **RealityFog execution note:** app implementation is delegated to Bilt AI. This control repository stores the plan/prompt/review trail; code changes happen in `ahmad-obj/realityfog-d46c99`.

**Goal:** Replace the accidental single-screen prototype shell with a deliberate Map / Atlas / Journal product shell and monochrome design system while preserving the currently working fog, GPS, persistence, simulation, and session logic.

**Architecture:** Use Expo Router route groups for the three meaningful current destinations. Keep the existing `FogMap` and exploration stores intact behind the Map route, derive Atlas statistics from the authoritative explored-cell set, and expose completed local exploration records through a small Journal read model. This phase changes information architecture and presentation only; it must not solve map/fog synchronization, background Explorer Mode, auto-Journey segmentation, XP, zones, or collectibles.

**Tech Stack:** React Native 0.81, Expo 54, Expo Router 6, TypeScript 5.9, Zustand 5, HeroUI Native, Uniwind/Tailwind, Lucide React Native, existing AsyncStorage/session stores.

**Spec:** `docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md`

## Global Constraints

- Current visible destinations are exactly **Map**, **Atlas**, and **Journal**.
- **Collection** is architecturally reserved but must not appear as an empty/coming-soon tab.
- Profile/settings must not consume a permanent bottom-navigation destination.
- Map remains the default launch destination and visual/emotional center.
- The visual system is predominantly black / near-black / white / graphite / restrained gray.
- Color is semantic only; remove the current decorative navy/amber/cyan identity from the product shell.
- Preserve current fog engine, chunking/LOD, explored-cell persistence, GPS filtering, session completion reliability, and simulation behavior.
- Do not claim persistent/background Explorer Mode is implemented in this phase. The existing foreground session engine remains the temporary execution mechanism until Foundation C.
- Do not implement Journey auto-segmentation in this phase.
- Do not implement XP/levels, authored zones, discoveries, badges, photo collectibles, camera/CV, social, auth/backend, or Collection UI.
- Do not attempt the map/fog synchronization fix here. That is Foundation B and must be handled through its own technical spike/plan.
- Existing development simulator remains development tooling and must remain clearly labelled.
- All new screens must have meaningful real content derived from current state; no fake statistics or fake history.

---

## File Structure

Target structure in `ahmad-obj/realityfog-d46c99`:

```text
app/
  _layout.tsx                        root providers + root Stack
  (main)/
    _layout.tsx                      three-destination bottom navigation
    index.tsx                        Map destination
    atlas.tsx                        Atlas destination
    journal.tsx                      Journal destination

components/
  navigation/
    RealityBottomNav.tsx             custom restrained Map/Atlas/Journal dock
    ExplorationStatusPill.tsx        global current foreground exploration status
  shell/
    ScreenHeader.tsx                 consistent non-map screen header
    EmptyState.tsx                   restrained meaningful empty state
  atlas/
    CityProgressRow.tsx              city/overall progress presentation
  journal/
    JourneyRow.tsx                   current completed-session row presentation
  hud/
    ExplorationHud.tsx               map-only contextual HUD, simplified
    SessionControls.tsx              current foreground engine control, monochrome

lib/
  atlas/
    progress.ts                      pure city/Twin Cities progress derivation
  journal/
    journalModel.ts                  pure completed-session display model
    journalStore.ts                  loads completed local sessions for Journal
  theme/
    fog.ts                           monochrome SVG/native map constants

global.css                           monochrome semantic/theme tokens

tests/
  atlasProgress.test.ts              city/overall math
  journalModel.test.ts               deterministic history formatting/model
```

Responsibilities must stay separate: route/navigation code does not calculate geography, Atlas presentation does not read AsyncStorage directly, and Journal UI does not mutate session persistence.

---

### Task 1: Create the three-destination Expo Router shell

**Files:**
- Create: `app/(main)/_layout.tsx`
- Create: `app/(main)/index.tsx`
- Create: `app/(main)/atlas.tsx`
- Create: `app/(main)/journal.tsx`
- Create: `components/navigation/RealityBottomNav.tsx`
- Modify: `app/_layout.tsx`
- Remove after migration: `app/index.tsx`

**Interfaces:**
- Produces route paths `/`, `/atlas`, `/journal` through the `(main)` group.
- Produces `RealityBottomNav` with three destinations only: `map`, `atlas`, `journal`.
- Consumes no new domain logic.

- [ ] **Step 1: Move the existing Map screen composition into `app/(main)/index.tsx` without changing its behavior**

Use the current `app/index.tsx` body as the starting point:

```tsx
export default function MapScreen() {
  useEffect(() => {
    void useExplorationStore.getState().hydrate();
  }, []);

  useSessionLifecycle();

  return (
    <View className="bg-rf-black flex-1">
      <StatusBar style="light" />
      <FogMap />
      <ExplorationHud />
    </View>
  );
}
```

Do not alter `FogMap`, location watching, simulation, or reveal logic in this step.

- [ ] **Step 2: Add routable Atlas and Journal screens with real structural shells**

`app/(main)/atlas.tsx` and `app/(main)/journal.tsx` must render stable screen containers and headers but must not invent content. Their data sections are populated in Tasks 4 and 5.

Use this screen contract:

```tsx
export default function AtlasScreen() {
  return <View className="bg-rf-black flex-1">{/* header + real content added in Task 4 */}</View>;
}
```

```tsx
export default function JournalScreen() {
  return <View className="bg-rf-black flex-1">{/* header + real content added in Task 5 */}</View>;
}
```

- [ ] **Step 3: Implement `RealityBottomNav` using Expo Router navigation**

Required destination model:

```ts
export type MainDestination = 'map' | 'atlas' | 'journal';

export const MAIN_DESTINATIONS: ReadonlyArray<{
  id: MainDestination;
  label: 'Map' | 'Atlas' | 'Journal';
  href: '/' | '/atlas' | '/journal';
}>;
```

Use Lucide icons with restrained monochrome treatment. Do not use a giant central FAB, neon selected state, or a fourth disabled Collection item.

- [ ] **Step 4: Wire the `(main)` layout and root Stack**

`app/(main)/_layout.tsx` owns the bottom navigation. `app/_layout.tsx` retains providers/error handling and mounts the main route group without a visible Stack header.

The bottom nav must remain usable on Map, Atlas, and Journal and respect safe-area insets.

- [ ] **Step 5: Run navigation smoke checks**

Run:

```bash
npm run lint
npm run format:check
npm run export:web
```

Expected:
- all three routes compile;
- `/`, `/atlas`, `/journal` render without route-not-found errors;
- Map still mounts the existing fog/session system;
- no Collection route/tab exists.

- [ ] **Step 6: Commit**

```bash
git add app components/navigation

git commit -m "feat: add RealityFog Map Atlas Journal shell"
```

---

### Task 2: Replace the accidental prototype palette with the monochrome design system

**Files:**
- Modify: `global.css`
- Modify: `lib/theme/fog.ts`
- Modify: `app/_layout.tsx`
- Create: `components/shell/ScreenHeader.tsx`
- Create: `components/shell/EmptyState.tsx`
- Modify as needed: `components/navigation/RealityBottomNav.tsx`

**Interfaces:**
- Produces reusable shell tokens/classes with names independent of old `fog-*`, `ember`, and `signal` branding where practical.
- Keeps `FOG_COLORS` exported for native/SVG consumers, but values become monochrome/semantic.

- [ ] **Step 1: Define monochrome semantic tokens**

Create a restrained set in `global.css` equivalent to:

```css
@theme {
  --color-rf-black: #050505;
  --color-rf-elevated: #101010;
  --color-rf-surface: #171717;
  --color-rf-line: #2a2a2a;
  --color-rf-white: #f5f5f3;
  --color-rf-muted: #9a9a96;
  --color-rf-dim: #656561;
  --color-rf-danger: #d65c5c;
  --color-rf-warning: #c9a75e;
}
```

Exact neutral shades may be adjusted slightly for accessibility, but the identity must remain monochrome. Semantic warning/danger colors are allowed only where state meaning requires them.

- [ ] **Step 2: Convert `FOG_COLORS` and native map styling to monochrome**

Required conceptual structure:

```ts
export const FOG_COLORS = {
  world: '#050505',
  fogTop: '#101010',
  fogBottom: '#070707',
  fogCloud: '#202020',
  outside: '#020202',
  worldEdge: '#777773',
  route: '#C8C8C4',
  player: '#F5F5F3',
  playerRing: '#A6A6A0',
  signal: '#F5F5F3',
} as const;
```

Convert `DARK_MAP_STYLE` toward desaturated grayscale cartography. Roads/labels may use multiple neutral luminance levels, but no decorative blue/navy/amber palette.

Do not change fog geometry/masking logic.

- [ ] **Step 3: Make system chrome consistently dark**

`app/_layout.tsx` should use near-black Stack content background matching `--color-rf-black`. Keep Inter for this phase; typography replacement is not required.

- [ ] **Step 4: Implement `ScreenHeader` and `EmptyState`**

Required API:

```ts
export interface ScreenHeaderProps {
  eyebrow?: string;
  title: string;
  subtitle?: string;
  right?: React.ReactNode;
}
```

```ts
export interface EmptyStateProps {
  title: string;
  body: string;
}
```

No card-heavy dashboard styling. Headers should create hierarchy with whitespace and type, not boxes.

- [ ] **Step 5: Verify visual token migration**

Search for old identity usages:

```bash
rg "ember|signal-soft|bg-signal|text-signal|fog-950|fog-900|fog-800" app components lib global.css
```

Expected: remaining matches are either intentionally backwards-compatible implementation names inside fog internals or are removed/replaced from product-shell presentation.

Run:

```bash
npm run lint:css
npm run lint
npm run format:check
```

- [ ] **Step 6: Commit**

```bash
git add global.css lib/theme/fog.ts app/_layout.tsx components/shell components/navigation

git commit -m "feat: establish monochrome RealityFog design system"
```

---

### Task 3: Simplify Map chrome and expose exploration state globally without changing lifecycle semantics

**Files:**
- Create: `components/navigation/ExplorationStatusPill.tsx`
- Modify: `components/hud/ExplorationHud.tsx`
- Modify: `components/hud/SessionControls.tsx`
- Modify: `app/(main)/_layout.tsx`

**Interfaces:**
- Consumes current `useSessionStore` only.
- Produces `ExplorationStatusPill` that reflects the **current implementation's** foreground session state; it must not claim background persistence has been implemented.

Required status model:

```ts
export type ExplorationUiState = 'idle' | 'starting' | 'active' | 'ending' | 'attention';
```

- [ ] **Step 1: Create a pure status selector**

Inside `ExplorationStatusPill.tsx` or a focused adjacent helper, map current store state to UI state:

```ts
export function deriveExplorationUiState(input: {
  status: 'idle' | 'starting' | 'active' | 'ending';
  tracking: boolean;
  notice: string | null;
}): ExplorationUiState {
  if (input.notice && input.status === 'active' && !input.tracking) return 'attention';
  return input.status;
}
```

- [ ] **Step 2: Render restrained global status**

Behavior:
- idle: do not show a global `Exploring` pill on Atlas/Journal;
- starting: `Starting…`;
- active + healthy tracking: `● Exploring`;
- active + tracking problem: `Exploration needs attention`;
- ending: `Stopping…`.

Tapping the pill on Atlas/Journal navigates to `/`.

Do **not** add `background active` copy in this phase.

- [ ] **Step 3: Simplify `ExplorationHud`**

Map chrome should contain only immediate context:
- current city/coverage status;
- new-territory transient feedback;
- current exploration control/state;
- development controls only in development.

Remove visual clutter and decorative colored panels. Do not add Atlas/Journal statistics to Map.

- [ ] **Step 4: Convert `SessionControls` to monochrome copy/appearance without changing engine methods**

Continue calling exactly:

```ts
useSessionStore.getState().startSession();
useSessionStore.getState().endSession();
```

User-facing copy may use `Start Exploring` / `Stop Exploring`, but completion/history semantics remain the current session system until Foundation C.

Do not add background permissions here.

- [ ] **Step 5: Verify no lifecycle regression**

Manual checks:
1. Start current exploration from Map.
2. Navigate to Atlas and Journal while it remains active.
3. Confirm global pill reflects active state.
4. Tap status pill and return to Map.
5. Stop exploration from Map.
6. Confirm status disappears on other tabs.
7. Run simulator and confirm DEV labelling remains unmistakable.

Run:

```bash
npm test
npm run lint
npm run format:check
```

- [ ] **Step 6: Commit**

```bash
git add components/navigation components/hud app/'(main)'/_layout.tsx

git commit -m "feat: unify exploration status across product shell"
```

---

### Task 4: Build Atlas from real explored-cell data

**Files:**
- Create: `lib/atlas/progress.ts`
- Create: `components/atlas/CityProgressRow.tsx`
- Modify: `app/(main)/atlas.tsx`
- Test: `tests/atlasProgress.test.ts`
- Modify if required for exported helpers only: `lib/exploration/world.ts`

**Interfaces:**
- Consumes `exploredCells: Set<string>` from `useExplorationStore`.
- Consumes `cellToLocation`, `cityForLocation`, and city/world playable-cell totals.
- Produces:

```ts
export interface GeographicProgress {
  islamabad: { explored: number; total: number; percent: number };
  rawalpindi: { explored: number; total: number; percent: number };
  twinCities: { explored: number; total: number; percent: number };
}

export function calculateGeographicProgress(exploredCells: ReadonlySet<string>): GeographicProgress;
```

- [ ] **Step 1: Write failing tests for separate city progress**

`tests/atlasProgress.test.ts` must cover:
- empty explored set returns 0% for all three views;
- one Islamabad cell increments Islamabad + Twin Cities but not Rawalpindi;
- one Rawalpindi cell increments Rawalpindi + Twin Cities but not Islamabad;
- duplicate cell keys cannot inflate counts;
- percentages are bounded `[0, 100]`.

- [ ] **Step 2: Run the focused test and confirm failure before implementation**

```bash
node --import tsx --test tests/atlasProgress.test.ts
```

Expected: FAIL because `calculateGeographicProgress` does not exist.

- [ ] **Step 3: Implement per-city playable totals and progress calculation**

If `world.ts` does not expose per-city totals, add:

```ts
export function totalPlayableCellsForCity(cityId: PlayableCity['id']): number;
```

Cache totals using the existing playable-cell iteration model; do not recompute the full world on every React render.

`calculateGeographicProgress()` should classify only valid playable explored cells and return deterministic values.

- [ ] **Step 4: Run tests to pass**

```bash
node --import tsx --test tests/atlasProgress.test.ts
```

Expected: PASS.

- [ ] **Step 5: Build the Atlas overview UI from this real data**

`app/(main)/atlas.tsx` must display:
- `Atlas` header;
- Islamabad progress;
- Rawalpindi progress;
- Twin Cities overall progress;
- a restrained `Areas` section explaining that named authored areas will appear once area data is implemented, without fabricating area names/statuses in code yet.

The empty state must be meaningful, e.g. if nothing is explored: `Your atlas begins with your first explored territory.`

Do not create fake `46 areas discovered` values.

- [ ] **Step 6: Run full checks**

```bash
npm test
npm run lint
npm run format:check
```

- [ ] **Step 7: Commit**

```bash
git add lib/atlas lib/exploration/world.ts components/atlas app/'(main)'/atlas.tsx tests/atlasProgress.test.ts

git commit -m "feat: add real city progress to Atlas"
```

---

### Task 5: Build Journal from real completed local exploration records

**Files:**
- Create: `lib/journal/journalModel.ts`
- Create: `lib/journal/journalStore.ts`
- Create: `components/journal/JourneyRow.tsx`
- Modify: `app/(main)/journal.tsx`
- Test: `tests/journalModel.test.ts`

**Interfaces:**
- Consumes existing `loadCompletedSessions(): Promise<ExplorationSession[]>` from `lib/storage/sessionStorage.ts`.
- Must not mutate session history.
- Produces:

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

export function toJournalEntry(session: ExplorationSession): JournalEntry | null;
```

`journalStore.ts` should expose:

```ts
interface JournalState {
  loading: boolean;
  entries: JournalEntry[];
  error: string | null;
  refresh: () => Promise<void>;
}
```

- [ ] **Step 1: Write failing tests for the pure Journal model**

Cover:
- incomplete session with `endedAt === null` returns `null`;
- completed session derives correct duration;
- repeated `newlyRevealedCells` are counted uniquely;
- source is preserved;
- distance is clamped to a non-negative display value.

- [ ] **Step 2: Run focused test and confirm failure**

```bash
node --import tsx --test tests/journalModel.test.ts
```

Expected: FAIL because `toJournalEntry` does not exist.

- [ ] **Step 3: Implement `toJournalEntry`**

Keep formatting out of the persisted domain model. The Journal model may derive display-safe numbers, but it must not alter stored session data.

- [ ] **Step 4: Implement read-only Journal store**

`refresh()` loads completed sessions, maps through `toJournalEntry`, drops nulls, sorts newest-first by `startedAt`, and returns controlled load errors.

Do not add a second history persistence format.

- [ ] **Step 5: Build Journal UI**

Journal screen must:
- load on first mount;
- refresh when the screen regains focus or via explicit pull/refresh behavior supported by the current component choices;
- show actual completed records;
- distinguish simulated entries visibly;
- show a meaningful empty state when no completed record exists.

A `JourneyRow` should show only currently truthful data: date/time, distance, duration, new territory proxy, and simulated/real source where relevant.

Do not claim automatic Journey segmentation exists yet. These rows are current completed exploration records presented inside the future Journal surface.

- [ ] **Step 6: Run tests/checks**

```bash
npm test
npm run lint
npm run format:check
```

- [ ] **Step 7: Commit**

```bash
git add lib/journal components/journal app/'(main)'/journal.tsx tests/journalModel.test.ts

git commit -m "feat: surface local exploration history in Journal"
```

---

### Task 6: Foundation A integration and regression gate

**Files:**
- Modify only files required to resolve integration findings from Tasks 1–5.
- Do not expand scope into Foundation B/C/D.

**Interfaces:**
- No new public domain interfaces unless an integration failure demonstrates a specific need.

- [ ] **Step 1: Run complete automated verification**

```bash
npm test
npm run lint
npm run lint:css
npm run format:check
npm run expo-check
npm run export:web
```

Every command must be reported with its actual exit result. If a command fails because of the Bilt environment, report the exact failure; do not claim success.

- [ ] **Step 2: Manual product-shell checklist**

Verify:

1. App launches to **Map**.
2. Bottom navigation has exactly **Map / Atlas / Journal**.
3. No visible empty Collection tab exists.
4. Map retains fog/reveal/GPS/session/dev-simulator behavior from before this phase.
5. Atlas shows real separate Islamabad / Rawalpindi / Twin Cities progress.
6. Journal shows real completed local exploration records or a truthful empty state.
7. Active current foreground exploration remains active while switching between current routes as allowed by current runtime lifecycle.
8. Atlas/Journal can return to Map through navigation/status.
9. UI is predominantly monochrome and no longer reads as navy/amber/cyan gamer UI.
10. No fake badges, areas, XP, discoveries, social content, or fake statistics exist.
11. The known fog/map gesture desynchronization is **not** disguised as fixed; it remains a Foundation B blocker.
12. Background tracking is **not** represented as implemented yet.

- [ ] **Step 3: Physical/native preview screenshots/evidence**

Capture at least:
- Map idle;
- Map current exploration active;
- Atlas with actual state;
- Journal with actual state or real empty state;
- bottom navigation on one non-Map screen.

These are review evidence, not marketing assets.

- [ ] **Step 4: Commit any integration-only corrections**

```bash
git add app components lib tests global.css

git commit -m "fix: integrate RealityFog Foundation A shell"
```

Skip this commit if no integration correction was required after Task 5.

---

## Foundation A Completion Criteria

Foundation A passes only if all are true:

- real three-destination navigation exists;
- Map remains functional;
- Atlas has real separate city/overall coverage rather than fake dashboard numbers;
- Journal exposes real local completed records;
- monochrome visual system is materially applied across shell/HUD;
- current exploration status is understandable across destinations;
- no future feature is represented as if it already works;
- no known map/fog synchronization or background-location limitation is hidden;
- tests/lint/format/build evidence is returned for review.

## What Comes Next

Do **not** proceed directly to XP/zones after this plan.

Next separate plans, in order unless later evidence changes it:

1. **Foundation B — Map Architecture Spike:** determine and prove the map/rendering approach that keeps fog spatially synchronized during live gestures and resolves provider/API-key/build behavior.
2. **Foundation C — Persistent Explorer Mode:** background location, explicit durable ON/OFF state, platform transparency, and Journey segmentation design/implementation.
3. **Foundation D — Exploration Rule Hardening:** reconcile GL-001 through GL-009 as appropriate before permanent progression math.
4. Only after those gates: authored areas/zones, progression, discoveries, richer Journal, account sync, and future Collection.

## Bilt Review Protocol

After Bilt executes this plan/prompt, return:

```text
Foundation: A — Product Shell
Bilt result: completed / partial / failed
Commit SHA: ...
Commands run + exact results:
- npm test: ...
- npm run lint: ...
- npm run lint:css: ...
- npm run format:check: ...
- npm run expo-check: ...
- npm run export:web: ...
Screenshots/preview evidence:
- ...
Known limitations retained intentionally:
- map/fog gesture synchronization
- persistent background Explorer Mode not yet implemented
Unexpected behavior:
- ...
```

Never advance from Bilt's completion claim alone; inspect the actual repository diff and runtime evidence.