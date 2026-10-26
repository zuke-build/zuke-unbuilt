# Scenarios: CI generation

## Minimal declaration

Given a build that declares a CI pipeline with only a provider
When any target runs locally
Then a workflow named CI appears, triggered on push and change request to the main branch, with one job running the launcher

Covers: CIGEN-1

## Hand edit detected on CI

Given a committed workflow file someone edited by hand
When the build runs on CI
Then it fails naming the stale file and does not rewrite it; a copy that differs only in line endings passes

Covers: CIGEN-2

## Fan-out

Given lint, and test depending on lint, in fan-out mode
When the workflow is generated
Then it has two jobs, and the test job needs the lint job

Covers: CIGEN-3

## Missing pin

Given a pin manifest without an entry for the checkout step
When the workflow is generated
Then generation fails naming the step, and no file with an unpinned reference is written

Covers: CIGEN-4

## Unsupported field

Given a pipeline that sets a field the chosen provider cannot express
When it is generated
Then generation fails naming the field and the provider

Covers: CIGEN-5, CIGEN-11

## Same declaration, another provider

Given one pipeline declaration
When it is generated for each supported provider
Then each provider's file expresses the same jobs and dependency edges

Covers: CIGEN-11

## Matrix exclusion

Given a matrix whose exclusions remove every combination
When it is generated
Then generation fails saying the matrix is empty

Covers: CIGEN-6

## Daylight-saving schedule

Given a weekday-morning schedule in a zone with daylight-saving time
When it is generated
Then two UTC cron expressions and a guard job are emitted; a push event passes the guard

Covers: CIGEN-7, CIGEN-8, INV-4

## Half-hour zone

Given a schedule in a zone with a half-hour offset
When it is generated
Then generation fails and tells the user to write a UTC cron

Covers: CIGEN-8

## Affected targets

Given two targets with disjoint inputs, a third depending on the first, and a change only under the first one's inputs
When an affected run is made against the base revision
Then the first and third run and the second is skipped

Covers: CIGEN-9

## Hostile target name in summary

Given a target whose name contains table delimiters and a line that looks like a CI workflow command
When the summary is written on the CI host
Then the name stays in its cell and no workflow command takes effect

Covers: CIGEN-10
