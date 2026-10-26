# Identity provider

## What the original used

An OAuth issuer and audience validated by a pluggable authenticator on the HTTP transport, plus a static bearer token as the simplest option, and CI actor variables for attribution.

## What a replacement must provide

A way to turn a request into an actor and roles, failing closed on any error.

## Known alternatives

Any OpenID Connect provider, mutual TLS, a reverse proxy that signs identities.

## How to tell it is right

The throwing-authenticator scenario and AGENT-7 hold.
