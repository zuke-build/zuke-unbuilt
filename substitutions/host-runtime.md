# Host runtime

## What the original used

A TypeScript runtime (Deno) with built-in type-checker, test runner, formatter and linter, its process-spawning API, POSIX signals, and async-context storage for the per-run cancellation signal. Packages were published to its registry (JSR).

## What a replacement must provide

A strongly typed language whose build types can hold targets as members discovered in declaration order; a process API that spawns from argument vectors without a shell and can terminate a child politely, then by force; a per-run ambient context for cancellation; one toolchain for type-checking, tests and formatting.

## Known alternatives

Any statically typed language with reflection over members or a registration step, such as a compiled language on a managed or native runtime, with a small launcher.

## How to tell it is right

GRAPH-1 holds: renaming a dependency target fails type-checking. WRAP-1 and WRAP-5 scenarios pass on all three operating systems.
