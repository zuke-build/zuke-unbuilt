# Problem: incremental and remote cache

Without a cache every run redoes unchanged work. A fresh CI checkout, and every job in a fanned-out pipeline, rebuilds everything from nothing.

Teams patch this with hand-written CI cache steps whose keys and paths drift away from what each task really reads and writes. A naive remote restore is worse: anyone who can write to the shared store can plant files in the workspace, such as version-control hooks, configuration or lockfiles, and the next build runs them.
