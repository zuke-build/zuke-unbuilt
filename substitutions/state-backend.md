# State backend

## What the original used

A local folder store using atomic rename plus an exclusive-create mutex file (single host only), and a hosted HTTP store speaking the documented contract with bearer authentication and entity tags.

## What a replacement must provide

Compare-and-swap writes on an opaque version, newest-first listing, and atomic locks with server-side expiry, exactly as in DURABLE-2 and DURABLE-3.

## Known alternatives

A relational database with row versions, a key-value store with conditional writes, an object store with conditional puts.

## How to tell it is right

It passes the shared conformance kit (DURABLE-14), including the racing-writers scenario.
