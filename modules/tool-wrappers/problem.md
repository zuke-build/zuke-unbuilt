# Problem: typed tool wrappers and safe shell

Builds call external command-line tools. Assembling those calls as shell strings invites injection and quoting bugs: one early importer silently lost backslashes. Untyped flags drift from the real tool, and a missing binary surfaces as a raw "not found" from the operating system with no hint how to fix it.

Assuming tools are already installed makes CI and developer machines drift apart, and an unpinned download can be swapped under the build.

Without this capability every build re-learns quoting, process cleanup and tool resolution, and a reviewer cannot tell from the build file which flags actually reach the tool.
