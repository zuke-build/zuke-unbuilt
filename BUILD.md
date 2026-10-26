# Build order

Milestones in dependency order. Milestone 1 is the thinnest slice you can see run. The author may reorder later milestones.

## Milestone 1: Two targets run in order

Covers: GRAPH-1, GRAPH-2, GRAPH-3, GRAPH-7, INV-2, INV-4
Done when: a build file with targets lint and test, where test depends on lint, runs both in order from the command line, and a typo in a dependency or flag fails with a named error.
Effort: S

## Milestone 2: Parameters, secrets and failure semantics

Covers: GRAPH-4, GRAPH-5, GRAPH-6, GRAPH-8, GRAPH-9, GRAPH-10, GRAPH-11, GRAPH-12, INV-1, INV-6
Done when: a secret parameter supplied by environment never appears in output, a failing target skips its dependents, parallel mode runs independent targets together, and the machine-readable list prints targets and parameters.
Effort: M

## Milestone 3: Safe shell and the first wrapper

Covers: WRAP-1, WRAP-2, WRAP-3, WRAP-4, WRAP-5, WRAP-6, WRAP-7, WRAP-8, WRAP-13, WRAP-14, INV-5
Done when: one wrapper for the host runtime's own test command passes the conformance check, and a hostile interpolated value stays a single argument.
Effort: M

## Milestone 4: Pinned tool installs

Covers: WRAP-9, WRAP-10, WRAP-11, WRAP-12, WRAP-16, INV-7
Done when: a build installs a pinned tool by checksum, reuses it on the next run, and refuses a tampered download or an archive that escapes its folder.
Effort: M

## Milestone 5: Launcher and global command

Covers: BOOT-1, BOOT-2, BOOT-3, BOOT-4, BOOT-5, BOOT-6, BOOT-7, BOOT-9, INV-8
Done when: a fresh machine with no runtime runs the build through the launcher, and the global command forwards from a subfolder but refuses a root owned by another user.
Effort: M

## Milestone 6: Local incremental cache

Covers: CACHE-1, CACHE-2, CACHE-3, CACHE-4, CACHE-5
Done when: a second run with unchanged inputs reports the target cached, and editing one input or deleting one output makes it run.
Effort: S

## Milestone 7: CI generation

Covers: CIGEN-1, CIGEN-2, CIGEN-3, CIGEN-4, CIGEN-5, CIGEN-6, CIGEN-10, CIGEN-11
Done when: the build writes a pinned workflow for its CI provider, fails on CI when that file is hand-edited, and renders the same declaration for a second provider.
Effort: L

## Milestone 8: Remote cache

Covers: CACHE-6, CACHE-7, CACHE-8, CACHE-9, CACHE-10, CACHE-11, INV-3
Done when: a fresh checkout restores outputs from a shared folder or HTTP store, and a poisoned archive is refused with a rebuild.
Effort: M

## Milestone 9: Durable state, locks and cancel

Covers: DURABLE-1, DURABLE-2, DURABLE-3, DURABLE-4, DURABLE-5, DURABLE-9, DURABLE-13, DURABLE-14
Done when: two runs racing for one lock produce one winner and a typed conflict, and cancel runs compensations in reverse order.
Effort: L

## Milestone 10: Suspend, resume, leases and services

Covers: DURABLE-6, DURABLE-7, DURABLE-8, DURABLE-10, DURABLE-11, DURABLE-12
Done when: a run suspends at a gate, a second process resumes it exactly once, a killed run is recovered by a sweep, and a service is stopped even when tests fail.
Effort: L

## Milestone 11: Schedules and affected targets

Covers: CIGEN-7, CIGEN-8, CIGEN-9
Done when: a daylight-saving schedule renders deterministic UTC crons with a guard, and an affected run skips targets whose inputs did not change.
Effort: M

## Milestone 12: Agent server

Covers: AGENT-1, AGENT-2, AGENT-3, AGENT-4, AGENT-5, AGENT-6, AGENT-7, AGENT-8, AGENT-9, AGENT-12, AGENT-13, AGENT-14
Done when: an agent lists and runs targets over the local and HTTP transports, a protected dependency is denied without the operator token, and the audit trail masks secrets.
Effort: L

## Milestone 13: Build registry

Covers: AGENT-10, AGENT-11, AGENT-16
Done when: a newly registered build is runnable by an already running server, and an unlisted remote module is refused without spawning.
Effort: M

## Milestone 14: AI review gate

Covers: AIREV-1, AIREV-2, AIREV-3, AIREV-4, AIREV-5, AIREV-6, AIREV-7, AIREV-8, AIREV-9, AIREV-12
Done when: a review gates a change request on a high finding, a maintainer's rebuttal dismisses it, and an untrusted rebuttal does not.
Effort: L

## Milestone 15: Self-healing fixer

Covers: AIREV-10, AIREV-11
Done when: a failing test is diagnosed, an allowed edit is applied and verified by re-running the test, and an edit to the lockfile is refused.
Effort: M

## Milestone 16: Release operations

Covers: DELIVER-1, DELIVER-2, DELIVER-3, DELIVER-4, DELIVER-5, DELIVER-6, DELIVER-7, DELIVER-8, DELIVER-9, DELIVER-10
Done when: a canary that fails its analysis rolls back once and releases its lock, and a resumed run exports one trace.
Effort: L
