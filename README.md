# RealityFog

Project control repository for the RealityFog mobile app.

**Product:** a dense exploration game for Islamabad + Rawalpindi where the map begins under fog and verified real physical movement permanently reveals the world. The app also preserves a personal history of where and when the user explored.

## Working model

- **ChatGPT:** product direction, specs, decisions, progress tracking, Bilt prompts, review of Bilt results.
- **Bilt AI:** actual React Native/Expo app implementation.
- **This repo:** authoritative documentation and prompt history. App code should only be added here if/when Bilt exports its implementation and the user wants that.

## Project docs

- `docs/superpowers/specs/2026-09-04-reality-fog-design.md` — overall product thesis/scope
- `docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md` — authoritative current navigation, persistent Explorer Mode, Journey model direction, visual system and foundation gates
- `docs/GAME_LOGIC_BACKLOG.md` — living accepted/open gameplay logic that must be revisited across future phases
- `docs/DECISIONS.md` — locked product/technical decisions
- `docs/PROGRESS.md` — current implementation state, gates and next action
- `docs/reviews/` — source-level reviews of Bilt implementation phases
- `prompts/` — Bilt implementation/correction prompts

## Documentation rule

Before creating any new Bilt prompt, check:

1. the overall product spec;
2. the navigation/UX foundation spec;
3. `docs/DECISIONS.md`;
4. `docs/GAME_LOGIC_BACKLOG.md`;
5. `docs/PROGRESS.md`;
6. the latest relevant implementation review and current Bilt source.

Where the older product spec conflicts with the newer navigation/UX foundation on navigation, Explorer Mode, Journey lifecycle, visual direction, or foundation order, the newer navigation/UX foundation controls.

The game-logic backlog is intentionally staged: not every accepted item belongs in the next prompt, and entries marked **OPEN DESIGN** must not be silently decided by Bilt.

## Current stage

The early map/fog/GPS foundation exists in the Bilt implementation repository. Feature expansion is paused. The product shell/navigation direction has now been approved and documented, and the next implementation planning work must solve the map foundation and persistent Explorer Mode architecture before adding XP or broad gameplay systems.
