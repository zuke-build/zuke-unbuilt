# Typed build automation

## Product goal

A build automation system in which the build is ordinary, strongly typed code. A build is a type whose members are targets; dependencies are typed references, so renames and typos fail type-checking instead of failing in CI. Every external tool is reached through a typed wrapper that mirrors the real tool's flags. The same build file runs locally and on CI, generates its own CI configuration, caches unchanged work, survives across processes for long pipelines, and exposes itself safely to AI agents and AI review, without any of these leaking a secret.

## Glossary

- **Build**: a type in the host language whose members are targets and parameters; one file, runnable on its own.
- **Target**: a named unit of work with a description, dependencies and a body.
- **Plan**: the ordered set of targets a run will execute, after dependency expansion.
- **Parameter**: a typed build input resolved from a flag, an environment variable, a secret source or a default.
- **Secret**: a parameter whose value is masked in every output.
- **Wrapper**: a typed group of tasks around one external command-line tool, configured with a settings function.
- **Settings function**: a function that receives a fluent settings object and returns it configured; evaluated at call time.
- **Launcher**: a small script committed to a project that obtains the pinned runtime and runs the build.
- **Store**: a durable backend for run state and locks, local or remote.
- **Gate**: a point in a run that waits for an external event; reaching an unsatisfied gate suspends the run.
- **Compensation**: an undo step for a succeeded target, run in reverse order when a run is cancelled.
- **Finding**: one issue raised by an AI review, with a stable identifier.

## Invariants

- **INV-1** (MUST) Every secret value is masked in every output channel the tool writes to: terminal, run summary, CI job summary, persisted state, audit trail, change-request comments and telemetry.
- **INV-2** (MUST) Targets, parameters and tool flags are typed references in the host language; the shipped library contains no untyped escape hatches or forced type casts.
- **INV-3** (MUST) Every remote backend (cache, state, registry, metrics, telemetry with credentials) refuses an unencrypted address unless it is loopback or an explicit, documented opt-out is set.
- **INV-4** (MUST) Output is deterministic: ordering and generated files are stable across runs and machines, and every validation error names the offending target and how to fix it.
- **INV-5** (MUST) The published library has no third-party runtime dependencies.
- **INV-6** (MUST) Each security guard or escaper (masking, path-confinement, loopback check, markup escaping) has exactly one implementation, reused by every caller.
- **INV-7** (MUST) Untrusted input (downloaded archives, change-request text, model output, store contents) is treated as data: it never becomes an instruction and never escapes the folder or fence it was confined to.
- **INV-8** (MUST) Authentication, trust and integrity checks fail closed on any error; only caches and telemetry are best-effort.

## Exclusions

- No baseline-versus-canary statistical comparison: canary analyses are absolute thresholds per check.
- Traces are exported only when a run settles; in-flight or parked runs are not exported as spans.
- Telemetry is exported only as JSON over HTTP: no binary encoding, no streaming RPC transport, no logs signal.
- Real traffic routing is not part of the canary component; it sits behind the traffic-router substitution point.
- No visual user interface beyond terminal output and an optional standalone graph page.
- No package distribution for a second language ecosystem.
