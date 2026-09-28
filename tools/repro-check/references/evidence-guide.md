# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

In eval mode, read the environment record inside `Candidate repro report`
and compare it with the issue context and `Repo facts`. In live mode, read
the environment section of the student's draft report and the issue's stated
versions, platforms, drivers, or configuration. Good evidence names details
that can change the result, such as OS, package/tool version, runtime,
driver, shell, build profile, or relevant configuration; a disclosed delta
from the issue's target is acceptable.

## Steps

In eval mode, read the commands and setup in `Candidate repro report`; in
live mode, read the same sections in the draft. Good steps state the starting
state, inputs or fixtures, configuration, and trigger in an order a stranger
can repeat. A command that depends on a private repository or omitted
environment detail is not followable.

## Behavior shown

Read output excerpts, logs, screenshots, test results, and controls in
`Candidate repro report`, then compare them with the issue context's exact
trigger and symptom. Good evidence shows the reported behavior or clearly
shows a real attempt that did not reproduce it; a similar error, changed
input, or version-only assertion does not establish the issue.

## Honesty

Compare the report's outcome and expected/actual statements with its
artifacts, and compare promises in `Candidate claim comment` with what a
claim-only draft can know. `Cannot reproduce` passes when the attempt and
relevant differences are shown; confident claims of a crash, root cause, or
fix without matching evidence fail.

## Comms

Read `Candidate claim comment` and the report against the issue thread and
the `Repo facts` block's bug-report template, contribution policy, and AI-use
policy. A claim should identify the issue, promise the next investigation,
and avoid claiming reproduction in advance. Required disclosure or template
items must appear; Path Review classmates' claim comments do not block the
student's own package.
