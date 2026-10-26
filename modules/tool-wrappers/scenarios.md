# Scenarios: typed tool wrappers and safe shell

## Hostile value stays one argument

Given a value containing a semicolon followed by a destructive command
When it is interpolated into an echo command and run
Then the literal text is printed and no other command runs

Covers: WRAP-1

## Dash-prefixed value passes through

Given a value that starts with two dashes
When it is interpolated into a command
Then the child receives it unchanged as one argument

Covers: WRAP-2

## Non-zero exit

Given a command that exits with code 3
When it runs in the default mode
Then an error carrying the exit code is raised; in no-throw mode the result reports code 3 instead

Covers: WRAP-3

## Output cap keeps the tail

Given a child that prints more than the capture cap
When its output is captured
Then only the newest bytes are kept and the result is marked truncated

Covers: WRAP-4

## Timeout escalates

Given a child that ignores the polite termination signal and a short timeout in no-throw mode
When the timeout elapses
Then the child is force-killed and the call returns

Covers: WRAP-5

## Argument vector is pure

Given a wrapper configured with three flags and one extra argument
When its argument vector is built in a test with no tool installed
Then the vector is tool name, the three flags in order, then the extra argument, and nothing was spawned

Covers: WRAP-6, WRAP-14

## Missing binary

Given a wrapper whose tool is not installed
When the wrapper runs
Then a typed not-found error names the tool and the install hint

Covers: WRAP-7

## Project-local binary found two levels up

Given a wrapper in project-local mode and the binary in a project bin folder two directories above the working directory
When it resolves
Then it spawns that binary; with an explicit path configured, the explicit path wins

Covers: WRAP-8

## Checksum mismatch

Given a pinned download whose content hashes differently from the pin
When it is installed
Then the error shows expected and actual hashes, and nothing is left installed

Covers: WRAP-9

## Tampered cached binary

Given a tool installed earlier whose binary was later modified on disk
When the build asks for it again with the same pin
Then the tool is downloaded and verified again rather than reused

Covers: WRAP-10

## Archive escape

Given an archive with one entry pointing through a symbolic link outside the destination
When it is unpacked
Then nothing is written outside the destination, the install fails, and the scratch folder is gone

Covers: WRAP-11

## Credential printed by a tool

Given a wrapper task that returns a token printed by its tool
When a later step logs that token
Then the log shows the redaction marker

Covers: WRAP-12

## Conformance check

Given a new wrapper that forgets to declare its resolution mode
When the shared conformance check runs
Then it fails naming the missing declaration

Covers: WRAP-13
