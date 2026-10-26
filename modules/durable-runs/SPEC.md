# Durable runs

A run can persist its status and per-target state to a pluggable store, take cross-run locks, suspend at a wait gate and be resumed or cancelled later from another process. Long-lived service targets are kept up while their dependents run.

## Requirements

- **DURABLE-1** (MUST) Durable state is opt-in: a build with no store configured writes nothing; the store is chosen by precedence (explicit argument, build override, remote address from the environment, local folder from the environment, default folder flag), and a build that uses locks, waits or cancel hooks gets the local store automatically.
- **DURABLE-2** (MUST) Every state write is a compare-and-swap on an opaque version; of two writers at the same version exactly one wins and the other receives a typed conflict.
- **DURABLE-3** (MUST) The remote store speaks a documented HTTP contract: one JSON document per run; reads return the document with a required entity tag, or not-found; create is conditional on absence and update on the entity tag, failing with a precondition error on mismatch; listing is newest-first, a limit applies after ordering, and without a limit nothing is truncated; every request carries a protocol-version header and a mismatched echo fails the client; lock acquire returns created with a token or conflict with the current holder, renew and release need the token, expiry is decided by the server, and listings never show tokens.
- **DURABLE-4** (MUST) A remote store address without an encrypted transport is refused unless it is loopback or an explicit opt-out is set.
- **DURABLE-5** (MUST) Secret parameter values are never persisted, and per-target state is masked before it is written.
- **DURABLE-6** (MUST) At an unsatisfied wait gate the gate is recorded as waiting and the run as suspended; targets behind the gate stay pending, independent branches finish, and the process exits with code 0.
- **DURABLE-7** (MUST) Resuming moves the run from suspended to running by compare-and-swap, so a racing resumer gets a typed "already resumed" error; the graph must match the recorded one unless forced, and only targets that have not already succeeded run.
- **DURABLE-8** (MUST) A record whose writes were lost is flagged degraded; resume refuses a degraded record without an explicit override, and cancel then compensates every target not recorded as failed or skipped.
- **DURABLE-9** (MUST) A lock is acquired atomically, an expired lock is taken over, the holder renews at half its time-to-live and releases on success, failure or cancel, and a conflict raises a typed error naming the holder's actor and run.
- **DURABLE-10** (MUST) A stateful run holds a heartbeated self-lease taken before its record says running; transient renewal errors are retried, and a refused renewal stops the run without compensating or settling it, because another process now owns it.
- **DURABLE-11** (MUST) A reaper sweep leaves any run with a live lease alone, returns an orphaned running run to suspended and resumes it in the same pass, settles an orphan past its deadline as failed with compensations, and skips runs from a different origin, build or graph without counting them as failures.
- **DURABLE-12** (MUST) A service target starts when a dependent needs it and is polled for readiness up to a timeout, being stopped if it never becomes ready; all services stop in reverse start order when the run ends, even when a dependent failed, and one failing teardown does not block the others.
- **DURABLE-13** (MUST) Cancelling a run runs compensations for succeeded targets only, in reverse order; a throwing compensation does not stop the walk, and cancelling a finished run is a no-op.
- **DURABLE-14** (MUST) A shared conformance kit exercises any third-party store against the contract, including the compare-and-swap and lock rules, and a store is supported only if it passes.
- **DURABLE-15** (SHOULD) A lock can wait for its holder by polling until a deadline, without fairness, printing the holder once per change.
- **DURABLE-16** (SHOULD) A lock can be held across a suspension and is re-claimed by token on resume, which refuses if another run took it.
- **DURABLE-17** (SHOULD) A run deadline excludes time parked at a gate.
- **DURABLE-18** (SHOULD) Commands list, show and prune runs, with filters and machine-readable output, and prune never deletes a run that has not finished.
- **DURABLE-19** (SHOULD) An operator can settle a target as done without running it.
