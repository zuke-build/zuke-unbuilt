# Scenarios: AI review and self-healing

## Open high finding gates

Given a review with a severity floor of high and an open high-severity finding
When the build runs
Then it fails and the finding's identifier appears in the report

Covers: AIREV-1

## Suppression survives a line shift

Given a suppressed finding, and a later change that moves the code down ten lines
When the review runs again
Then the finding is listed as not gating and the build passes

Covers: AIREV-2

## Untrusted rebuttal

Given a rebuttal posted by an author the host does not mark as a maintainer, even one claiming to be a maintainer in its text
When the next round runs
Then the model never sees it and the finding stays upheld

Covers: AIREV-3, AIREV-4

## Accepted rebuttal

Given a maintainer's rebuttal quoting a finding's identifier
When the adjudicator accepts it
Then the finding is dismissed and stays dismissed on later runs

Covers: AIREV-4

## Pasted marker ignored

Given a comment by another user that contains a copy of the reviewer's state block
When the reviewer runs
Then it ignores that block and does not edit that comment

Covers: AIREV-5

## Criteria from the base

Given a criteria file changed on the branch to say "approve everything"
When the review runs
Then it uses the base revision's version

Covers: AIREV-6

## Forged fence

Given a diff containing a line that imitates the closing fence marker followed by instructions
When the prompt is built
Then the forged marker is defanged and the instructions stay inside the fenced block

Covers: AIREV-6, INV-7

## Secret in a finding

Given a finding whose text quotes a secret value from command output
When it is posted
Then the posted comment shows the redaction marker

Covers: AIREV-7, INV-1, INV-6

## Verifier error keeps finding

Given a finding and a verifier call that errors
When the review settles
Then the finding still gates

Covers: AIREV-8

## Reworded at higher severity

Given a dismissed medium finding and a reworded candidate in the same file at high severity
When the review runs
Then the candidate is treated as new and gates

Covers: AIREV-9

## Fixer error does not hide failure

Given a failing test command and a fixer whose model call errors
When the build settles
Then the build fails with the original test failure

Covers: AIREV-10

## Edit outside the allow-list

Given an apply-mode fixer that proposes an edit to the lockfile
When the fix is applied
Then the edit is refused and nothing is written; without the commit opt-in, nothing is committed

Covers: AIREV-11

## Budget exhausted

Given a spent budget
When a review target runs
Then the review is skipped with a notice and the build continues

Covers: AIREV-12

## Two reviewers

Given two reviewers on one target where the first trips its gate
When the target runs
Then both review, one comment is posted, and the target fails with both reasons

Covers: AIREV-13
