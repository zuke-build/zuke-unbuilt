# Typed build model

A build is a type defined in a strongly typed host language. Each target is a named member of that type, built fluently with a description, its dependencies and a body. Dependencies are references to sibling members, never strings.

## Requirements

- **GRAPH-1** (MUST) Targets are discovered from the build's own members in declaration order, and a dependency is a typed reference to a sibling target, so a renamed or misspelled dependency fails type-checking; one target bound to two names is an error.
- **GRAPH-2** (MUST) Running a target first runs all of its transitive dependencies in topological order, and a dependency shared by several dependents runs exactly once per run.
- **GRAPH-3** (MUST) A cycle, a forward reference or an undefined dependency fails before any target runs, with exit code 1 and a message that names the offending targets and, for a cycle, the full path; this holds for every entry point, not only the command line.
- **GRAPH-4** (MUST) Soft ordering hints (run before, run after) only reorder targets already in the plan and never add a target to it.
- **GRAPH-5** (MUST) Every parameter is resolved before any target runs, with precedence command-line flag, then environment variable, then secret source, then declared default; a missing required parameter fails the build and names both its flag and its environment variable.
- **GRAPH-6** (MUST) A secret's resolved value is replaced by a fixed marker in everything the tool prints, including progress lines, the summary, error messages, echoed commands and timeout errors; the value is registered for masking before it is parsed, matching is literal, and a multi-line secret is masked line by line.
- **GRAPH-7** (MUST) An unknown flag or unknown target is a hard error with exit code 1, suggesting the closest valid name when one is near, and nothing runs.
- **GRAPH-8** (MUST) A failing target stops the run: no new target starts, targets already running finish, and every dependent of the failed target is reported as skipped.
- **GRAPH-9** (MUST) Cancelling a run aborts a cancellation signal every target body receives and terminates child processes the run started; once cancelled, no retry or remediation starts.
- **GRAPH-10** (MUST) The user can opt in to parallel execution with a concurrency bound that defaults to the processor count; independent targets then run concurrently, output is buffered per target, and dependency order still holds.
- **GRAPH-11** (SHOULD) A list command prints every target with its description and dependencies, sorted by name, and a machine-readable form of the same list plus all parameters and commands is a stable contract for tools.
- **GRAPH-12** (SHOULD) A target can declare a run condition, a per-attempt timeout, a retry count, that the run proceeds after its failure, and that it always runs once its dependencies settle; the user can skip named targets and rehearse a run without executing bodies.
- **GRAPH-13** (MAY) At an interactive terminal outside CI, a missing required parameter is prompted for instead of failing.
- **GRAPH-14** (MAY) The dependency graph can be rendered as a text listing and as a standalone visual page.
