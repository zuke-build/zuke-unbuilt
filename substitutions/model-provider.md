# Model provider

## What the original used

Anthropic Claude, OpenAI and Google Gemini models, each with its own structured-output dialect.

## What a replacement must provide

Schema-enforced structured output, a credential sent in a header, and usage figures for budgeting.

## Known alternatives

Any hosted or local model with structured output.

## How to tell it is right

AIREV-1 holds: a malformed assessment is rejected by schema, not parsed loosely.
