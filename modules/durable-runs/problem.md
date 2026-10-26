# Problem: durable runs

A run that lives only in memory is gone when its process dies. Nothing records what ran, and a pipeline that spans days (deploy, wait for an approval, promote) cannot pause in one process and continue in another.

Two concurrent deploys of the same resource clash. A killed process leaves records stuck at "running" for ever. Without managed services, every team writes "start the server in the background, sleep, run tests, maybe kill it", and the server leaks whenever tests fail.
