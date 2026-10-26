# Agent surface

The tool exposes a build to AI agents as typed tools over an agent tool-calling protocol, through a local stream transport and an HTTP transport. A registry lets one server discover many builds. A generated API index and packaged agent skills tell agents the exact API instead of letting them guess.

## Requirements

- **AGENT-1** (MUST) Tools that run targets are hidden and refused unless execution was explicitly enabled when the server started, and the refusal names the enabling flag.
- **AGENT-2** (MUST) With an allow-list, a target outside it is reported exactly like a tool that does not exist, and the read-only tools describe only the allow-listed targets and their dependency closure.
- **AGENT-3** (MUST) Protection, operator tokens and required roles are evaluated over the whole plan a call would run, dependencies and triggered targets included; a plan that cannot be resolved is denied, and a server with no operator token configured denies every protected target.
- **AGENT-4** (MUST) A destructive run returns its plan without running until the call carries an explicit confirmation, never shows a protected plan before the token is checked, and exempts targets marked read-only.
- **AGENT-5** (MUST) The HTTP transport binds to loopback by default and refuses to bind elsewhere unless a bearer token or an authenticator is configured; a hook that trusts request headers does not count.
- **AGENT-6** (MUST) HTTP requests are checked in a fixed order (metadata document, method, origin, token, authenticator, protocol version, body size cap of one mebibyte), so authentication completes before the body is read.
- **AGENT-7** (MUST) The authenticator fails closed: a throw, a non-object, an empty actor or a malformed rejection becomes a bare unauthorised response, and a rejection status outside the client-error range collapses to unauthorised.
- **AGENT-8** (MUST) Every mutating or denied call is written to an audit trail with the actor, the roles held, the outcome and masked arguments, with undeclared argument values elided; the trail cannot be read through the agent protocol under any spelling of its name.
- **AGENT-9** (MUST) Secret values never reach tool output, the audit trail or the machine-readable description of the command line, including a secret's default or allowed values.
- **AGENT-10** (MUST) A registry launch refuses a remote build module whose host is not allow-listed, requires an exact case-folded program match for command locations, audits refusals and spawns nothing for them, and starts each child with an environment stripped of the server's own credentials.
- **AGENT-11** (MUST) The registry's run tool validates parameters by kind before spawning and caps concurrent spawns, answering a call over the cap at once with a structured busy error.
- **AGENT-12** (MUST) Protected-resource metadata is served at exactly one well-known path inserted between host and path, and a resource identifier that carries credentials is refused.
- **AGENT-13** (MUST) A generated index of the whole public API, for agents to read, is produced from source, and a gate fails when it drifts from source.
- **AGENT-14** (MUST) Each run tool's input schema is derived from the target's declared parameters (required flags, enumerations, numeric types) plus a rehearsal flag, and the tool carries read-only and destructive hints from the target.
- **AGENT-15** (SHOULD) Typed failures come back as structured results inside the tool response, not as transport errors.
- **AGENT-16** (SHOULD) A registry-backed server re-reads its catalogue on every list or call, so a newly registered build is usable without a restart.
- **AGENT-17** (SHOULD) A per-build authorisation override can narrow a decision but never widen it.
- **AGENT-18** (MAY) Agent skills that teach the build API are authored once, packaged for several agent harnesses, and validated against the skills specification.
