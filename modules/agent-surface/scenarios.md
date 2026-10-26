# Scenarios: agent surface

## Execution not enabled

Given a server started without enabling execution
When an agent calls a run tool
Then the call is refused and the message names the enabling flag

Covers: AGENT-1

## Allow-listed closure

Given an allow-list holding only target B, which depends on A
When an agent lists tools and calls a run tool for C
Then the listing describes only A and B, and the call for C fails exactly as for a non-existent tool

Covers: AGENT-2

## Protected dependency

Given target A is protected and B depends on A
When an agent calls B without the operator token
Then the call is denied as unauthorised, and B's schema lists the token as required

Covers: AGENT-3, AGENT-14

## Confirmation before a destructive run

Given a destructive, unprotected target
When an agent calls it without confirmation, then with confirmation
Then the first call returns the plan and runs nothing, and the second runs it

Covers: AGENT-4

## Off-loopback without credentials

Given an HTTP transport configured to bind on all interfaces with no token or authenticator
When the server starts
Then it refuses to start

Covers: AGENT-5

## Oversized unauthenticated body

Given an HTTP request without a valid token and with a body larger than the cap
When it arrives
Then it is rejected as unauthorised before the body is read

Covers: AGENT-6

## Throwing authenticator

Given an authenticator that throws
When an HTTP request arrives
Then the response is a bare unauthorised status and nothing runs

Covers: AGENT-7, INV-8

## Audit masks secrets

Given a call that passes a secret parameter
When the audit trail is read on the host
Then the secret value is masked and the operator token is absent; requesting the trail through the agent protocol is refused

Covers: AGENT-8, AGENT-9

## Unlisted remote module

Given a registry entry whose module is on a host that is not allow-listed
When its run tool is called
Then a structured launch-origin error is returned, the call is audited as denied, and nothing is spawned

Covers: AGENT-10

## Busy registry

Given the concurrent-spawn cap is reached
When another run call arrives with valid parameters
Then it returns a structured busy error at once

Covers: AGENT-11

## Metadata path

Given a resource identifier with a path
When protected-resource metadata is requested
Then the document is served at the well-known path inserted before the original path; an identifier carrying credentials is refused at startup

Covers: AGENT-12

## API index drift

Given a public function whose documentation changed and an index not regenerated
When the gate runs
Then it fails naming the stale index

Covers: AGENT-13
