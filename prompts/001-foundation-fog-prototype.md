# Bilt Prompt 001 — Foundation + Fog Prototype

You are implementing **Phase 1 of RealityFog**, a real React Native + Expo mobile app.

Do not build the whole product yet. This phase exists to prove the core visual/technical mechanic first.

## Product

RealityFog turns **Islamabad and Rawalpindi** into an exploration game.

The map begins hidden under fog. During a real exploration session later, physical movement will permanently reveal territory. For this first phase, use a clearly labeled **development simulated route** so we can validate the fog mechanic without physically traveling.

The app must feel like a polished exploration game, **not Google Maps with a black layer**.

## Build now

### 1. App foundation
- React Native + Expo + TypeScript.
- Use a clean file/component structure with the map/fog engine separated from screens and storage.
- Primary screen is the map. Do not waste time building auth, onboarding, social features, backend, settings, or unrelated screens yet.
- Use the best map implementation that is genuinely supported in this Bilt/Expo environment and supports custom overlays/shapes plus smooth pan/zoom.

### 2. Twin-cities map
- Initial camera should frame Islamabad/Rawalpindi appropriately, with Islamabad as the initial focal area.
- The app is playable only in Islamabad and Rawalpindi.
- Outside the playable region may remain visible as map context, but it is not part of exploration/progression.

### 3. Fog-of-war — the centerpiece
- Unexplored playable territory must be visually covered by a dark, premium-looking fog layer.
- Explored territory must appear genuinely uncovered beneath it.
- Fog removal should follow a route as a continuous corridor, not as ugly disconnected circles or obvious square blocks.
- Internally you may use cells/tiles/grid geometry, but hide that implementation visually with smooth/organic edges.
- Panning/zooming must keep the fog correctly anchored to geography.
- Do **not** fake exploration by only drawing a bright route line on top of unchanged fog. The fog itself must be removed/reduced for explored territory.

### 4. Development simulated exploration
Add a small, clearly labeled developer/demo control such as **Simulate Exploration**.

When triggered:
- animate a predefined route through a sensible Islamabad area such as the F-6/F-7/Blue Area vicinity;
- progressively reveal fog around the moving simulated position;
- show a small player/location marker moving with the route;
- make the reveal happen progressively so we can judge whether it feels satisfying;
- optional subtle haptic/visual feedback when new territory opens if supported cleanly.

Simulated exploration must always be labeled as test/dev data. It must not look like legitimate real exploration.

### 5. Persistent explored territory
- Persist explored test territory locally using a suitable Expo-compatible local persistence mechanism.
- Reloading/restarting the app must keep previously revealed fog removed.
- Do not make raw route history the authoritative fog state; maintain a compact explored-territory representation.

Add a development-only **Reset Exploration Data** action that restores all fog after explicit confirmation.

### 6. Minimal game HUD
Keep it restrained. On the main map show only useful game information, for example:
- `Islamabad • 0.0% explored` or equivalent placeholder percentage derived from the current test territory;
- compact “new territory” feedback during simulation;
- the Simulate Exploration dev control.

Do not add generic dashboard cards or excessive buttons.

## Visual direction

The map should feel **mysterious, premium, modern, and game-like**.

Avoid:
- default Expo styling;
- generic SaaS cards;
- neon cyberpunk overload;
- cartoon fantasy UI;
- giant gradients everywhere;
- unnecessary text.

Prefer:
- map filling almost the entire screen;
- dark unexplored fog with subtle depth/texture/softness;
- crisp explored terrain beneath it;
- restrained typography;
- small glass/dark HUD elements;
- smooth animations;
- strong contrast around the player's position and newly revealed edge.

The fog/reveal interaction is the hero visual. Spend design effort there.

## Architecture expectations

Keep responsibilities separated conceptually like this, adapting filenames if your environment requires it:

```text
app/index.tsx                     map screen
src/components/map/FogMap.tsx    map + fog presentation
src/domain/exploration/*          reveal/spatial logic
src/storage/*                     persisted explored territory
src/dev/simulatedRoutes.ts        test-only route data
src/theme/*                       visual tokens
```

Use a compact spatial model for explored territory. Expose clean logic equivalent to:

```ts
type LatLng = { latitude: number; longitude: number };

function locationToCell(location: LatLng): string;
function getRevealCells(location: LatLng, radiusM: number): string[];
function getNewCells(candidateCells: string[], explored: Set<string>): string[];
```

The exact spatial algorithm is your engineering choice, but document it briefly in the project/code comments or implementation summary.

## Explicitly DO NOT build yet

- authentication
- backend/cloud sync
- real GPS session lifecycle
- passive/background tracking
- social/friends
- leaderboards
- AR
- large POI/discovery datasets
- monetization
- complex achievements
- expansion outside Islamabad/Rawalpindi

## Completion checklist

Do not call this phase complete until all are true:

1. App launches successfully.
2. Map pans/zooms smoothly.
3. Islamabad/Rawalpindi are the intentional game world.
4. Unexplored territory is visibly fogged.
5. Simulated movement progressively removes the actual fog along its route.
6. Reveal looks continuous and visually satisfying.
7. Revealed territory persists across reload/restart.
8. Reset Exploration Data restores the fog.
9. Simulated/test state is clearly labeled.
10. No unrelated product features were added.

At the end, give me a concise implementation report containing:
- what you built;
- important package/architecture choices;
- exact fog/reveal technique used;
- how local persistence works;
- any limitation in the current preview/runtime;
- what I should manually test before we move to Phase 2.
