# Constraints

- The tool MUST run on Linux, macOS and Windows, and behaviour that crosses process or operating-system boundaries (locks, signals, file ownership, launchers) MUST be tested on all three.
- Developing and using the tool MUST require only one runtime toolchain; no second language toolchain or external build system may be needed.
- The published library MUST NOT ship third-party runtime dependencies.
- Every downloaded executable or runtime MUST be verified against a pinned SHA-256 checksum before it runs.
- Every remote address MUST use an encrypted transport except loopback or an explicit, documented opt-out.
- Secret values MUST NOT be persisted to any store, and MUST be masked in every output channel.
- Tests SHOULD be hermetic and fast: no network, no reliance on tools installed on the machine, subprocesses only through the running runtime itself.
- Line and branch test coverage SHOULD stay at or above 95%, enforced by a gate.
- Every feature SHOULD receive an adversarial security review that tries to break it before it is merged, and each confirmed defect SHOULD get a regression test.
- Generated artifacts (CI files, launchers, API index) SHOULD be checked for drift by a gate rather than trusted to stay in sync.
