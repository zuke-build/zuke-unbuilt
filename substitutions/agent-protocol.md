# Agent protocol

## What the original used

The Model Context Protocol, over a standard-stream transport and a streamable HTTP transport, supporting a fixed set of protocol revisions.

## What a replacement must provide

Tool listing with input schemas, tool calls with structured results, and a version handshake.

## Known alternatives

Any agent tool-calling protocol with typed schemas.

## How to tell it is right

The AGENT-1 to AGENT-9 scenarios pass over both transports.
