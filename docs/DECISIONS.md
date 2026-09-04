# RealityFog — Decisions

This file records decisions that should not be silently changed by later Bilt prompts.

## Locked

- Platform: mobile app.
- Initial geography: **Islamabad + Rawalpindi only**.
- Product direction: **exploration game**, not navigation utility.
- Core mechanic: unexplored map starts under fog; verified real physical movement permanently reveals territory.
- Fog does not regenerate.
- The experience should be dense and handcrafted rather than globally broad.
- Personal history is a core product system, not an analytics afterthought.
- Location history is private by default.
- Actual app implementation is delegated to **Bilt AI**.
- ChatGPT maintains project docs, decisions, progress, implementation prompts and reviews Bilt output.
- Control/documentation repository: `ahmad-obj/RealityFog`.
- Current Bilt implementation repository: `ahmad-obj/realityfog-d46c99`.

## Locked UX / navigation foundation

Authoritative UX/navigation spec:

`docs/superpowers/specs/2026-09-04-realityfog-navigation-ux-foundation.md`

Locked directions from that spec:

- **Explorer Mode is a global user-controlled ON/OFF state.**
- Explorer Mode remains ON until the user explicitly stops it; changing screens, locking the phone, or backgrounding the app should not itself end exploration where the OS permits background tracking.
- Persistent Explorer Mode is opt-in and must remain visibly active to the user; it is not hidden passive tracking enabled by default.
- The app must not represent an arbitrarily long Explorer Mode period as one giant manual session. Long-term history should evolve toward automatically segmented **Journeys** beneath Explorer Mode.
- Exact automatic Journey segmentation rules are still OPEN DESIGN and must not be guessed by Bilt.
- Long-term navigation architecture is **Map / Atlas / Journal / Collection**.
- Current visible navigation should be **Map / Atlas / Journal** until Collection has real content; do not ship a dead placeholder Collection tab.
- **Map** is for immediate exploration.
- **Atlas** is for named geographical progress, city/area status and what to explore next.
- **Journal** is for personal movement/Journey history.
- **Collection** is reserved for future badges, awards, photo finds, rare discoveries and collectible sets.
- Profile/settings does not consume a permanent bottom-navigation slot.
- Global exploration status remains visible across major destinations while Explorer Mode is ON.
- The visual direction is predominantly **black / white / graphite / restrained gray**, with color reserved for semantic meaning rather than decorative branding.
- The current navy/amber/cyan prototype palette is not the target visual identity.
- The current map/fog gesture desynchronization is not accepted as final behavior. Map and fog must behave as one spatial surface during active pan/zoom.
- The map SDK/provider is not locked yet; decide it through a focused technical spike rather than inertia.
- Do not return to the old straight-line feature sequence by automatically adding XP after GPS. Product shell, map architecture, persistent Explorer Mode, and staged exploration rules come first.

## Living game-logic decisions

Detailed accepted directions, unresolved gameplay rules, dependencies, and future implementation checkpoints are maintained in:

`docs/GAME_LOGIC_BACKLOG.md`

Every future Bilt prompt that touches reveal behavior, progression, zones, GPS trust, travel mode, or distance statistics must check that document first.

Locked directions already recorded there include:

- Islamabad, Rawalpindi, and overall Twin Cities exploration progress must be tracked separately.
- The current prototype reveal radius is too large and must be reduced; exact tuning remains coupled to cell resolution, GPS quality and route interpolation.
- Legitimate movement between accepted GPS samples should reveal continuously along the travelled segment rather than only at isolated sample points.
- Journey distance and actual exploration distance must not be treated as the same future progression statistic.
- Zone progress should become the main achievable exploration goal beneath high-level city progress.
- Repeated travel through already explored territory should provide little/no normal exploration reward, while revisit value remains an open design problem.

Important currently unresolved logic includes:

- how walking/cycling/vehicle travel should differ for reveal and progression;
- what value revisiting already explored territory should provide without XP farming;
- the final GPS-acquisition/lock rule before first permanent reveal;
- automatic Journey segmentation beneath persistent Explorer Mode.

These are accepted problems but **not yet final rules** and must not be silently decided by Bilt.

## Working technical direction

- Bilt target: React Native + Expo + TypeScript.
- Current implementation uses `react-native-maps`, `react-native-svg`, Zustand and AsyncStorage, but this is prototype state rather than a guarantee of the final map stack.
- GPS/location is required for the core loop.
- Fog state uses a spatial representation rather than treating raw GPS history as the authoritative explored map.
- Current prototype spatial grid is approximately **100 m × 100 m cells** with an organic SVG-mask reveal layer.
- Weak/impossible GPS readings should not reveal territory.
- History/progress should survive restart and intermittent connectivity.
- Target Explorer Mode requires background-location capability while ON, subject to platform permissions and OS limitations.
- The current foreground-only location/session architecture is prototype work and is superseded as a product model by persistent Explorer Mode.

## Deferred / not locked yet

- Final product name.
- Exact typography/iconography/motion system within the approved monochrome direction.
- Final map SDK/provider for production.
- Final territory cell/tile resolution.
- Exact final reveal radius.
- Final vehicle/travel-mode progression rules.
- Final revisit-value mechanics.
- Final GPS acquisition/lock criteria.
- Final Journey auto-segmentation rules.
- Exact area taxonomy and boundaries.
- Exact Discovered / Explored / Mastered thresholds.
- Backend provider.
- XP balancing/economy.
- Collection implementation and photo/CV verification model.
- Social mechanics.
- Monetization.
- AR features.
- Expansion beyond the twin cities.
