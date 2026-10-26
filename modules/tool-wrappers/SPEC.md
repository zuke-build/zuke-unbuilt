# Typed tool wrappers and safe shell

Every external tool is reached through a wrapper: a namespaced group of tasks configured with a settings function whose setters mirror the real tool's flags. A low-level shell primitive underneath runs processes from argument vectors, never from command strings.

## Requirements

- **WRAP-1** (MUST) Values interpolated into a shell command become separate argument-vector entries with no shell interpreter involved; a list expands to several arguments, and a value is never re-split on whitespace.
- **WRAP-2** (MUST) A value that starts with a dash is passed through unchanged; the shell primitive does not claim to prevent argument injection, and its documentation says validation is the caller's job.
- **WRAP-3** (MUST) A non-zero exit code raises an error by default; an explicit no-throw mode returns the result instead, and asking only for the exit code never raises.
- **WRAP-4** (MUST) Captured standard output and standard error are each capped, keeping the newest bytes and marking the result as truncated; the cap is a positive whole number with no unlimited setting, and live streaming to the terminal is not capped.
- **WRAP-5** (MUST) A timeout terminates the child process even in no-throw mode, and escalates to a forced kill if the polite termination is ignored.
- **WRAP-6** (MUST) Building a wrapper's argument vector is pure, with no file, network or process access, so it can be unit-tested; the order is tool name, built arguments, then extra arguments.
- **WRAP-7** (MUST) A missing binary raises a typed not-found error naming the tool and how to install it; any other spawn error propagates unchanged.
- **WRAP-8** (MUST) Tool resolution order is an explicit path, then a per-call resolution mode, then a repository-wide environment setting, then the wrapper's declared default; project-local resolution walks up from the working directory and falls back to the system path.
- **WRAP-9** (MUST) A pinned tool install requires a SHA-256 checksum, validated in shape before any download; a mismatch installs nothing and removes partial files.
- **WRAP-10** (MUST) A previously installed tool is reused only when its pin matches and the binary on disk still hashes to the recorded value.
- **WRAP-11** (MUST) Unpacking a downloaded archive never writes outside the destination, whether through relative paths, a symbolic-link leaf or a symbolic-link ancestor; encrypted and oversized-format archives are rejected, and scratch space is removed even on failure.
- **WRAP-12** (MUST) A wrapper whose command prints a credential registers that output for masking before it is returned.
- **WRAP-13** (MUST) Every wrapper passes a shared conformance check that, without spawning the real tool, asserts the binary it runs, its declared resolution mode and its not-found behaviour; a wrapper must declare its resolution mode rather than inherit one.
- **WRAP-14** (MUST) A wrapper's task names and setters mirror the real tool's subcommands and flags, not invented aliases.
- **WRAP-15** (SHOULD) A build can define an ad-hoc wrapper for an unwrapped tool, with flag and option helpers that add the dash prefix unless one is present.
- **WRAP-16** (SHOULD) Several pinned tools can be installed concurrently, returning a map from tool name to installed path, with per-platform download and checksum selection.
- **WRAP-17** (SHOULD) Output helpers return trimmed text, or a list of lines that is empty for empty output.
