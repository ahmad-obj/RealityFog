# RealityFog — Game Logic Decisions & Backlog

Last updated: 2026-09-04
Status: Living product-logic document

## Purpose

This file tracks gameplay/product logic that has been accepted as important but may need to be implemented across multiple Bilt phases.

It is intentionally separate from the implementation plan. A feature appearing here does **not** mean it should be implemented immediately.

Rules for future work:

- Check this document before writing each new Bilt prompt.
- Pull in only the items that belong naturally in that phase.
- Do not silently invent final rules for entries marked **OPEN DESIGN**.
- Once an open item is decided, record the final rule here before implementation.
- If implementation reveals a conflict with one of these rules, update the decision explicitly rather than silently changing behavior.

---

## Status legend

- **LOCKED DIRECTION** — product direction accepted; exact implementation details may still require tuning.
- **OPEN DESIGN** — the problem/direction is accepted, but the final gameplay rule is not yet decided.
- **IMPLEMENT LATER** — accepted but intentionally deferred until the relevant system exists.

---

## GL-001 — Reduce the fog reveal radius

**Status:** LOCKED DIRECTION

The current prototype reveal radius of approximately **220 m** is too generous for the intended exploration experience.

A user travelling along one street should not automatically clear several nearby streets or blocks that they never meaningfully visited.

### Decision

- Reduce the effective reveal radius substantially from the current prototype value.
- The map should reveal territory close to the user's actual travelled path rather than a broad circular area.
- Do **not** lock an exact final radius yet without physical-device testing.

### Important dependency

The current prototype uses roughly 100 m spatial cells and determines reveal candidates from cell centres. Therefore the final radius cannot be tuned independently of:

- cell/grid resolution;
- path interpolation;
- GPS accuracy;
- visual feathering of the fog mask.

A smaller radius must not create gaps simply because a user's location falls near the edge/corner of a large grid cell.

### Revisit

Before or during the spatial-territory/progression phase, with physical GPS testing.

---

## GL-002 — Reveal continuously along the travelled segment

**Status:** LOCKED DIRECTION

Fog reveal should represent the route actually travelled, not only isolated GPS sample points.

### Decision

For each pair of accepted GPS fixes:

- treat the segment between them as the travelled path;
- interpolate/sweep reveal coverage along that legitimate segment;
- reveal the appropriate nearby territory along the segment;
- never interpolate across a rejected/impossible GPS jump.

This becomes especially important once GL-001 reduces the reveal radius.

### Goal

A legitimate walk or drive should create a continuous explored corridor without requiring extremely dense GPS samples.

### Revisit

Spatial-territory/reveal-engine phase.

---

## GL-003 — Separate Islamabad, Rawalpindi, and Twin Cities progress

**Status:** LOCKED DIRECTION

The user should not see one city's label with a percentage calculated from both cities combined.

### Decision

Track and expose at least three progress values:

- **Islamabad explored %**
- **Rawalpindi explored %**
- **Twin Cities overall explored %**

The current location can influence which city stat is emphasized in the HUD, but the underlying values must be mathematically separate.

### Revisit

When city/zone progression is implemented.

---

## GL-004 — Travel mode must affect exploration fairness

**Status:** OPEN DESIGN

The current GPS rules allow legitimate high-speed travel. That raises a core game-design question: how should walking, cycling, cars, buses, and other transport affect fog reveal and progression?

### Accepted direction

Travel mode **must matter somehow** so the exploration game cannot be trivially optimized by simply driving around large roads for a few hours.

### Not decided yet

Do **not** assume any of the following until explicitly decided:

- that vehicles reveal the exact same fog as walking;
- that vehicles reveal less fog;
- that vehicle travel reveals history but not game progress;
- that walking/cycling receive multipliers;
- that vehicle exploration earns zero XP;
- that the user manually selects a travel mode;
- that the app automatically infers travel mode.

### Questions to resolve

- What should count as having truly "explored" a place?
- Should being driven through a road permanently uncover it?
- Should map history and game progression use different rules?
- How can the rule avoid punishing people who legitimately explore by car/bus?
- Can travel mode be inferred reliably enough to affect rewards?
- What behavior remains understandable to the player without explaining a complicated scoring system?

### Revisit

Before XP/progression rules become permanent.

---

## GL-005 — Revisited territory must still have value

