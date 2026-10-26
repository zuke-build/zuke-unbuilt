# Release operations

A canary component stages a candidate, steps its exposure up under analysis, optionally waits for approval, and promotes it, with one rollback shared by every failure path. A telemetry plugin exports run and target spans and counters to a collector.

This module is partial. Its exclusions are listed in the root `SPEC.md`.

## Requirements

- **DELIVER-1** (MUST) Every canary failure path (a failed analysis, a failed exposure step or promotion, an approval timeout, a cancel, and a stage that fails part-way) runs one idempotent rollback.
- **DELIVER-2** (MUST) A promoted release is never rolled back by a later cancel, and a rollout refused by its lock changes nothing.
- **DELIVER-3** (MUST) The rollback is told why it runs: a rollback with recorded platform state, or a hand-started run with empty state; a degraded record still rolls back as a rollback.
- **DELIVER-4** (MUST) The canary refuses to run without a durable state store, and the platform state it persists across processes holds no secrets.
- **DELIVER-5** (MUST) A lock is held from staging until the run settles, across bakes and approvals.
- **DELIVER-6** (MUST) Analyses receive the exposure actually achieved; a reported exposure outside 0 to 100 fails and rolls back; a not-a-number or empty metric result fails the analysis; an inline bake stops at the first failing check.
- **DELIVER-7** (MUST) Analysis failure messages are masked, credentials in metric addresses are stripped, a query carrying credentials needs an encrypted transport except on loopback, and redirects are not followed.
- **DELIVER-8** (MUST) Trace and span identifiers are derived deterministically from the run identifier and the target, so a run that suspends and resumes produces one trace spanning the gap.
- **DELIVER-9** (MUST) Telemetry export is best-effort: errors and non-success responses are swallowed, each export is bounded by a timeout, the plugin does nothing without an endpoint, and exported records hold no secrets.
- **DELIVER-10** (MUST) The final trace of a run is exported exactly once, even across a cancelling-then-cancelled sequence.
- **DELIVER-11** (SHOULD) A long bake can suspend the run and be reopened by a periodic check instead of holding a CI job.
- **DELIVER-12** (SHOULD) Promotion can wait for an approval signal delivered from another process.
- **DELIVER-13** (SHOULD) The run summary shows achieved and requested exposure side by side.
- **DELIVER-14** (SHOULD) The telemetry plugin honours the standard collector environment variables, explicit setters win, and per-signal endpoints are used verbatim.
