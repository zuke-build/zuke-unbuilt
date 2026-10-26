# CI generation

The build declares its CI pipeline in the same typed file as its targets. The tool renders provider-specific workflow files from that declaration, keeps them in sync, and checks them on CI.

## Requirements

- **CIGEN-1** (MUST) The pipeline is declared inside the build with the provider as the only required field; the defaults are a pipeline named CI, triggered on pushes and change requests to the main branch, with one job that runs the build through its launcher.
- **CIGEN-2** (MUST) Running any target locally regenerates the declared CI files; on CI the run only verifies them and fails naming each stale file, and a check flag does the same locally; line-ending differences are not drift.
- **CIGEN-3** (MUST) In fan-out mode the generator emits one job per runnable target with dependency edges from its dependencies, dropping edges to omitted targets and skipping targets without a body.
- **CIGEN-4** (MUST) Every generated job starts with a prelude of a hardening step and a checkout step pinned to an immutable commit identifier read from one pin manifest; a missing pin is a generation error, and no unpinned step reference is ever emitted.
- **CIGEN-5** (MUST) A declared field that the chosen provider cannot express is a generation error, never silently dropped.
- **CIGEN-6** (MUST) Matrices apply exclusions before inclusions, and an exclusion key that is not an axis, an empty resulting matrix, or a partial operating-system setting is an error.
- **CIGEN-7** (MUST) A schedule written as a cron expression plus a named time zone from the standard time-zone database compiles to UTC cron expressions: one for a fixed-offset zone, one per distinct offset for a daylight-saving zone, with offsets sampled over every day of a pinned reference year so output is deterministic.
- **CIGEN-8** (MUST) For a daylight-saving zone a guard job checks the local wall-clock time and every other job depends on it; events that are not schedules always pass the guard; unsupported cron grammar, fractional-hour zones and day-constrained schedules that cross midnight fail with a message telling the user what to write instead.
- **CIGEN-9** (MUST) In affected mode, given a base revision, a target runs when a changed file lies inside its declared inputs or any upstream dependency is affected; a target with no declared inputs always runs, and a base revision that looks like a command option is rejected.
- **CIGEN-10** (MUST) A job summary is written only on the CI host, with secrets masked, never fails the build when it cannot be written, and escapes target names and output so that neither can break the summary's table nor emit a CI workflow command.
- **CIGEN-11** (MUST) One declaration renders for at least four CI providers, and each provider's limits are enforced as in CIGEN-5.
- **CIGEN-12** (SHOULD) A pipeline can declare environments, job outputs, path filters, per-job permissions and concurrency groups.
- **CIGEN-13** (SHOULD) Jobs can be wired explicitly instead of fanned out, and the two modes are mutually exclusive.
- **CIGEN-14** (SHOULD) Emitted workflow text quotes any scalar another reader could type differently.
- **CIGEN-15** (MAY) The CI integration can expose the provider's cache service as the build's remote cache.
