# Scenarios: release operations

## Failed analysis rolls back

Given a rollout at its second exposure step and an analysis that trips
When the analysis fails
Then the rollback runs once with the recorded state, the lock is released, and the run fails

Covers: DELIVER-1, DELIVER-3, DELIVER-5

## Cancel a parked rollout

Given a rollout parked waiting for approval
When an operator cancels it from another process
Then the same rollback runs and the lock is released

Covers: DELIVER-1, DELIVER-12

## Cancel after promotion

Given a promoted release
When a later cancel arrives for its run
Then nothing is rolled back

Covers: DELIVER-2

## Lock refused

Given another run holds the rollout lock
When a second rollout starts
Then it fails without staging or changing exposure

Covers: DELIVER-2, DELIVER-5

## No state store

Given a build without a durable store
When a canary target runs
Then it is refused before staging

Covers: DELIVER-4

## Router reports impossible exposure

Given a router that reports 140 percent exposure
When the step completes
Then the analysis fails and the rollout rolls back

Covers: DELIVER-6

## Empty metric result

Given a metric query that returns no samples
When the analysis runs
Then it fails

Covers: DELIVER-6

## Credentialed metric address

Given a metric address with embedded credentials and a query that fails
When the failure is reported
Then the message shows neither the credentials nor any secret value, and a redirect from the backend is not followed

Covers: DELIVER-7

## One trace across a suspension

Given a run that suspends at a gate and is resumed a day later
When it settles
Then one trace holds spans from before and after the wait

Covers: DELIVER-8

## Collector down

Given an endpoint that refuses connections
When the run ends
Then the build still succeeds within the export timeout; with no endpoint, nothing is attempted

Covers: DELIVER-9

## Exported once

Given a run that is cancelling and then cancelled
When it settles
Then its final trace is exported exactly once

Covers: DELIVER-10
