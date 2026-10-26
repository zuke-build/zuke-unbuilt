# Problem: release operations

Manual canary rollouts leak partial exposure when something fails: each failure path grows its own ad-hoc rollback, and some paths have none. Long bake periods hold CI jobs open for hours.

Build runs are invisible to the tracing stack an operations team already watches, and a run that suspends and resumes later shows up as unrelated fragments, if at all.