**Status:** OPEN DESIGN

Once a user's normal surroundings are explored, repeatedly travelling through them should not make RealityFog feel dead. At the same time, old routes must not become an easy source of infinite exploration rewards.

### Accepted direction

- New territory remains the primary source of exploration progress.
- Repeating the same commute should **not** farm normal exploration XP.
- Previously explored places should still provide some form of useful/personal/game value.

### Final rule not decided

Potential sources of revisit value may later include things such as:

- journey/history recording;
- distance and personal statistics;
- challenges/missions;
- discovery interactions;
- route-specific accomplishments;
- time/context-based goals.

These examples are **possibilities, not approved mechanics yet**.

### Revisit

During progression, missions, and retention design.

---

## GL-006 — Do not permanently reveal from one unconfirmed first GPS fix

**Status:** OPEN DESIGN

The first accepted GPS sample currently has no previous accepted point against which movement consistency can be checked.

A single inaccurate-but-technically-acceptable first fix should not permanently alter the user's map.

### Accepted direction

RealityFog needs a stronger **GPS acquisition / exploration-start lock** before permanent reveal begins.

### Final lock rule is intentionally undecided

Possible inputs to the decision include:

- multiple consecutive accurate fixes;
- position consistency over a short period;
- reported accuracy improving below a stricter initial threshold;
- small movement consistency checks;
- a visible `Acquiring GPS…` state;
- timeout/fallback behavior in difficult GPS environments.

Do not implement an arbitrary "2 fixes" or "3 fixes" rule without designing the behavior properly.

### Desired outcome

- starting a session feels fast;
- false initial reveals are rare;
- the player understands when the app is waiting for a trustworthy location;
- poor urban GPS does not make the app unusable.

### Revisit

Before polishing real-world GPS exploration and before XP becomes meaningful.

---

## GL-007 — Separate journey distance from exploration distance

**Status:** LOCKED DIRECTION

A session may contain legitimate travel outside the playable world or through already explored territory. Those kilometres still belong to the person's journey history, but they are not identical to actual RealityFog exploration.

### Decision

Keep separate concepts/statistics for at least:

- **total journey distance** — legitimate accepted travel recorded during the session;
- **playable-world distance** — legitimate travel inside Islamabad/Rawalpindi;
- **new-exploration distance/progress** — movement that meaningfully contributes to new exploration, once its exact definition is finalized.

Do not use one generic `distanceM` value for every future profile/progression purpose.

### Revisit

Session/history data-model evolution and progression/profile phases.

---

## GL-008 — Zones should be the primary achievable exploration goals

**Status:** LOCKED DIRECTION / IMPLEMENT LATER

A single giant city-wide percentage will become psychologically weak as progress slows.

### Decision

City percentages remain useful high-level identity/status numbers, but the primary moment-to-moment completion goals should be **zones**.

The experience should eventually communicate progress more like:

- Islamabad — overall exploration
- F-7 — local exploration progress
- Blue Area — local exploration progress/completion
- Margalla — local exploration progress
- nearby unexplored pockets / next achievable goals

### Product principle

Users should regularly have reachable targets rather than seeing only a huge city denominator that moves by tiny fractions.

### Revisit

Zone/discovery phase and later progression polish.

---

# Accepted items summary

| ID | Direction | Status |
|---|---|---|
| GL-001 | Smaller reveal radius | LOCKED DIRECTION |
| GL-002 | Segment/path interpolation for reveal | LOCKED DIRECTION |
| GL-003 | Separate Islamabad / Rawalpindi / Twin Cities progress | LOCKED DIRECTION |
| GL-004 | Travel mode must affect fairness | OPEN DESIGN |
| GL-005 | Revisited territory must retain value without XP farming | OPEN DESIGN |
| GL-006 | Stronger initial GPS lock before permanent reveal | OPEN DESIGN |
| GL-007 | Separate journey vs exploration distance | LOCKED DIRECTION |
| GL-008 | Zones become primary achievable exploration goals | LOCKED DIRECTION / IMPLEMENT LATER |

---

# Pending user additions

The user has additional recommendations to add. Record them here as they are discussed, preserving whether each recommendation is:

- accepted;
- rejected;
- still under discussion;
- dependent on another game-logic decision.

Do not infer or fill in user recommendations before they are actually provided.
