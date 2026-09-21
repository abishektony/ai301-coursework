# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | The repo-facts block's last 5 default-branch commits and maintainer first-response sample. | Pass if at least one of the last 5 default-branch commits is human-authored within 180 days of the bundle capture date, or the response sample contains a maintainer response within 30 days. | required |
| repo-in-use | The repo-facts block's archived flag, last push to any branch, and latest release. | Pass if `archived: no` and either the last push to any branch or the latest release is within 365 days of the bundle capture date. | required |
| clear-scope | The issue body and comment thread, including labels and linked or mentioned PR history. | Pass if the issue names one observable defect or one bounded docs/UI task with concrete examples, a target area, or an acceptance target. A single defect may include related implementation suggestions or updates to a few directly relevant pages. Fail if it is a tracking/umbrella issue, a pure usage question, requires codebase-wide changes, lacks a workable specification, or has unresolved design debate shown by the thread or multiple abandoned PR attempts. | required |
| unclaimed | The repo-facts block's assignees and linked PRs, plus the comment thread. | Pass if there is no assignee, no open linked PR, and no claim comment from the last 90 days without a maintainer explicitly releasing or redirecting that claim. A closed/unmerged PR or older claim alone does not fail this check. | required |
| contribution-policy | The contribution policy line in the repo-facts block, including any quoted AI policy, contributor documentation, and templates. | Pass if no policy bans AI-assisted or AI-generated contributions. Disclosure, testing, human review, or other conditions are compatible with a pass; an explicit outright ban fails. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if every required check passes. Any required `fail` or `unclear`
produces `reject`; there are no preferred checks.
