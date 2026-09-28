# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment record, read against the issue context and repo-facts block. | Pass if the report identifies the relevant OS, tool/package version, and configuration or runtime detail needed to interpret the issue's target behavior; a stated version difference passes when the report explains it. | required |
| steps-followable | The repro report's commands and setup steps, including any starting state, fixtures, or configuration it relies on. | Pass if a stranger with the recorded environment can follow the steps from a stated starting state and reach the reported behavior or an honestly documented cannot-reproduce result. | required |
| behavior-faithful | The issue context's described trigger and expected/observed behavior, compared with the repro report's output excerpt, log, screenshot, or other artifact and any control run. | Pass if the artifact demonstrates the issue's specific behavior, or if an exact or near-exact attempt includes the relevant output, control or expected comparison, and a meaningful environment difference that plausibly explains a cannot-reproduce result. A neighboring error, altered trigger, or unsupported inference fails. | required |
| outcome-honest | The repro report's outcome statement and its supporting artifacts, plus the claim comment's promises. | Pass if the conclusion says only what the evidence supports: a reproduced issue names the observed result, while a cannot-reproduce result stays conditional and does not claim a fix or root cause without evidence. | required |
| communication-ready | The claim comment and repro report, read against the repo-facts block's bug-report template, contribution policy, and AI-use policy. | Pass if the claim names the issue and intended next work without pretending reproduction already happened, the report/comment follows required template rules, and any policy requiring AI disclosure is satisfied by naming the AI assistance in the comment or report. If the policy is silent, no disclosure is required. A classmate's claim does not block a pass under Path Review rules. | required |

## Verdict rule

For a full package, accept only when every required check passes; any
`fail` or `unclear` produces `reject`. For a claim-only draft, grade
`communication-ready` and any other applicable claim-side checks, mark
report-only checks `unclear` with evidence `not yet applicable:
claim-only draft`, and apply the verdict rule only to the applicable
checks. No preferred checks change the verdict.

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
