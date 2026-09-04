# RealityFog — Progress

Last updated: 2026-09-04

## Current phase

**Phase 2 persistence correction implemented; feature expansion paused pending product/map/navigation foundation decisions**

## Repositories

- Control/docs/prompts: `ahmad-obj/RealityFog`
- Bilt implementation: `ahmad-obj/realityfog-d46c99`
- Reviewed Phase 1 commit: `344697a5491ed3907e7858a6c6a96b76172857a0`
- Reviewed Phase 2 commit: `6a19f90266d56ac5ead07f409a83daa72fcceadb`
- Fog-scaling commit: `7a86991cf15c58fc80de775cd744867262cb934f`
- Persistence-correction commit present in implementation repo: `7e5cc087c4b9a46e5f176079050c58dc84ddadb0`

## Completed

- [x] Product concept/spec/decisions established.
- [x] Bilt-oriented V1 implementation plan created.
- [x] Prompt 001 executed and code-reviewed.
- [x] Genuine subtractive fog engine, spatial explored-cell state, simulation and persistence verified in source.
- [x] Prompt 002 generated and executed by Bilt.
- [x] Phase 2 implementation diff reviewed against Prompt 002.
- [x] Verified foreground-only Expo Location configuration.
- [x] Verified Start/End Exploration architecture and real GPS watcher lifecycle.
- [x] Verified GPS quality filtering before route/reveal state.
- [x] Verified active-session checkpointing and foreground/background recovery logic.
- [x] Verified outside-world points do not reveal fog.
- [x] Verified simulator/real-session active-mode exclusion.
- [x] Verified source tests were added for filter/session/mode logic.
- [x] Created Phase 2 review: `docs/reviews/002-phase2-code-review.md`.
- [x] Generated corrective Prompt 002b.
- [x] Bilt added fog-render scaling work with chunking/culling/LOD.
- [x] Bilt implemented the 002b persistence correction in commit `7e5cc087...`.
- [x] Source now contains recoverable finalized-session handling and no 100-session history truncation.
- [x] Created living gameplay staging document: `docs/GAME_LOGIC_BACKLOG.md`.
- [x] Cross-linked the game-logic backlog from `docs/DECISIONS.md` and the main product spec.

## Living game-logic backlog

Authoritative file:

`docs/GAME_LOGIC_BACKLOG.md`

This file exists because accepted product logic cannot all be implemented in one prompt/phase. Future Bilt prompts must check it and pull in only the items appropriate to that phase.

Currently tracked:

- GL-001 smaller reveal radius — LOCKED DIRECTION
- GL-002 continuous segment/path reveal — LOCKED DIRECTION
- GL-003 separate Islamabad / Rawalpindi / Twin Cities progress — LOCKED DIRECTION
- GL-004 travel-mode fairness — OPEN DESIGN
- GL-005 revisit value without normal XP farming — OPEN DESIGN
- GL-006 trustworthy GPS acquisition before first permanent reveal — OPEN DESIGN
- GL-007 journey vs playable-world vs new-exploration distance — LOCKED DIRECTION
- GL-008 zones as primary achievable exploration goals — LOCKED DIRECTION / IMPLEMENT LATER

User has additional recommendations still to add. They must be recorded in this backlog as they are discussed rather than inferred in advance.

## Phase 1 status

**Code-level: PASS WITH MANUAL VISUAL GATE**

Manual device verification remains important for fog alignment, visual reveal quality, haptics, restart persistence and performance.

## Phase 2 status

The original Phase 2 source review found two persistence issues. Bilt later implemented corrective logic in commit `7e5cc087...`:

- finalized sessions are written to a recoverable pending record before history completion;
- finalized pending sessions are not restored as active GPS sessions;
- history append remains idempotent;
- the silent 100-session history limit was removed.

No GitHub CI/status checks are attached, so command-pass claims still require runtime/build evidence rather than GitHub status alone.

## Product/foundation concerns discovered after early implementation

Feature expansion is intentionally paused because the current implementation exposed higher-level decisions that were not designed first:

- product navigation/screen architecture is not yet settled;
- visual identity/UI system is not yet intentionally designed;
- map provider/API-key/build behavior needs deliberate resolution;
- current native-map + separate React/SVG fog overlay produces visible camera-sync problems during active gestures;
- progression should not be layered on before the relevant game-logic rules are resolved.

These are not yet implementation instructions. They are the next architectural/design discussion.

## Planned execution order

The old straight-line sequence is no longer authoritative. Do **not** automatically proceed from Phase 2 into XP.

Completed:
1. Prompt 001 — Foundation + fog prototype
2. Prompt 002 — Real GPS sessions
3. Prompt 002b — Persistence reliability correction

Before additional feature prompts, resolve the product/map/navigation foundation and reconcile future phases with `docs/GAME_LOGIC_BACKLOG.md`.

Candidate later systems remain:
- reveal/gameplay-rule hardening;
- navigation/product shell;
- city/zone progression;
- authored zones + discoveries;
- session summary + journal/history;
- profile/progression;
- persistence/account sync;
- product polish/hardening.

Exact order is intentionally pending redesign rather than assumed.

## Prompt history

### Prompt 001 — Foundation + Fog Prototype
Status: **EXECUTED / CODE-REVIEWED / MANUAL VISUAL GATE OPEN**

### Prompt 002 — Real GPS Exploration
Status: **EXECUTED / CODE-REVIEWED**
Implementation commit: `6a19f90266d56ac5ead07f409a83daa72fcceadb`
Review: `docs/reviews/002-phase2-code-review.md`

### Prompt 002b — Phase 2 Persistence Fixes
Status: **EXECUTED IN BILT; CORRECTIVE SOURCE PRESENT**
File: `prompts/002b-phase2-persistence-fixes.md`
Implementation commit: `7e5cc087c4b9a46e5f176079050c58dc84ddadb0`

## Bilt execution policy

- Review actual source/result before advancing.
- Never update progress from Bilt claims alone.
- Do not silently change locked product decisions.
- Before each new prompt, check `docs/GAME_LOGIC_BACKLOG.md` for relevant staged logic.
- Do not let Bilt decide OPEN DESIGN items on its own.
- Foundational product/map/navigation decisions take precedence over piling on more features.
