# RealityFog — Progress

Last updated: 2026-09-04

## Current phase

**UX/navigation foundation approved; Foundation A product-shell plan and Bilt Prompt 003 are ready for execution**

## Repositories

- Control/docs/prompts: `ahmad-obj/RealityFog`
- Bilt implementation: `ahmad-obj/realityfog-d46c99`
- Reviewed Phase 1 commit: `344697a5491ed3907e7858a6c6a96b76172857a0`
- Reviewed Phase 2 commit: `6a19f90266d56ac5ead07f409a83daa72fcceadb`
- Fog-scaling commit: `7a86991cf15c58fc80de775cd744867262cb934f`
- Persistence-correction/current implementation HEAD before Foundation A: `7e5cc087c4b9a46e5f176079050c58dc84ddadb0`

## Authoritative product docs

Before every new Bilt prompt, reconcile:

1. `docs/superpowers/specs/2026-09-04-reality-fog-design.md` — overall thesis/product scope;
2. `docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md` — current navigation, Explorer Mode, visual and foundation architecture;
3. `docs/DECISIONS.md` — locked decisions;
4. `docs/GAME_LOGIC_BACKLOG.md` — staged/open gameplay logic;
5. latest relevant implementation plan/review and actual Bilt source.

Where the older product spec conflicts with the newer UX/navigation foundation on exploration lifecycle or navigation, the newer UX/navigation foundation controls.

## Completed

- [x] Product concept/spec/decisions established.
- [x] Prompt 001 executed and code-reviewed.
- [x] Genuine subtractive fog engine, spatial explored-cell state, simulation and persistence verified in source.
- [x] Prompt 002 executed and code-reviewed.
- [x] Foreground GPS filtering/session prototype verified as useful engine work.
- [x] Bilt added fog-render scaling work with chunking/culling/LOD.
- [x] Bilt implemented recoverable completion and removed the 100-session history cap in commit `7e5cc087...`.
- [x] Created living gameplay staging document: `docs/GAME_LOGIC_BACKLOG.md`.
- [x] User approved revised product structure: Map / Atlas / Journal, with Collection reserved for future real content.
- [x] User approved persistent global Explorer Mode as target product behavior.
- [x] User approved monochrome black/white/graphite visual direction.
- [x] Created and user-approved `docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md`.
- [x] Created Foundation A implementation plan: `docs/superpowers/plans/2026-09-04-realityfog-foundation-a-product-shell.md`.
- [x] Created Bilt Prompt 003: `prompts/003-foundation-a-product-shell.md`.

## Living game-logic backlog

Authoritative file:

`docs/GAME_LOGIC_BACKLOG.md`

Tracked items include:

- GL-001 smaller reveal radius — LOCKED DIRECTION
- GL-002 continuous segment/path reveal — LOCKED DIRECTION
- GL-003 separate Islamabad / Rawalpindi / Twin Cities progress — LOCKED DIRECTION
- GL-004 travel-mode fairness — OPEN DESIGN
- GL-005 revisit value without normal XP farming — OPEN DESIGN
- GL-006 trustworthy GPS acquisition before first permanent reveal — OPEN DESIGN
- GL-007 journey vs playable-world vs new-exploration distance — LOCKED DIRECTION
- GL-008 zones as primary achievable exploration goals — LOCKED DIRECTION / IMPLEMENT LATER
- GL-009 automatic Journey segmentation beneath persistent Explorer Mode — OPEN DESIGN

Bilt must not silently decide OPEN DESIGN items during unrelated phases.

## Current implementation status

The implementation repo contains useful engine/prototype work:

- fog and explored-territory state;
- GPS filtering;
- persistence/recovery;
- fog chunking/culling/LOD;
- explicit foreground short-session prototype;
- source tests.

Prototype choices that are **not** final product decisions:

- foreground-only explicit short sessions are superseded by persistent Explorer Mode + automatically segmented Journey direction;
- the single-screen app shell is superseded by Map / Atlas / Journal navigation;
- current navy/amber/cyan styling is superseded by monochrome direction;
- current native map + detached React/SVG fog camera behavior is not accepted as final due visible gesture desynchronization.

## Revised foundation sequence

### Foundation A — Product shell

Plan:
`docs/superpowers/plans/2026-09-04-realityfog-foundation-a-product-shell.md`

Prompt:
`prompts/003-foundation-a-product-shell.md`

Scope:
- Map / Atlas / Journal navigation;
- real Atlas city/Twin Cities statistics from current explored cells;
- real Journal view from current completed local records;
- monochrome design system;
- global display of the current foreground exploration state;
- preserve current engine work.

Explicitly not part of Foundation A:
- map/fog synchronization fix;
- background Explorer Mode;
- auto-Journey segmentation;
- reveal-rule redesign;
- XP/zones/discoveries/Collection.

### Foundation B — Map architecture

After Foundation A review:
- resolve map provider/API-key/build configuration;
- prove the correct solution for fog/map synchronization during live gestures;
- decide whether current `react-native-maps` stack remains viable through evidence.

### Foundation C — Persistent Explorer Mode

After map foundation:
- background location and permissions;
- durable Explorer Mode ON/OFF state;
- honest platform/degraded states;
- automatic Journey segmentation design and migration from explicit sessions.

### Foundation D — Exploration rules

Then reconcile GL-001 through GL-009 as appropriate before progression becomes authoritative.

Only after the foundations pass do we resume authored areas/zones, progression/XP, discoveries, richer Journal, account sync, and future Collection.

## Current action

Run Bilt against the existing implementation using:

`prompts/003-foundation-a-product-shell.md`

Do not ask Bilt to continue beyond Foundation A automatically.

After Bilt returns, review the actual repository diff and runtime/screenshots before generating Foundation B work.

## Bilt execution policy

- Review actual source/result before advancing.
- Never update progress from Bilt claims alone.
- Do not silently change locked product decisions.
- Before each new prompt, check both UX/navigation spec and `docs/GAME_LOGIC_BACKLOG.md`.
- Do not let Bilt decide OPEN DESIGN items on its own.
- Technical spikes must answer one explicit architectural question rather than opportunistically build unrelated features.
- Foundation quality takes precedence over piling on gameplay features.
