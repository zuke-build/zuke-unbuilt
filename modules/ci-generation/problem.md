# Problem: CI generation

Hand-written CI configuration cannot be run locally, drifts from the build it is meant to run, and must be rewritten for each CI provider. Schedules written in local time need external glue and manual bookkeeping twice a year when daylight-saving time changes. Third-party CI steps referenced by a movable tag are a supply-chain risk, and nobody notices when one slips in unpinned.

Without this capability the CI file is a second, untested copy of the build.
