# Traffic router

## What the original used

Platform adapters for serverless containers, Kubernetes, Helm, Compose, function aliases and container apps, each in its own wrapper package.

## What a replacement must provide

Stage a candidate, set an exposure percentage and report the exposure achieved, promote, and roll back idempotently.

## Known alternatives

A service mesh, a load balancer with weighted targets, a feature-flag service.

## How to tell it is right

The DELIVER-1 to DELIVER-6 scenarios pass against it.
