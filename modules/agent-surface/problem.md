# Problem: agent surface

Coding agents that drive a build guess shell commands and flags. A typo or a wrong parameter fails only when the command runs, and a new pipeline needs a redeploy or new instructions before an agent can use it.

Exposing builds to agents adds risks a plain command line never had: who is allowed to start a deploy, who started it, and whether a secret ends up in an agent's context. Without a deliberate, typed and authorised surface, agents either cannot use the build or can use too much of it.
