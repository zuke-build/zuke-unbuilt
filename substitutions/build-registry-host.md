# Build registry host

## What the original used

A filesystem catalogue and a hosted registry service implementing a REST contract.

## What a replacement must provide

Register, list and look up build descriptors, with an encrypted transport.

## Known alternatives

A database table, a shared folder, an internal service catalogue.

## How to tell it is right

The AGENT-10, AGENT-11 and AGENT-16 scenarios pass.
