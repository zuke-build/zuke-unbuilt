# Remote cache backend

## What the original used

A local folder, a generic HTTP store answering GET and PUT, S3-compatible object storage signed with SigV4, and the GitHub Actions cache service.

## What a replacement must provide

Get and put of an opaque archive by key, an encrypted transport, and a clear "not found" distinct from an error.

## Known alternatives

Any object store, a shared network folder, another CI provider's cache service.

## How to tell it is right

The CACHE-6 to CACHE-10 scenarios pass against it, including the unreachable-store and poisoned-archive scenarios.
