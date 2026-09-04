# RealityFog — Decisions

This file records decisions that should not be silently changed by later Bilt prompts.

## Locked

- Platform: mobile app.
- Initial geography: **Islamabad + Rawalpindi only**.
- Product direction: **exploration game**, not navigation utility.
- Core mechanic: unexplored map starts under fog; real physical movement permanently reveals territory.
- Fog does not regenerate.
- The experience should be dense and handcrafted rather than globally broad.
- Personal history is a core product system, not an analytics afterthought.
- History should record when/where the user explored, route, distance, duration, new territory, zones and discoveries.
- Location history is private by default.
- Repeated travel through already explored territory should provide little/no normal exploration reward, while revisit value is still an open game-design problem.
- Islamabad, Rawalpindi, and overall Twin Cities exploration progress must be tracked separately.
- The current prototype reveal radius is too large and must be reduced; exact tuning remains coupled to cell resolution, GPS quality and route interpolation.
- Legitimate movement between accepted GPS samples should reveal continuously along the travelled segment rather than only at isolated sample points.
- Journey distance and actual exploration distance must not be treated as the same future progression statistic.
- Zone progress should become the main achievable exploration goal beneath high-level city progress.
- Actual app implementation is delegated to **Bilt AI**.
- ChatGPT maintains project docs, decisions, progress, implementation prompts and reviews Bilt output.
- Control/documentation repository: `ahmad-obj/RealityFog`.
- Current Bilt implementation repository: `ahmad-obj/realityfog-d46c99`.

## Living game-logic decisions

Detailed accepted directions, unresolved gameplay rules, dependencies, and future implementation checkpoints are maintained in:

`docs/GAME_LOGIC_BACKLOG.md`

Every future Bilt prompt that touches reveal behavior, progression, zones, GPS trust, travel mode, or distance statistics must check that document first.

Important currently unresolved logic there includes:

- how walking/cycling/vehicle travel should differ for reveal and progression;
- what value revisiting already explored territory should provide without XP farming;
- the final GPS-acquisition/lock rule before first permanent reveal.

These are accepted problems but **not yet final rules** and must not be silently decided by Bilt.

## Working technical direction

- Bilt target: React Native + Expo + TypeScript.
- Current implementation uses `react-native-maps`, `react-native-svg`, Zustand and AsyncStorage.
- GPS/location is required for the core loop.
- Fog state uses a spatial representation rather than treating raw GPS history as the authoritative explored map.
- Current prototype spatial grid is approximately **100 m × 100 m cells** with an organic SVG-mask reveal layer.
- Weak/impossible GPS readings should not reveal territory.
- History/progress should survive restart and intermittent connectivity.
- Phase 1 uses development-only simulated movement; Phase 2 introduces deliberate foreground GPS sessions.
- Passive/background 24/7 location tracking remains out of scope.

## Deferred / not locked yet

- Final product name and visual identity.
- Final map SDK/provider for production.
- Final territory cell/tile resolution.
- Exact final reveal radius.
- Final vehicle/travel-mode progression rules.
- Final revisit-value mechanics.
- Final GPS acquisition/lock criteria.
- Backend provider.
- Final zone boundaries.
- XP balancing.
- Social mechanics.
- Monetization.
- AR features.
- Expansion beyond the twin cities.
