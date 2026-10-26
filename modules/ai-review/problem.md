# Problem: AI review and self-healing

A model's review written as prose cannot gate a build: nobody can tell whether "consider handling this" means "block the merge". Without memory between runs, the same false positive comes back on every push, and each rewording of it gates again, so people learn to ignore the reviewer.

When a build fails, a human has to diagnose it, even though the failing command, its error output and the diff are already at hand. Handing that to a model is useful only if the model cannot hide the failure, edit what it should not, or be steered by text an attacker put in the change.
