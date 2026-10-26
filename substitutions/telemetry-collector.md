# Telemetry collector

## What the original used

An OpenTelemetry collector receiving OTLP over HTTP as JSON.

## What a replacement must provide

Ingestion of spans and counters with caller-chosen trace and span identifiers.

## Known alternatives

Any tracing backend that accepts caller-set identifiers.

## How to tell it is right

The one-trace-across-a-suspension scenario passes.
