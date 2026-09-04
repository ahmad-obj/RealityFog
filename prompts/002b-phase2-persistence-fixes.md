# Bilt Prompt 002b — Phase 2 Persistence Fixes

Continue from the current RealityFog implementation in:

`ahmad-obj/realityfog-d46c99`

Current implementation HEAD reviewed for this prompt:

`7a86991cf15c58fc80de775cd744867262cb934f`

Do not redesign the app and do not start Phase 3 yet.

This is a focused corrective pass for two persistence issues that remain present after the Phase 2 implementation and the later autonomous fog-scaling optimization.

## Preserve current work

Do **not** revert or rewrite the working Phase 1/2 architecture.

In particular preserve:
- foreground-only real GPS exploration sessions;
- existing trusted-location filtering;
- Start / End Exploration lifecycle;
- DEV simulation and its separation from real sessions;
- explored-cell fog authority;
- the newer chunked fog render index / viewport culling / LOD blob rendering introduced in commit `7a86991...`;
- the fog stress-test DEV control added by that commit;
- current UI/map styling and local-first behavior.

The new chunked/LOD fog system is derived rendering state only and must remain compatible with explored-cell persistence.

## Fix 1 — make session completion recoverable

Current problem:

`endSession()` finalizes a session, attempts to append it to completed history, then clears the active-session key even if the history write failed. That can permanently lose a completed route when storage fails at the wrong moment.

Implement completion as a recoverable transaction.

Required behavior:

1. When End Exploration is pressed, detach GPS tracking as now.
2. Finalize the session in memory.
3. Persist the **finalized** session to a recoverable pending/active record before considering completion done.
4. Attempt to append that finalized session to completed history.
5. Only clear the pending/active record after history persistence succeeds.
6. If history persistence fails:
   - keep the finalized session recoverable on disk;
   - show the controlled storage error as now;
   - do not resume GPS tracking for that finalized session;
   - do not silently discard it.
7. On app launch/hydration:
   - if the stored pending session has `endedAt === null`, restore it as an active session exactly as today;
   - if the stored pending session is already finalized (`endedAt !== null`), do **not** restore it as active and do **not** attach a location watcher;
   - retry appending it to completed history;
   - if retry succeeds, clear the pending record;
   - if retry fails, keep it pending and surface a controlled persistence warning.
8. Appending completed history must remain idempotent by session id.

Do not introduce a backend.

### Tests required

Add pure/storage-level tests where practical for:

- successful finalize → history append → pending record cleared;
- history append failure leaves finalized session recoverable;
- next hydration retries a finalized pending session;
- successful retry does not duplicate history;
- finalized pending session never restarts foreground GPS tracking.

If AsyncStorage mocking is awkward in the existing test setup, extract the completion decision/transaction sequencing into testable pure functions and test the behavior there, while keeping the actual storage adapter simple.

## Fix 2 — never silently delete old Explorer History

Current problem:

`lib/storage/sessionStorage.ts` still has `HISTORY_LIMIT = 100` and slices completed history to 100 entries.

RealityFog's history is intended to become the user's persistent record of where and when they explored. Session 101 must not silently delete session 1.

Required behavior for current V1:

- remove the 100-session cap;
- preserve every completed session stored locally;
- keep newest-first ordering if desired;
- keep idempotent replacement by session id;
- do not silently truncate history anywhere else.

If AsyncStorage will eventually be the wrong storage layer for lifetime-scale history, document that as a later migration concern. Do not solve it by deleting old history now.

## Small correctness cleanup

Prompt 002 allowed reported accuracy up to and including 50 m. Current code rejects exactly 50 m.

Either:
- change the filter to reject only `> 50 m`, or
- deliberately keep `< 50 m` and clearly state the reason in the completion report.

Do not spend time tuning GPS beyond this in the corrective pass.

## Do NOT change

Do not add:
- XP/levels;
- zones/discoveries;
- history screens;
- auth/backend;
- friends/social;
- background tracking;
- AR;
- new cities;
- major UI redesign.

Do not replace the current fog engine, including the new chunked/LOD rendering optimization.

## Verification

Run the actual project commands:

```bash
npm test
npm run lint
npm run format:check
npm run expo-check
```

If any command is unavailable or fails for an environment-only reason, report the exact command and exact failure rather than claiming success.

## Completion report

Report only:

1. files changed;
2. exact new completion/pending-session flow;
3. how finalized-session recovery works on relaunch;
4. confirmation that history no longer truncates at 100;
5. confirmation the `7a86991...` chunked/LOD fog work was preserved;
6. tests added/changed;
7. exact command results for test/lint/format/expo-check;
8. remaining limitations.
