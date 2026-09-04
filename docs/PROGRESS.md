# RealityFog — Progress

Last updated: 2026-09-04

## Current phase

**Phase 1 code-reviewed — Phase 2 prompt ready**

## Repositories

- Control/docs/prompts: `ahmad-obj/RealityFog`
- Bilt implementation: `ahmad-obj/realityfog-d46c99`
- Reviewed Phase 1 commit: `344697a5491ed3907e7858a6c6a96b76172857a0`

## Completed

- [x] Selected core concept: real-world exploration game with fog-of-war.
- [x] Restricted initial world to Islamabad + Rawalpindi.
- [x] Established dense/authored-city direction instead of global coverage.
- [x] Made Explorer History a core system.
- [x] Defined the main gameplay loop.
- [x] Defined V1 systems: fog map, sessions, history, zones, discoveries, progression.
- [x] Defined privacy and GPS error principles.
- [x] Created product design spec.
- [x] User approved the design direction.
- [x] Created project decision log.
- [x] Established Bilt AI as implementation agent.
- [x] Created Bilt-oriented V1 implementation plan.
- [x] Generated Prompt 001 for the foundation + fog-reveal prototype.
- [x] Bilt executed Prompt 001 in `ahmad-obj/realityfog-d46c99`.
- [x] Reviewed the actual Phase 1 source at commit `344697a...`.
- [x] Verified genuine subtractive SVG fog masking, spatial explored-cell state, dev simulation, and local persistence in source.
- [x] Created Phase 1 review: `docs/reviews/001-phase1-code-review.md`.
- [x] Generated Prompt 002 for real foreground GPS exploration sessions.

## Phase 1 status

**Code-level: PASS WITH MANUAL VISUAL GATE**

Verified from source:
- real map-first implementation exists;
- fog is genuinely removed through SVG masking;
- fog/reveal geometry is map-anchored;
- explored territory is separate from route history;
- explored state persists via AsyncStorage;
- dev simulation is clearly labelled;
- reset behavior exists.

Not independently verified from GitHub:
- native runtime launch;
- Bilt's lint/type-clean claim (no CI status attached to reviewed commit);
- actual visual smoothness/alignment on device;
- haptics;
- runtime restart persistence.

Report discrepancy:
- Bilt said explored cells were delta-encoded in storage; source actually writes the string cell-key array directly. Acceptable for Phase 1, but the report was inaccurate.

Manual visual gate still worth checking:
- pan/zoom fog alignment;
- organic reveal feel;
- repeated route gives nearly no new reveal;
- force-close/relaunch persistence;
- reset restores fog;
- no obvious frame collapse on the sample route.

## Current action

Run this in Bilt against the existing implementation:

`prompts/002-real-gps-exploration.md`

Objective: add deliberate **real foreground GPS exploration sessions** with permission handling, trusted-location filtering, route capture, session persistence/recovery, and clean Start/End Exploration lifecycle without expanding into history UI, auth or social features yet.

## Planned execution order

1. **Prompt 001:** Foundation + fog-reveal prototype — EXECUTED / CODE-REVIEWED
2. **Prompt 002:** Real exploration session + GPS filtering — READY
3. Prompt 003: Spatial territory model + XP
4. Prompt 004: Authored zones + discoveries
5. Prompt 005: Session summary + explorer history
6. Prompt 006: Explorer profile + progression
7. Prompt 007: Local-first persistence + account sync
8. Prompt 008: Product polish + hardening

## Bilt execution policy

- One meaningful implementation objective per prompt where practical.
- Every prompt states what must not be broken.
- Never assume Bilt completed something merely because it said it did; review the resulting app/code/evidence.
- Update docs from verified results, not intentions.
- Do not let Bilt silently redesign locked product decisions.
- Large redesigns require a new explicit decision before prompting.

## Prompt history

### Prompt 001 — Foundation + Fog Prototype

Status: **EXECUTED / CODE-REVIEWED / MANUAL VISUAL GATE OPEN**

File: `prompts/001-foundation-fog-prototype.md`

Implementation commit: `344697a5491ed3907e7858a6c6a96b76172857a0`

Review: `docs/reviews/001-phase1-code-review.md`

### Prompt 002 — Real GPS Exploration

Status: **READY / NOT YET EXECUTED**

File: `prompts/002-real-gps-exploration.md`

Objective: real foreground location permission, Start/End Exploration lifecycle, GPS quality filtering, route capture, active-session persistence/recovery, and coexistence with the dev simulator.
