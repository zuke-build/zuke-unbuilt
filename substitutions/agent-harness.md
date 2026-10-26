# Agent harness

## What the original used

Claude Code, OpenAI Codex and Gemini CLI, each loading the same agent skills through its own plugin manifest; the self-healing fixer could also drive a coding agent.

## What a replacement must provide

A way to load instructions that teach an agent the build API, and for the fixer, a way to run an agent within a path allow-list.

## Known alternatives

Any coding agent that reads skill or instruction files.

## How to tell it is right

An agent given only the skills writes a build that type-checks first time, and AIREV-11 holds for the fixer.
