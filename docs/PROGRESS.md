# RealityFog — Progress

Last updated: 2026-09-04

## Current phase

**Implementation planning complete — awaiting Bilt Prompt 001 execution**

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

## Current gate

Run this prompt in Bilt:

`prompts/001-foundation-fog-prototype.md`

The goal is to prove the highest-risk mechanic before building the rest: a real map of Islamabad/Rawalpindi with satisfying, geographically anchored fog that is progressively removed by a development simulated route and remains persistently revealed.

## What to return after Bilt Prompt 001

Any combination that lets the result be verified:
- Bilt's implementation report;
- screenshots/video/preview;
- errors;
- exported code/repository state if available;
- your own notes about what works or looks wrong.

I will review that evidence before issuing Prompt 002.

## Planned execution order

1. **Prompt 001:** Foundation + fog-reveal prototype — READY
2. Prompt 002: Real exploration session + GPS filtering
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

Status: **READY / NOT YET VERIFIED**

File: `prompts/001-foundation-fog-prototype.md`

Objective: establish the React Native/Expo foundation and prove the core fog-of-war visual/technical mechanic with development-only simulated movement and local persistence.
