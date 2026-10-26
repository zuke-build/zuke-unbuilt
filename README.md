# zuke-unbuilt

## What it is

A specification for a code-first, strongly typed build automation system. The build is ordinary code: a type whose members are targets, with dependencies as typed references, parameters and secrets resolved before anything runs, and every external tool reached through a typed wrapper that mirrors the real tool's flags. The same build file generates its own CI configuration, caches unchanged work locally and across machines, survives across processes for long pipelines, and exposes itself safely to AI agents and AI review.

## Who it is for

Teams who are tired of keeping a scripts file, a make-style rules file and CI YAML in sync by hand, and who want build mistakes caught by the type checker instead of in CI. It is also for anyone who wants AI agents to drive builds through a typed, authorised surface rather than by guessing shell commands.

## How to build it

Read `SPEC.md`, `constraints.md` and each module under `modules/`, then follow the milestones in `BUILD.md`. Each substitution point under `substitutions/` names a piece of the original environment you are expected to replace with your own. Use the `/unbuilt:build` skill or the plain prompt `prompts/build.md` from the [unbuilt](https://github.com/vtrifonov/unbuilt) repository to drive a build with an agent. Builds are expected to differ from the original; no conformance is claimed. Extracted with [unbuilt](https://github.com/vtrifonov/unbuilt) 0.1.0.

## Terms

Build your version and share it freely, under any license you choose. No endorsement by the original project is implied.
