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
- Repeated travel through already explored territory should provide little/no exploration reward.
- Actual app implementation is delegated to **Bilt AI**.
- ChatGPT maintains project docs, decisions, progress, implementation prompts and reviews Bilt output.

## Working technical direction

- Bilt target: React Native + Expo.
- GPS/location is required for the core loop.
- Fog state should use a spatial representation rather than treating raw GPS history as the authoritative explored map.
- Weak/impossible GPS readings should not reveal territory.
- History/progress should survive restart and intermittent connectivity.

## Deferred / not locked yet

- Final product name and visual identity.
- Map SDK/provider.
- Territory cell/tile resolution.
- Backend provider.
- Final zone boundaries.
- XP balancing.
- Social mechanics.
- Monetization.
- AR features.
- Expansion beyond the twin cities.
