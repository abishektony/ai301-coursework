# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| cause-grounded | The plan's stated cause, read against the repro-evidence block's steps, observed output, and any control run (a variant that works, a flag-off run, a debug trace). | Pass if the stated cause explains what the repro evidence shows and is not ruled out by any control or output in that evidence. Fail if the cause blames a component the repro evidence's own control run or trace clears, contradicts an observed result, or ignores the repro evidence and rests on a guess. | required |
| scope-bounded | The plan's in-scope statement, its not-in-scope line, and the list of files or areas it will change, read against the issue's one reported behavior. | Pass if the plan is one bounded change that fixes the reported behavior and says what it will not touch. Fail if it adds drive-by refactors, migrations, upgrades, redesigns, or extra fronts the repro evidence does not require, even if the real fix is somewhere inside it. | required |
| executable | The plan's named files or areas, its approach, and its order of work, read against the repo-facts block. | Pass if a stranger who has read only the plan could open the named file or area and start making the described change without asking the author anything. Fail if the location or the change is left vague ("improve the handling", "refactor as needed") or the plan depends on a step it never names. | required |
| test-observable | The plan's test plan, read against the repro-evidence block's steps and expected/actual results. | Pass if the test plan re-runs the repro steps (or a named regression test built from them) and names an observable result that differs before and after the fix (an output, exit code, value, or visible state). Fail if it says only "verify it works", "add tests", or names no outcome someone could see. | required |
| honest-unknowns | The plan's risks and unknowns, and any claim of certainty in the plan or comment, read against the repro evidence. | Pass if the plan states what it has not verified or what could go wrong where the repro evidence leaves a real gap, and does not claim certainty the evidence does not show. Fail if it asserts an untested fix, a root cause, or a timeline as established fact. | preferred |
| thread-aware | The plan comment, read against the thread-highlights section and the repo-facts block's bug template asks, contribution policy, and AI-use policy. | Pass if the comment engages any explicit maintainer direction, prior-art PR, or convention present in the thread or repo facts (for example a maintainer's stated approach, an open competing PR, or a review-bandwidth note), and satisfies any AI-disclosure the policy requires. Fail if it ignores explicit maintainer direction in the thread, races or duplicates an open PR without acknowledging it, or omits a disclosure the stated policy requires. If the thread and policy are silent, pass as long as the comment states the diagnosis, scope, and test plan itself rather than pointing elsewhere. | required |

## Verdict rule

Accept only when every `required` check passes. Any required `fail` or
`unclear` produces `reject`: a plan whose grounding, scope, steps, test,
or comment I cannot verify from the package is not ready to build from.
`preferred` checks are reported but never change the verdict.
