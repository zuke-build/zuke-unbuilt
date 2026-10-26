# Problem: typed build model

Build scripts that name their tasks with strings break quietly. A rename or a typo surfaces only when the pipeline runs, often in CI, long after the edit. Ordering is written by hand, so a prerequisite shared by two tasks runs twice, or not at all when someone forgets it.

Secrets are passed around as plain strings and end up in logs, error messages and echoed commands. A mistyped flag such as a misspelled dry-run switch was once ignored, so the build really ran when the user meant to rehearse it.

Without this capability a team keeps a scripts file, a make-style rules file and CI YAML in sync by hand, and learns about every mistake at run time.
