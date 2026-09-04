# RealityFog — Progress

Last updated: 2026-09-04

## Current phase

**Phase 2 code-reviewed — corrective Prompt 002b required before Phase 3**

## Repositories

- Control/docs/prompts: `ahmad-obj/RealityFog`
- Bilt implementation: `ahmad-obj/realityfog-d46c99`
- Reviewed Phase 1 commit: `344697a5491ed3907e7858a6c6a96b76172857a0`
- Reviewed Phase 2 commit: `6a19f90266d56ac5ead07f409a83daa72fcceadb`

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

## Phase 1 status

**Code-level: PASS WITH MANUAL VISUAL GATE**

Manual device verification remains open for fog alignment, visual reveal quality, haptics, restart persistence and sample-route performance.

## Phase 2 status

**Code-level: PASS WITH FIXES REQUIRED**

Verified from source:
- `expo-location` is installed/configured;
- iOS When-In-Use permission only;
- Android coarse/fine foreground permissions with background permission blocked;
- no background tracking task/service;
- Start Exploration requests permission and creates a real session;
- accepted GPS points update player, route and fog;
- rejected points do not reveal/add route distance;
- weak/stale/impossible fixes are filtered;
- app backgrounding suspends tracking and checkpoints state;
- unfinished active sessions can be restored;
- completed-session local history exists;
- DEV simulation remains separate from real session history;
- fog reset keeps session history.

### Blocking fixes before Phase 3

1. **Recoverable completion transaction** — if the completed-history write fails, current code still clears the active draft, so a completed route can be lost.
2. **Remove silent 100-session history cap** — Explorer History must not delete old journeys after session 100.

Review:
`docs/reviews/002-phase2-code-review.md`

Corrective prompt:
`prompts/002b-phase2-persistence-fixes.md`

## Non-blocking observations

- Current GPS filter rejects exactly 50 m accuracy although Prompt 002 recommended accepting `<= 50 m`; acceptable if intentional/documented.
- Direct point-to-point distance may still accumulate small stationary GPS jitter; test on a phone before distance becomes progression-critical.
- GitHub has no CI/status checks on the reviewed Phase 2 commit, so lint/test/type success is not independently verified here.

## Current action

Run in Bilt:

`prompts/002b-phase2-persistence-fixes.md`

Then return the new Bilt response/commit for re-review.

## Planned execution order

1. Prompt 001 — Foundation + fog prototype — EXECUTED / CODE-REVIEWED
2. Prompt 002 — Real GPS sessions — EXECUTED / CODE-REVIEWED
3. **Prompt 002b — Persistence reliability fixes — READY**
4. Prompt 003 — Spatial territory + XP
5. Prompt 004 — Authored zones + discoveries
6. Prompt 005 — Session summary + explorer history UI
7. Prompt 006 — Explorer profile + progression
8. Prompt 007 — Local-first persistence + account sync
9. Prompt 008 — Product polish + hardening

## Prompt history

### Prompt 001 — Foundation + Fog Prototype
Status: **EXECUTED / CODE-REVIEWED / MANUAL VISUAL GATE OPEN**

### Prompt 002 — Real GPS Exploration
Status: **EXECUTED / CODE-REVIEWED / FIXES REQUIRED**
Implementation commit: `6a19f90266d56ac5ead07f409a83daa72fcceadb`
Review: `docs/reviews/002-phase2-code-review.md`

### Prompt 002b — Phase 2 Persistence Fixes
Status: **READY / NOT YET EXECUTED**
File: `prompts/002b-phase2-persistence-fixes.md`

## Bilt execution policy

- Review actual source/result before advancing.
- Never update progress from Bilt claims alone.
- Do not silently change locked product decisions.
- Fix foundational data-loss issues before layering progression/history UI on top.
