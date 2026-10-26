# Substitution points

Each point is a piece of the original environment a builder replaces with their own. `required` means the module cannot be built without choosing one; `optional` means the module works without it.

| Module | host-runtime | remote-cache-backend | state-backend | ci-provider | agent-protocol | identity-provider | agent-harness | build-registry-host | model-provider | code-host | traffic-router | metric-backend | telemetry-collector |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| GRAPH | required | - | - | - | - | - | - | - | - | - | - | - | - |
| WRAP | required | - | - | - | - | - | - | - | - | - | - | - | - |
| CACHE | - | optional | - | - | - | - | - | - | - | - | - | - | - |
| DURABLE | required | - | required | - | - | - | - | - | - | - | - | - | - |
| CIGEN | - | - | - | required | - | - | - | - | - | - | - | - | - |
| BOOT | required | - | - | - | - | - | - | - | - | - | - | - | - |
| AGENT | - | - | - | - | required | optional | optional | optional | - | - | - | - | - |
| AIREV | - | - | - | - | - | - | optional | - | required | required | - | - | - |
| DELIVER | - | - | required | - | - | - | - | - | - | - | required | required | optional |
