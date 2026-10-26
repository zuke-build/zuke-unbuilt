# Scenarios: incremental and remote cache

## Unchanged inputs are cached

Given a target with declared inputs and outputs that succeeded once
When it is run again with nothing changed
Then it is reported cached and its body does not run; after one input file is edited, it runs

Covers: CACHE-1, CACHE-2

## Deleted output forces a rebuild

Given unchanged inputs but one declared output deleted
When the target is run
Then it rebuilds

Covers: CACHE-2

## Failure records nothing

Given a target whose body fails
When it is run twice with the same inputs
Then it runs both times

Covers: CACHE-3

## Corrupt store

Given a local store file filled with garbage
When the build runs
Then every target runs as on a first run and the build succeeds

Covers: CACHE-4

## Dependents of a cached target

Given B depends on A, and A is cached
When B runs
Then B runs normally; with the no-cache flag, A runs too; in a rehearsal run, the store is unchanged

Covers: CACHE-5

## Fresh checkout restores from the shared store

Given a populated shared store and a fresh checkout
When the target runs
Then its outputs are restored, it is reported cached, its body never runs, and nothing is uploaded

Covers: CACHE-6

## Unreachable store

Given a shared store that refuses connections
When the build runs
Then the target runs, a warning is printed, and the build succeeds

Covers: CACHE-7

## Poisoned archive

Given a stored archive holding the declared output plus one file outside the declared outputs
When it is restored
Then nothing at all is written, a warning is printed, and the target rebuilds

Covers: CACHE-8, CACHE-7, INV-7

## Decompression bomb

Given an archive that is small on the wire but inflates past the inflated cap
When it is restored
Then decompression stops at the cap and the target rebuilds

Covers: CACHE-9

## Plain-text store address

Given a store address using an unencrypted scheme to a non-loopback host
When the build starts
Then it refuses the backend with a message naming the opt-out

Covers: CACHE-10, INV-3
