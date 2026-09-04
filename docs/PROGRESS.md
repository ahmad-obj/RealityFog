# RealityFog — Progress

Last updated: 2026-09-04

## Current phase

**UX/navigation foundation approved and documented; feature expansion remains paused pending written-spec review and foundation implementation planning**

## Repositories

- Control/docs/prompts: `ahmad-obj/RealityFog`
- Bilt implementation: `ahmad-obj/realityfog-d46c99`
- Reviewed Phase 1 commit: `344697a5491ed3907e7858a6c6a96b76172857a0`
- Reviewed Phase 2 commit: `6a19f90266d56ac5ead07f409a83daa72fcceadb`
- Fog-scaling commit: `7a86991cf15c58fc80de775cd744867262cb934f`
- Persistence-correction commit: `7e5cc087c4b9a46e5f176079050c58dc84ddadb0`

## Authoritative product docs

Before every new Bilt prompt, reconcile:

1. `docs/superpowers/specs/2026-09-04-reality-fog-design.md` — overall thesis/product scope;
2. `docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md` — current navigation, Explorer Mode, visual and foundation architecture;
3. `docs/DECISIONS.md` — locked decisions;
4. `docs/GAME_LOGIC_BACKLOG.md` — staged/open gameplay logic;
5. latest implementation review and actual Bilt source.

Where the older product spec conflicts with the newer UX/navigation foundation on exploration lifecycle or navigation, the newer UX/navigation foundation controls.

## Completed

- [x] Product concept/spec/decisions established.
- [x] Bilt-oriented early implementation plan created.
- [x] Prompt 001 executed and code-reviewed.
- [x] Genuine subtractive fog engine, spatial explored-cell state, simulation and persistence verified in source.
- [x] Prompt 002 generated and executed by Bilt.
- [x] Phase 2 implementation diff reviewed against Prompt 002.
- [x] Foreground GPS filtering/session architecture verified as useful prototype work.
- [x] Created Phase 2 review: `docs/reviews/002-phase2-code-review.md`.
- [x] Bilt added fog-render scaling work with chunking/culling/LOD.
- [x] Bilt implemented the 002b persistence correction in commit `7e5cc087...`.
- [x] Source now contains recoverable finalized-session handling and no 100-session history truncation.
- [x] Created living gameplay staging document: `docs/GAME_LOGIC_BACKLOG.md`.
- [x] Cross-linked the gameplay staging document across authoritative docs.
- [x] User approved the revised navigation/product direction: Map / Atlas / Journal, with Collection reserved for future real content.
- [x] User approved persistent global Explorer Mode rather than repeated manual foreground-only short sessions.
- [x] User approved predominantly monochrome black/white/graphite visual direction.
- [x] Created `docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md`.

## Living game-logic backlog

Authoritative file:

`docs/GAME_LOGIC_BACKLOG.md`

Currently tracked:

- GL-001 smaller reveal radius — LOCKED DIRECTION
- GL-002 continuous segment/path reveal — LOCKED DIRECTION
- GL-003 separate Islamabad / Rawalpindi / Twin Cities progress — LOCKED DIRECTION
- GL-004 travel-mode fairness — OPEN DESIGN
- GL-005 revisit value without normal XP farming — OPEN DESIGN
- GL-006 trustworthy GPS acquisition before first permanent reveal — OPEN DESIGN
- GL-007 journey vs playable-world vs new-exploration distance — LOCKED DIRECTION
- GL-008 zones as primary achievable exploration goals — LOCKED DIRECTION / IMPLEMENT LATER

The new persistent Explorer Mode also introduces a separate OPEN DESIGN problem: automatic Journey segmentation. Do not let Bilt invent final segmentation thresholds during unrelated work.

## Current implementation status

The existing Bilt app contains useful engine/prototype work, including:

- fog and explored-territory state;
- GPS filtering;
- persistence/recovery;
- fog chunking/culling/LOD;
- explicit short-session prototype;
- source tests.

But several prototype choices are **not target product decisions**:

- foreground-only explicit short sessions are superseded by persistent Explorer Mode + automatically segmented Journey direction;
- the single-screen app shell is superseded by Map / Atlas / Journal navigation;
- current navy/amber/cyan styling is superseded by monochrome direction;
- current native map + detached React/SVG fog camera behavior is not accepted as final due visible gesture desynchronization.

## Foundation problems to solve before feature expansion

### A. Product shell

- meaningful Map / Atlas / Journal routes;
- restrained global navigation;
- Explorer Mode global active state/control;
- monochrome design-system primitives;
- no empty Collection tab yet.

### B. Map architecture

- resolve Android provider/API-key/build configuration;
- determine root solution for fog/map synchronization during active gestures;
- evaluate whether current `react-native-maps` architecture can satisfy RealityFog or whether another map/rendering stack is necessary;
- validate on a real native build/device rather than trusting static code.

### C. Persistent Explorer Mode

- explicit background-location permission and transparency;
- durable Explorer Mode ON/OFF preference;
- correct foreground/background behavior subject to platform constraints;
- honest degraded/stopped states when the OS prevents reliable tracking;
- design Journey auto-segmentation before replacing short-session history.

### D. Exploration rules

Reconcile the relevant GL backlog items before progression/XP becomes authoritative.

## Planned execution order

The old `GPS → XP → Zones → History` path is retired.

Current intended order after written-spec approval:

1. Foundation planning / technical spike design
2. Product shell/navigation foundation
3. Map-provider + fog synchronization foundation
4. Persistent Explorer Mode/background lifecycle
5. Journey segmentation design + migration from short sessions
6. Smaller-radius / path interpolation / GPS-trust / distance semantics
7. Atlas named areas + city/zone progress
8. Progression/XP only after fairness rules are resolved
9. Discoveries and richer Journal
10. Collection later when a real badge/photo-find system is ready
11. Account sync/social/other expansions only after core loop is stable

## Current action

**Do not send Bilt another feature-building prompt yet.**

The written UX/navigation foundation is at:

`docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md`

It is awaiting user review under the Superpowers architectural workflow. After explicit written-spec approval, create a new implementation plan tailored to the revised foundation rather than continuing the old prompt sequence.

## Bilt execution policy

- Review actual source/result before advancing.
- Never update progress from Bilt claims alone.
- Do not silently change locked product decisions.
- Before each new prompt, check both UX/navigation spec and `docs/GAME_LOGIC_BACKLOG.md`.
- Do not let Bilt decide OPEN DESIGN items on its own.
- Technical spikes must answer one explicit architectural question rather than opportunistically build unrelated features.
- Foundation quality takes precedence over piling on gameplay features.
