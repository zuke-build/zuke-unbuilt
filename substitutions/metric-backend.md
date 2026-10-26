# Metric backend

## What the original used

Prometheus queries, cloud monitoring APIs and a plain HTTP probe.

## What a replacement must provide

A query that returns numeric samples, or an empty result, over an encrypted transport.

## Known alternatives

Any time-series store.

## How to tell it is right

The empty-result and credentialed-address scenarios pass.
