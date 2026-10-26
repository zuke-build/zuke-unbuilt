# Known failures

What the original build tried, or shipped, that did not work, and why.

## Typed build model

- A required list parameter quietly resolved to an empty list instead of failing. Required means required for every parameter kind.
- A forward reference crashed with a raw type error when the build was entered through a path other than the command line. Graph validation belongs in the shared core, not in one entry point.
- A multi-line secret leaked after its first line, because masking matched the whole value only. Mask line by line.
- Unknown flags were once ignored, so a misspelled rehearsal flag ran the real build.

## Typed tool wrappers and safe shell

- Archive extraction followed a symbolic link at the leaf of an entry and wrote outside the destination. The fix then caused a large redundant file-status regression. Check each path once, ancestors included.
- Windows command shims and their retry path misbehaved. Test shim handling on Windows, not only on POSIX.
- Ecosystem tools first resolved from the system path and picked up the wrong version. They had to default to project-local resolution.
- A secret reshaped by a query, so that its printed form differed from its stored form, escaped masking.
- With no checksum supplied, a stale cached binary is reused silently. The documentation now warns about it.

## Incremental and remote cache

- A poisoned archive could plant a configuration file or a lockfile next to the real outputs. Restores must be confined to the declared outputs.
- Restores wrote through symbolic links already in the workspace, and archive size was unbounded. After the fix, a target whose outputs are symbolic links always rebuilds.
- One object store answers a missing key with "forbidden" when the credential lacks list permission. That looks like a lookup failure, not a miss.
- An early AI response cache hashed the wrong fields, so different prompts shared a key.

## Durable runs

- A queued state write was destroyed by a later conflict and nobody was told. The degraded flag exists because of this.
- A process that lost its lease rolled back a run another process had already taken over. A lost lease means stop, not compensate.
- A gate's waiting status was not settled when its run settled, so finished runs showed a gate still waiting.
- Parameter overrides given at resume were missing from the audit trail. The first fix wrote them back over the run's recorded parameters, so a later rollback would have used the wrong environment. It was replaced.
- Wait gates inside a fan-out were ambiguous to resume and are rejected.
- On Windows, an access-denied error on the mutex marker file had to be treated as contention, not as a failure.
- A cancel from another process can briefly overlap a long body that does not check its cancellation signal. This is documented, not fixed.

## CI generation

- The job summary was written even in console modes that should have suppressed it.
- A hostile target name broke out of its cell in the summary table.
- The summary was not masked, so secrets could reach it.
- The pin check hashed the whole action manifest, not the inputs the workflows actually use, so harmless upstream edits failed the gate.
- Fan-out jobs added a second, unpinned checkout after the pinned prelude.
- A schedule in the hour repeated on the fall-back day can fire twice. This is documented, not fixed.

## Bootstrap and global command

- Self-upgrade installed an older release on a newer runtime, because the runtime's minimum-dependency-age filter hid the newest release.
- Scaffolded projects first inlined full package specifiers in imports and failed the project's own lint. They needed bare specifiers mapped in the configuration.
- Command output could forge CI workflow commands.
- The first bootstrap trusted the download. Checksum verification was added later and then hardened.
- Overwriting a running POSIX shell launcher in place corrupts the running script, because the shell reads it incrementally. Write by rename.

## Agent surface

- An adversarial review found an authorisation bypass, a gap in secret masking and a throw that crashed the transport. All three had passed lint, type-checking and a 95% coverage gate. An adversarial pass is now part of every feature.
- Origin validation and the encrypted-transport requirement came late. Until then, an on-path attacker could choose the commands a registry launched.
- Registry launch, spawn and gate paths were hardened in a later round.
- Parameter overrides supplied at resume were missing from the audit trail.

## AI review and self-healing

- A provider credential was sent in a URL query string, where proxies and logs record it.
- Path-matching magic in a model-proposed edit path swept the whole tree into one commit.
- Model text broke out of the Markdown it was posted in.
- A fence marker could be forged through a candidate finding's title.
- What the reviewer posted was not masked until the core gave every validation the run's masking function.
- Reworded findings dodged dismissals until identity resolution and inheritance across reviewers were added.
- Widening the suppression fingerprint invalidated every existing suppression. The documented one-time migration fails safe.
- A review started by a comment runs with secrets. This is an accepted trade-off: the change's code is fetched as data only, and the commenter's write permission is checked first.

## Release operations

- The rollback hook could not tell a real rollback from a hand-started run, so it acted on empty state.
- A rollback for one container platform took down the serving canary, or miscounted stopped containers.
- A call to one serverless platform could hit a service that was never staged.
- A durable-bake timing test was flaky because it slept instead of waiting for the tick.
- Documentation pointed metric-backend authentication at a raw header instead of the connection settings.
