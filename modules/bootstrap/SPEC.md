# Bootstrap and global command

Each project carries small launcher scripts that obtain the pinned runtime and run the build. A separately installed global command scaffolds projects and forwards everything else to the nearest project's build.

## Requirements

- **BOOT-1** (MUST) One template renders both a POSIX shell launcher and a Windows shell launcher; the bootstrap variant downloads a pinned runtime version, verifies its per-platform SHA-256 before use, never runs a downloaded install script, and rejects "latest" or a version override without a matching checksum, while the plain variant fails closed with install instructions when no runtime is found.
- **BOOT-2** (MUST) The launcher runs the build with a frozen lockfile once a lockfile exists, and the unfrozen path prints a warning on standard error.
- **BOOT-3** (MUST) The project's own launchers are generated from the same template and a gate fails when they drift from it; the generator replaces a launcher by rename, never by rewriting it in place.
- **BOOT-4** (MUST) The global command forwards any command it does not own to the nearest ancestor folder holding a project configuration, runs that project's launcher from the project root, inherits standard streams, propagates the build's exit code, and names the build on standard error when it is not in the current folder.
- **BOOT-5** (MUST) Forwarding passes a trust gate modelled on per-directory ownership checks: it refuses a project root owned by another user or writable by everyone, root build, configuration, lock or launcher files with the same faults, runtime configuration files in ancestor folders owned by another user, and a symbolic-link launcher whose target or target folder fails the same checks; it names the remedy, and it is inert where the platform has no file ownership.
- **BOOT-6** (MUST) The forwarded child receives a marker environment variable so a launcher does not forward again.
- **BOOT-7** (MUST) The global command's reserved words take precedence over build targets of the same name, and arguments after a double-dash separator reach the build unchanged.
- **BOOT-8** (MUST) Self-upgrade refuses an installation that did not come from the package registry (pointing to the right package manager), a missing global install, and an unpublished or withdrawn version, and verifies the version it resolved.
- **BOOT-9** (MUST) Project setup never overwrites an existing file unless forced, and appends the tool's state folder to an existing ignore file only once.
- **BOOT-10** (SHOULD) Help output merges the global command's help with the current build's targets and parameters.
- **BOOT-11** (SHOULD) Setup offers an interactive wizard, including registering the build with an agent server, and a custom launcher name.
- **BOOT-12** (SHOULD) Commands add and remove packages and refresh the lockfile, keeping a backup and restoring it on failure.
- **BOOT-13** (MAY) An importer turns each existing package script or make-style rule into a target, marks shell-only commands as to-do, breaks cycles with a note, and refuses a source that is a symbolic link.
- **BOOT-14** (MAY) Shell completions for several shells can be printed or installed idempotently, with flags taken from the same registry the parser uses.
