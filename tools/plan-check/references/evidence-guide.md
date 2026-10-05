# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

Where it lives: the plan's "Cause" statement (or the first paragraph of the plan). The behavior it must explain is in the repro-evidence block: the steps, observed output, expected/actual lines, and any control run, flag-off run, or debug trace. Live: the student's posted repro comment on the issue.

What good looks like: the cause accounts for what the repro shows and no control or trace in the repro evidence clears the component it blames. A cause that blames something the package's own control run rules out, or that never refers to the repro evidence, is not grounded.


## Scope

Where it lives: the plan's in-scope statement, its not-in-scope line, and the files or areas it lists.

What good looks like: one bounded change that fixes the one reported behavior, with an explicit line about what is left alone. A drive-by refactor, upgrade, migration, or redesign wrapped around the fix is scope creep even if the real fix is inside it.


## Executability

Where it lives: the plan's named files or areas, the "Change" or approach description, and any ordering of steps. Compare with the repo-facts block for whether the named location is plausible.

What good looks like: a stranger could open the named file or area and start the described edit without asking the author. Phrases like "improve handling" or "refactor as needed" with no location are not executable.


## Test plan

Where it lives: the plan's "Test" section, read against the repro-evidence block's steps and expected/actual results.

What good looks like: it re-runs the repro steps or a regression test built from them and names an observable result (output, exit code, value, visible state) that differs before and after. "Verify it works" or "add tests" with no named outcome proves nothing.


## Honesty

Where it lives: the plan's risks and unknowns, any "not verified" notes, any wording of certainty in the plan or comment, and, after the build, the ## Deviations section.

What good looks like: gaps the repro evidence leaves open are named as unknowns, and the plan does not present an untested fix, root cause, or timeline as fact. A mid-build deviation is recorded in the plan, not only in the diff.


## Comms

Where it lives: the candidate plan comment, read against the thread-highlights section (eval) or the live issue thread (live), and the repo-facts block's bug-report template asks, contribution policy, and AI-use policy.

What good looks like: the comment engages explicit maintainer direction, an open or prior PR, and stated conventions where they exist, includes any AI disclosure the policy requires, and states the diagnosis, scope, and test plan itself. Boilerplate that ignores the thread, or "same approach as above", is not thread-aware.

