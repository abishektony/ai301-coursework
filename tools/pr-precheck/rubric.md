# Rubric: is this pull request ready to submit?

<!--
The four failure families are the harness's scoring categories.
silent drift: plan-matches-diff, description-matches-diff.
not tested: repro-rerun, checks-run.
unreviewable: diff-clean.
standards wall: template-complete, ai-disclosure.
A PR that fails none of them is a clear accept. A PR that honestly
discloses a shortfall can be ready; "less than everything" is not a fail
by itself.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-matches-diff | The changed files and hunks in the diff (live: `git diff main...HEAD`), read against the plan's in-scope list, its not-in-scope line, its named files, and any deviation note. | Pass if every changed file and every behavioral hunk is something the plan names or a recorded deviation covers, and every plan item the description presents as done is in the diff. A plan item the diff leaves out passes only when the plan, a deviation note, or the description itself says it was deferred. Fail if the diff adds behavior, a new flag or config option, a rename, a refactor, or an edit to a file the plan scopes out or never names, with no deviation note. Also fail if a plan item is missing from the diff and nothing discloses it. | required |
| description-matches-diff | Each claim in the PR description ("implements the plan exactly", "docs updated", "no functional changes outside X", "adds tests for Y"), read against the diff's actual contents. | Pass if every concrete claim the description makes about what changed is visible in the diff. Fail if any claim is contradicted by the diff or not found in it: it says more than the diff delivers, says "exactly as planned" over extra hunks, or says "no other changes" when other hunks exist. | required |
| repro-rerun | The test-evidence section (live: `test_evidence.md`), read against the plan's test plan and the repro steps it names. | Pass if the evidence shows the plan's own repro (or a regression test built from it) run on the changed path, with an observable result that differs before and after, such as pasted output, an exit code, a value, or a diff. The result must come from the path the fix changes, covering every failure mode the plan's test plan names. Fail if the evidence is only a claim ("works for me", "tested locally"), if it exercises only the control or unchanged path, if the plan's named repro is never re-run, or if the plan's test plan names a second mode the evidence skips with no disclosure. | required |
| checks-run | The test-evidence section, read against the repo's stated checks (the PR template's checklist and test command, the plan's test plan, the live `make test-unit`, `make test-integration`, `make lint`, `make typecheck`). | Pass if the output of the repo's own named checks is pasted or quoted with a visible outcome, or the evidence honestly says a check was not run or failed and why. A check that exits with "no tests ran" because none exist passes when pasted as is. Fail if the repo's checks are never run and nothing says so, or if the evidence says only "tests pass" or "cargo test passes" with no output and no named result. | required |
| diff-clean | Every hunk in the diff and the commit messages, read for debris. | Pass if the diff contains only the change: no debug prints or logging left in, no commented-out code, no dead or unused functions, no leftover TODO or "wip" notes, no formatting-only or import-reordering churn, no unrelated hunks folded in. Fail if any of these buries the change, even when the underlying fix is correct. A small, disclosed edit the plan or a deviation note names is not debris. | required |
| template-complete | The repo-facts block's stated PR template asks and contributing rules (live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`), read against the description, the commit list, and the diff. | Pass if every section or checklist item the repo states is filled with real content for this change (an issue reference such as `Closes #N`, the checklist, a stated changelog or docs entry present in the diff), and no box is ticked that the evidence contradicts. Where the repo states no template or asks, pass if the description itself says what changed and why. Fail if a required section is empty, boilerplate, or missing, a required entry is absent from the diff, or an explicit maintainer ask in the thread is ignored. | required |
| ai-disclosure | The repo-facts block's stated AI-use policy (live: `docs/CONTRIBUTING.md` and the template's reviewer-notes section), read against the description. Treat every package as AI-assisted. | Pass if the description discloses AI use in the author's own words where the stated policy requires it (the tool and the extent of help, per the policy's wording), or in live mode in Notes for Reviewers. Pass if the repo states no AI policy. Fail if the policy requires disclosure and the description has none, or the disclosure is a bare token with no extent. | required |
| honest-gaps | The description, read against what the diff and the evidence leave undone. | Pass if every shortfall or deferred item visible in the diff or the evidence is stated in the description or plan. Fail if a visible gap is hidden or presented as done. | preferred |

## Verdict rule

Accept only when every `required` check passes. Any required `fail`
produces `reject`. A required `unclear` also produces `reject`, because
a PR whose fidelity, evidence, cleanliness, or compliance cannot be
verified from the package is not ready to submit. `preferred` checks are
reported in the summary and never change the verdict.
