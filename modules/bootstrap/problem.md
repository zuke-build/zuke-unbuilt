# Problem: bootstrap and global command

A fresh checkout or CI runner does not have the runtime the build needs. Without a launcher, every contributor and every CI job installs the right version by hand, and versions drift.

Without a global command, users must change to the project root and call the launcher there. A naive global command that searches parent folders for a project is a hazard of its own: a project file planted in a shared parent folder would run someone else's code with the user's full permissions.
