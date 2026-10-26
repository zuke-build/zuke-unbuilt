# Scenarios: typed build model

## Diamond dependency runs once

Given target A depends on B and C, and both B and C depend on D
When the user runs A
Then D runs exactly once, before B and C, and A runs last

Covers: GRAPH-1, GRAPH-2, INV-2

## Cycle is rejected before anything runs

Given two targets that each depend on the other
When the build is invoked for either target
Then it exits with code 1, the message shows the cycle path, and no target body runs

Covers: GRAPH-3, INV-4

## Forward reference is a friendly error

Given a target that depends on a sibling declared below it
When the build is loaded through any entry point
Then the build fails before running, naming both targets and saying to move the dependency above its dependent

Covers: GRAPH-3

## Soft hint does not pull a target in

Given target X is declared to run after target Y, and nothing depends on Y
When the user runs only X
Then Y does not run

Covers: GRAPH-4

## Flag beats environment

Given a parameter supplied both as a flag and as an environment variable
When the build resolves parameters
Then the flag value is used, and resolution finishes before the first target starts

Covers: GRAPH-5

## Missing required parameter

Given a required parameter with no flag, variable, source or default
When the build runs non-interactively
Then it fails before any target runs, and the message names the flag and the environment variable

Covers: GRAPH-5

## Secret in an error message

Given a secret parameter and a target that fails with a message containing the secret's value, one line of which is in a multi-line secret
When the failure is reported
Then every occurrence of each line of the value is shown as the redaction marker

Covers: GRAPH-6, INV-1

## Mistyped flag

Given the user types a dry-run flag with one letter missing
When the build is invoked
Then it exits with code 1, suggests the correct flag, and runs nothing

Covers: GRAPH-7

## Failure skips dependents

Given targets A and B are independent, C depends on A, and A fails while B is running in parallel
When the run settles
Then B finishes, C is reported skipped, and the run exits non-zero

Covers: GRAPH-8, GRAPH-10

## Cancel kills child processes

Given a target body running a long child process
When the run is cancelled
Then the body's cancellation signal is aborted, the child process terminates, and no retry starts

Covers: GRAPH-9

## Machine-readable list

Given a build with targets and parameters
When the user asks for the machine-readable list
Then the output holds every target with description and dependencies, sorted by name, plus every parameter

Covers: GRAPH-11

## Published library has no third-party dependency

Given the published library's package manifests
When their runtime dependencies are listed
Then none is a third-party package

Covers: INV-5
