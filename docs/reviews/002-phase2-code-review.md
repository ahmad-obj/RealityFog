# RealityFog — Phase 2 Code Review

Date: 2026-09-04
Implementation repo: `ahmad-obj/realityfog-d46c99`
Reviewed commit: `6a19f90266d56ac5ead07f409a83daa72fcceadb`
Prompt: `prompts/002-real-gps-exploration.md`

## Verdict

**PASS WITH FIXES REQUIRED BEFORE PHASE 3**

The Phase 2 architecture is substantially correct and matches the requested direction. Real foreground GPS sessions, permission handling, filtering, local active-session persistence, interruption recovery, and simulator coexistence are implemented in source.

Two persistence issues should be fixed before building progression/history on top of this foundation.

## Verified from source

- `expo-location` added and configured.
- iOS uses When-In-Use copy only.
- Android requests coarse/fine foreground location and explicitly blocks background location.
- No background task/geofence/foreground service implementation was added.
- Start Exploration requests permission and creates a real session.
- End Exploration detaches the watcher and flushes state.
- App backgrounding detaches foreground tracking; returning active resumes an existing session.
- Active session drafts are locally checkpointed.
- Completed sessions are stored separately from fog state.
- Accepted GPS points are filtered before route/reveal logic.
- Invalid coordinates, poor accuracy, stale timestamps and impossible speed are rejected.
- Rejected points do not reveal fog or add route distance.
- Outside-world accepted points can remain in route continuity but do not reveal fog.
- Real sessions and DEV simulation are prevented from actively running together.
- DEV fog reset wording now accurately says it keeps session history.
- Source tests exist for location filtering, session logic and movement-source exclusivity.

## Issue 1 — completed history can be lost on storage failure

Current `endSession()` flow:

1. finalize session in memory;
2. append finalized session to completed history;
3. clear active-session storage regardless of whether history append succeeded.

If the completed-history write fails but clearing the active draft succeeds, the route/session is no longer recoverable. The UI reports the failure, but the data has already been discarded.

### Required fix

Use a recoverable completion transaction:

- write the **finalized** session to the active/pending key first;
- attempt to append it to history;
- clear the pending key only after history succeeds;
- on launch, if the pending session is already finalized, retry history persistence instead of restoring it as an active GPS session;
- never restart location tracking for a finalized pending session.

Add tests for failed history persistence + successful retry.

## Issue 2 — history is silently capped at 100 sessions

`sessionStorage.ts` defines `HISTORY_LIMIT = 100` and slices older entries away.

That conflicts with the product requirement that Explorer History becomes the user's persistent record of where/when they explored. A user should not silently lose old journeys after session 101.

### Required fix

For this local V1:

- remove the silent 100-session deletion;
- preserve all completed session metadata;
- if storage scale becomes a concern later, migrate to a scalable/indexed store explicitly rather than silently deleting user history.

## Non-blocking observations

### GPS accuracy boundary

Prompt 002 recommended accepting reported accuracy `<= 50 m`. Current source rejects fixes at exactly `50 m` (`>= 50` is rejected). This is slightly stricter and acceptable for now if intentional.

### Distance jitter

Distance currently uses direct geodesic distance between accepted fixes. Accuracy/speed filtering removes large errors, but small GPS drift while stationary may still inflate session distance. This should be tested on-device and improved before distance becomes a meaningful progression/statistic.

### Test/lint execution

Tests and a `test` script exist in source, but GitHub has no CI/status checks attached to the reviewed commit. Therefore test/lint/type success is **not independently verified** from GitHub.

## Manual phone gate still open

Before treating real GPS behavior as proven, manually verify:

- permission grant/deny/settings recovery;
- Start Exploration begins live fixes;
- End Exploration stops updates;
- fog reveals only while session is active;
- backgrounding stops tracking and foreground resumes the same session;
- weak GPS shows a controlled state without opening fog;
- outside Islamabad/Rawalpindi does not reveal fog;
- force-close/relaunch restores an unfinished session;
- session completion survives relaunch;
- stationary GPS does not grossly inflate distance.

## Next action

Run `prompts/002b-phase2-persistence-fixes.md` in Bilt, then re-review the resulting commit before Phase 3.
