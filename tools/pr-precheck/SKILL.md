---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: **is this
ready to submit?** A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence) read against the plan it
claims to implement and the issue that plan belongs to. You grade one
package per run. You never answer a different question (is the code
elegant, should the issue exist, will a maintainer merge it), and you
never answer from gut feel: you answer by executing `procedure.md`,
which applies `rubric.md` to evidence found where
`references/evidence-guide.md` says it lives.

## Inputs and modes

Decide the mode first. If the request names a bundle id (`pkg-07`,
`calib-02`) or carries a package bundle inline, it is eval mode. If it
points at the student's own branch and draft files, it is live mode.

**Live mode** grades the student's own submission before it goes out.
Read these inputs from the working copy (the top folder of the
student's fork clone, on their branch) unless noted:

- `plan.md`, including its `## Deviations` section. This is the plan the
  PR claims to implement; a deviation recorded there is part of the plan.
- The diff: everything the branch changes relative to the default
  branch, produced by `git diff main...HEAD` (three dots). Also run
  `git diff main...HEAD --stat` for the changed-file list and
  `git log main..HEAD --oneline` for the commit list. Only committed
  changes count; uncommitted edits are not part of the diff.
- `pr_draft.md`: the draft PR title (first line) and description.
- `test_evidence.md`: the captured output of the repo's checks and the
  before/after repro.
- The issue and its thread, fetched live with `gh` (for example
  `gh issue view <number> --comments --repo codepath/pathreview-ai301-fa26-s3`).
- The repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and
  `docs/CONTRIBUTING.md`, which state the repo's asks, including how AI
  use is disclosed.

`plan.md`, `pr_draft.md`, and `test_evidence.md` are never committed, so
they will not appear in the diff; that is correct. A student on the
house chain reads the house issue, the house repro pack, and the house
plan in their place; the same checks grade the same things. If a live
input is missing (no commits ahead of main, no `pr_draft.md`, no
`test_evidence.md`, no plan), name which one is missing and grade the
checks that depend on it `unclear`. Do not substitute other files.

**Eval mode** grades a package bundle: a markdown file (or its JSON
twin) holding the repo facts, the issue and thread highlights, the plan
context with its repro evidence, and the candidate PR (title,
description, commits, diff, test evidence). The bundle is the whole
world. Use only its text. Fetch nothing, run nothing, read no other
file, and do not look up the real issue named on its `source:` line.
Eval mode always grades a complete package: every check, full verdict
rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else, before the plan and
before the diff. It names the one repo a pull request may target and the
house rules of that environment; apply those rules when you read the
evidence (for example, the PR comes from a branch on the student's fork,
and the template is always used).

- If the `Repo:` line still holds a placeholder (anything in angle
  brackets, such as `<ORG>/<PATH-REVIEW-REPO>`), stop without grading.
  Print one message telling the student to fill the `Repo:` line in
  `scope.md` with their section's Path Review repo. Emit no verdict and
  no JSON block, and never guess a scope.
- If the student's issue or PR target is not in the scoped repo, refuse
  to grade it and say why.
- In eval mode, ignore `scope.md` entirely; do not open it.

## The voice seam (live mode only)

In live mode, after the scope check, read `voice-guide.md`: the
student's own rules for how they write upstream. Hold the outgoing PR
text, the title and the description in `pr_draft.md`, against those
rules. In the readable summary, list each rule the draft breaks, naming
the rule and quoting the offending words; if none is broken, say so in
one line. The voice guide never changes the verdict on its own, because
only a rubric check can do that and none of the checks reads voice.
Voice findings are feedback for the student, not grades. In eval mode,
ignore `voice-guide.md` entirely; do not open it.

## Component reads

Use each component for its own job:

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. It decides what counts as pass, fail,
  and unclear, and how the grades combine.
- `references/evidence-guide.md` maps where each of the four evidence
  families lives, in an eval bundle and in a live working copy, and
  what good looks like there. Use it to find evidence, not to grade.
- `procedure.md` is how you work: the read order, how each check's
  evidence is gathered, how a check runs, and how grades become the
  verdict. Execute it as written, in order.

Where `procedure.md` is silent on a step you need, say so in the summary
on a line starting "Procedure gap:" and continue with the most literal
reading; never silently invent a step, and never add a check the rubric
does not have. If `rubric.md` has no checks filled in, or `procedure.md`
has no steps filled in (comments and the empty template row do not
count), stop and say that this tool cannot grade without a rubric and a
procedure. Emit no verdict and no JSON block.

## Verdict and output

The verdict space is binary: `accept` means the package is ready to
submit; `reject` means hold. There is no third verdict and no score;
reservations belong in a check's evidence line, never in the verdict.
Derive the verdict only by applying the rubric's verdict rule to the
check grades.

Your reply is a short readable summary (one line per check with its
grade and the deciding fact, plus voice-guide findings and any
procedure gaps in live mode) followed by one fenced JSON block, and
nothing after the block. The block must be valid JSON, must list every
rubric check by its exact name, and must be the last fenced JSON block
in the reply. Use the bundle id as `item` in eval mode, and the PR URL
(or the issue URL if the PR is not open yet) in live mode.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be ready
  and a beautiful confident one can be hiding drift. Read the diff, the
  pasted output, and the description's claims themselves; never grade
  on formatting, length, tone, or how sure the author sounds.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes; note the tension in the
  summary and leave the grade alone.
- **The procedure decides how, not you.** Follow `procedure.md` and
  report its gaps rather than papering over them.
- **Unclear follows the rubric's verdict rule.** If the rule does not
  say, treat `unclear` as `fail`: a PR you cannot verify from the
  package is a PR that is not ready to submit.
- **A description's claims are claims.** Verify each against the diff
  or the pasted output before crediting it. Never take "tests pass" or
  "implements the plan exactly" on trust.
