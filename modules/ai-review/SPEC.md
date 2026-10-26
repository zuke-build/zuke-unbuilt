# AI review and self-healing

Model-assessed review runs as a build validation: a typed verdict gates the build, findings are posted to the change request with stable identifiers, and maintainers can rebut them. A fixer can take a failure, diagnose it and optionally propose or apply a fix, which is verified by re-running the real command.

## Requirements

- **AIREV-1** (MUST) The provider enforces a schema on every assessment (a score from 0 to 10, a severity and a list of findings); a gate trips when the score is strictly above its limit or the severity reaches its floor, and a review that itself fails breaks the build by default.
- **AIREV-2** (MUST) Suppressions match a stable fingerprint of finding kind, normalised title and file, ignoring line numbers; suppressed findings stay listed as not gating, and an identifier that no longer matches anything fails safe.
- **AIREV-3** (MUST) Only comments whose authors the code host attributes as trusted maintainers reach the model, and trust is taken from host metadata, never from comment text.
- **AIREV-4** (MUST) Dismissing a finding needs two keys: a trusted rebuttal that references the finding's identifier, and an adjudicating model call that accepts it; the model alone cannot dismiss an uncontested finding.
- **AIREV-5** (MUST) The reviewer trusts its own state blocks and thread markers only when the host attributes them to the reviewer's identity; pasted copies are ignored and never edited over.
- **AIREV-6** (MUST) All untrusted text sent to a model (diffs, command output, conventions, comments, candidate titles) is fenced, forged fence markers are defanged, and the conventions and criteria files are read from the base revision, not the change.
- **AIREV-7** (MUST) Everything posted to the change request or the job summary passes through the run's secret masking.
- **AIREV-8** (MUST) A verification pass can remove only findings it refutes with cited evidence; an uncertain finding still gates, and a verifier error keeps the finding.
- **AIREV-9** (MUST) Merging reworded duplicates cannot launder severity: a decision recorded for one finding covers a new candidate only in the same file and at equal or lower severity.
- **AIREV-10** (MUST) The fixer never masks the original failure: its own errors are swallowed and the build failure stands, and re-running the real failing command is the only verification of a fix.
- **AIREV-11** (MUST) The fixer only diagnoses by default; applied edits are limited by a path allow-list that always excludes lockfiles, the version-control folder, CI workflows and key material, by an edit cap and by the run scope, and committing requires an explicit opt-in.
- **AIREV-12** (MUST) When the spending budget is exhausted the AI step is skipped, not failed; a cost cap applies only once a priced call has been recorded, and no price table ships with the tool.
- **AIREV-13** (SHOULD) Several reviewers on one target share one comment ordered by severity, every reviewer runs, and their failures are raised together at the end.
- **AIREV-14** (SHOULD) Findings can be anchored as line threads that the reviewer replies to, resolves and reopens, and a finding is never anchored to a guessed line.
- **AIREV-15** (SHOULD) Model responses are cached by provider, model, effort and prompt with a time-to-live, and a cache hit costs nothing against the budget.
- **AIREV-16** (SHOULD) Transient provider failures are retried with backoff and a per-attempt timeout.
- **AIREV-17** (SHOULD) In diagnose mode the fixer can post committable inline suggestions, and a missing provider key can skip the review with a visible notice.
