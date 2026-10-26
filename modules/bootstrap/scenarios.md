# Scenarios: bootstrap and global command

## Bootstrap rejects a bad runtime download

Given a launcher pinned to a runtime version and checksum, and a download whose hash differs
When the launcher runs on a machine without the runtime
Then it stops with a checksum error and runs nothing

Covers: BOOT-1

## Frozen once a lockfile exists

Given a project without a lockfile
When the launcher runs
Then the build runs unfrozen and a warning is printed; once a lockfile exists, the next run is frozen

Covers: BOOT-2

## Launcher drift

Given a committed launcher edited by hand
When the gate runs
Then it fails naming the launcher

Covers: BOOT-3

## Run from a subfolder

Given a project and a nested subfolder
When the user runs a target name through the global command from the subfolder
Then the build at the project root runs, standard error names the build, and a failing target's exit code is returned

Covers: BOOT-4

## Outside any project

Given a folder with no project above it
When the user runs a target name through the global command
Then it reports an unknown command and spawns nothing

Covers: BOOT-4

## Untrusted root

Given a project root owned by another user, or writable by everyone
When the user forwards a command into it
Then forwarding is refused and the message names the remedy

Covers: BOOT-5, INV-8

## No recursion

Given a launcher that itself calls the global command
When the forwarded build starts
Then the marker variable is set and no second forward happens

Covers: BOOT-6

## Reserved word and separator

Given a build with a target named like a global command, and arguments after a double dash
When the user runs the global command
Then the global command's own meaning wins, and the arguments after the separator reach the build

Covers: BOOT-7

## Upgrade from the wrong source

Given the global command installed by an operating-system package manager
When the user asks it to upgrade itself
Then it refuses and names the package manager to use

Covers: BOOT-8

## Setup keeps existing files

Given a project with an existing build file
When setup runs without force
Then the file is untouched; with force, it is overwritten; the ignore entry is added once across both runs

Covers: BOOT-9
