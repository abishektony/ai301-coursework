# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read the issue context first (title, body, thread highlights). Note the one behavior the issue reports and any explicit maintainer direction in the thread (a stated approach, a request, an open PR).
2. Read the repo-facts block. Note the bug-template asks, the contribution policy, and any AI-use or disclosure rule.
3. Read the repro-evidence block before the plan. Note the steps, the observed output, the expected result, and every control run, flag-off run, or trace, because these decide what a cause may and may not be.
4. Read the candidate plan: cause, change, in-scope and not-in-scope lines, files, test.
5. Read the candidate plan comment last, against what you noted in steps 1 and 2.


## Evidence gathering

1. cause-grounded: copy the plan's stated cause in one line. List each fact in the repro evidence the cause must explain, and each control or trace that could rule a component out. Record whether the cause is consistent with all of them.
2. scope-bounded: copy the plan's in-scope and not-in-scope statements and list every file or area it says it will change. Record any item that is not needed to fix the one reported behavior.
3. executable: record the file or area the plan names and the concrete change it describes at that place. Record any step that is left vague or unnamed.
4. test-observable: copy the plan's test plan. Record whether it re-runs the repro steps or a regression test built from them, and the exact result it says will differ before and after.
5. honest-unknowns: copy the plan's risks and unknowns, and quote any sentence that asserts certainty (untested fix, root cause, timeline).
6. thread-aware: list each maintainer direction, prior-art PR, and policy item from the thread highlights and repo-facts block. For each, record whether the plan comment addresses it, ignores it, or contradicts it. Record whether a required AI disclosure is present.
7. In live mode, pull these from the draft files and the live issue thread per the evidence guide. In eval mode, use only the bundle text.


## Check execution

1. Run the checks in the order they appear in the rubric table, using only the facts recorded in Evidence gathering.
2. Grade each check `pass` or `fail` by its pass condition, not by tone, length, or formatting. Quote the one fact or sentence that decided it.
3. Grade `unclear` only when the evidence the check needs is genuinely absent from the package (for example, the plan has no test section at all). If the evidence is present but weak, grade `fail`.
4. Never let one check's grade change another's: grade each independently, even when a plan fails several.
5. Re-read the whole package only when a recorded fact is missing; otherwise grade from the recorded facts.
6. Where this procedure is silent, say so in the summary rather than inventing a step.


## Verdict assembly

1. Collect the grade of every `required` check. Ignore `preferred` checks for the verdict, but still report them.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail` or `unclear`, the verdict is `reject`.
4. In the JSON, list every check with its grade and a one-line evidence quote. For a reject, make the first failing check's evidence the quote that decided it.
5. Emit the JSON block last, with nothing after it.

