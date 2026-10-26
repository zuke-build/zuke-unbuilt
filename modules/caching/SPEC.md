# Incremental and remote cache

A target may declare inputs (files, folders, globs), extra cache keys (any values) and outputs. The tool skips a target whose inputs are unchanged, and can restore its outputs from a store shared across machines.

## Requirements

- **CACHE-1** (MUST) A target's fingerprint is a SHA-256 over its declared inputs in declaration order, where a file contributes its content, a folder its sorted entries recursively, and a missing path a fixed sentinel, so a file appearing or disappearing changes the fingerprint; declared cache keys are mixed in.
- **CACHE-2** (MUST) A target is skipped as cached only when its fingerprint equals the one from its last successful run and every declared output still exists; a target with neither inputs nor cache keys always runs.
- **CACHE-3** (MUST) A fingerprint is recorded only after the target succeeds, and a cancelled run that ran compensations leaves the store untouched.
- **CACHE-4** (MUST) The local store is best-effort: a missing or corrupt store reads as empty, and it is written only when something changed.
- **CACHE-5** (MUST) A cached target counts as satisfied for its dependents; a rehearsal run never reads or writes the cache, and a no-cache flag disables both the local and the remote tier.
- **CACHE-6** (MUST) The remote tier applies only to targets with both inputs and outputs, in the order local hit, then remote restore marked cached, then run and upload; outputs just restored are not uploaded again.
- **CACHE-7** (MUST) A remote lookup failure, upload failure or refused archive is a warning followed by a normal rebuild, never a build failure.
- **CACHE-8** (MUST) A restore validates every archive entry before writing any, and refuses the whole archive when an entry is absolute or climbs with a parent segment, is a link, lands under the version-control folder or the tool's own state folder, falls outside the target's declared outputs by path segment, or would be written through a symbolic link already on disk.
- **CACHE-9** (MUST) The downloaded size and the inflated size are capped separately, and the inflated size is counted while decompressing.
- **CACHE-10** (MUST) A remote backend address must use an encrypted transport, except for loopback or an explicit, documented opt-out.
- **CACHE-11** (SHOULD) The remote backend is chosen by precedence: explicit build option, then build override, then environment, and the key is the sanitised target name plus its fingerprint.
- **CACHE-12** (SHOULD) Archives keep file permission bits and reject a path too long for the format rather than truncate it.
- **CACHE-13** (SHOULD) An HTTP backend can send extra headers and keep a query string on its base address, for signed URLs.
