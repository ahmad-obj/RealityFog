# RealityFog

Project control repository for the RealityFog mobile app.

**Product:** a dense exploration game for Islamabad + Rawalpindi where the map begins under fog and real physical movement permanently reveals the world. The app also preserves a personal history of where and when the user explored.

## Working model

- **ChatGPT:** product direction, specs, decisions, progress tracking, Bilt prompts, review of Bilt results.
- **Bilt AI:** actual React Native/Expo app implementation.
- **This repo:** authoritative documentation and prompt history. App code should only be added here if/when Bilt exports its implementation and the user wants that.

## Project docs

- `docs/superpowers/specs/2026-09-04-reality-fog-design.md` — overall product design
- `docs/GAME_LOGIC_BACKLOG.md` — living accepted/open gameplay logic that must be revisited across future phases
- `docs/DECISIONS.md` — locked product/technical decisions
- `docs/PROGRESS.md` — current implementation state, gates and next architectural concerns
- `docs/reviews/` — source-level reviews of Bilt implementation phases
- `prompts/` — Bilt implementation/correction prompts

## Documentation rule

Before creating any new Bilt prompt, check:

1. the main product spec;
2. `docs/DECISIONS.md`;
3. `docs/GAME_LOGIC_BACKLOG.md`;
4. `docs/PROGRESS.md`;
5. the latest relevant implementation review.

The game-logic backlog is intentionally staged: not every accepted item belongs in the next prompt, and entries marked **OPEN DESIGN** must not be silently decided by Bilt.

## Current stage

The early map/fog/GPS foundation exists in the Bilt implementation repository. Further feature expansion is paused while product navigation, visual direction, map/fog interaction architecture, and staged game-logic rules are clarified.
