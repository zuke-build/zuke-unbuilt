# CI provider

## What the original used

GitHub Actions as the primary provider, with GitLab CI, Azure Pipelines and Bitbucket Pipelines also rendered. Every GitHub job opened with a hardening step and a checkout step, both pinned by commit SHA in one hand-maintained action manifest bumped by a dependency bot. CI was detected from provider environment variables, and the job summary and annotation channels were provider-specific.

## What a replacement must provide

A workflow file format the generator can render deterministically, a way to pin every third-party step immutably, a detection signal, and optionally a summary channel.

## Known alternatives

Any hosted or self-hosted CI with file-based configuration.

## How to tell it is right

The CIGEN-1 to CIGEN-5 scenarios pass for it, and generated files survive a round trip through the drift check.
