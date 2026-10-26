# Scenarios: durable runs

## No store, no state

Given a build that declares no store and uses no lock, wait or cancel hook
When it runs
Then no state file or request is written

Covers: DURABLE-1

## Racing writers

Given two writers that both read the same run at the same version
When both write
Then exactly one write lands and the other receives a typed conflict

Covers: DURABLE-2

## HTTP contract

Given a remote store that implements the contract
When a client creates a run, updates it with a stale entity tag, and lists runs with a limit
Then the stale update fails with a precondition error and the listing is newest-first, limited after ordering

Covers: DURABLE-3, DURABLE-14

## Plain-text store refused

Given a store address with an unencrypted scheme to a non-loopback host
When the build starts
Then the store is refused with a message naming the opt-out

Covers: DURABLE-4

## Secret not persisted

Given a run with a secret parameter and a target that stores a value containing the secret
When the stored record is read back
Then the secret parameter is absent and the stored value shows the redaction marker

Covers: DURABLE-5

## Suspend and resume across processes

Given deploy, then a gate waiting for an approval signal, then promote
When the first process reaches the gate and a later process delivers the signal and resumes
Then the first process exits 0 with the run suspended, promote runs once in the later process, and deploy does not run again

Covers: DURABLE-6, DURABLE-7

## Two resumers

Given one suspended run
When two processes resume it at the same time
Then one proceeds and the other fails with "already resumed" and a non-zero exit

Covers: DURABLE-7

## Degraded record

Given a run record flagged degraded
When an operator resumes it without the override
Then the resume is refused, naming the override

Covers: DURABLE-8

## Lock conflict

Given run A holds a lock
When run B requests it without waiting
Then B fails with a typed conflict naming A's actor and run; with waiting, B takes the lock after A releases it

Covers: DURABLE-9, DURABLE-15

## Lost lease

Given a running run whose lease renewal is refused because another process took it over
When the renewal fails
Then the run stops without running compensations

Covers: DURABLE-10

## Reaper

Given one running run whose holder was killed, and one live run past its deadline
When a sweep runs
Then the orphan is returned to suspended and resumed in the same pass, and the live run is left untouched

Covers: DURABLE-11

## Service never ready

Given a service target whose readiness check never passes, and a test target depending on it
When the test target runs
Then the build fails, the test target is skipped, and the service process is stopped

Covers: DURABLE-12

## Cancel compensates in reverse

Given a run where A then B succeeded and C is pending, and B's compensation throws
When the run is cancelled
Then B's compensation runs, then A's, C never runs, and cancelling again is a no-op

Covers: DURABLE-13
