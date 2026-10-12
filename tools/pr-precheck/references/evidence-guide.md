# Evidence guide: where evidence lives in a PR package

Four families, matching the harness categories: plan fidelity (silent-drift), test evidence (not-tested), diff quality (unreviewable), standards and comms (standards-wall). A package that fails none of them is a clear accept.

## Plan fidelity (harness category: silent-drift)

Where it lives, in an eval bundle: the `## Plan context` block holds the plan: its in-scope statement, its "Not in scope" line, its "Files" line, its deliverables (code, tests, docs) and any deviation note. The `### Diff` under `## Candidate PR` shows what changed; `### Commits` lists commit messages; `### Description` makes the fidelity claims. In live mode: the plan is `plan.md` (its Scope, Files I will touch, and `## Deviations` sections); the diff is `git diff main...HEAD` with `git diff main...HEAD --stat` for the file list; the claims are in `pr_draft.md`.

What good looks like: every changed file and behavior hunk appears in the plan's named files or a deviation note, every deliverable the plan promised and the description presents as done is in the diff, and every description claim ("exactly as planned", "docs updated", "no functional changes outside X") is visible in the hunks. Drift runs both ways: extra behavior, a new flag or option, a rename, or an edit to a scoped-out file with no note; or a promised piece (a docs update, a second fix) missing while the description says it is there. A disclosed deferral is not drift.

## Test evidence (harness category: not-tested)

Where it lives, in an eval bundle: `### Test evidence` under `## Candidate PR`, read against the test plan inside the `## Plan context` block and the repro evidence quoted there. In live mode: `test_evidence.md` (the pasted output of `make test-unit`, `make test-integration`, `make lint`, `make typecheck`, and the before/after re-run of the unit 2 repro steps), read against the Test plan section of `plan.md`.

What good looks like: the plan's named repro is re-run on the path the fix changes, and a before and after output differ observably (a printed value, exit code, error message, or diff). Every failure mode the test plan names is shown. The repo's own checks appear with their output and result; a check that failed, or "no tests ran" because none exist, is reported as is, which still counts as honest evidence. "Tested locally", "works on my machine", or "tests pass" with no output proves nothing, and neither does an output taken from the unchanged or control path.

## Diff quality (harness category: unreviewable)

Where it lives, in an eval bundle: the unified diff in `### Diff` and the `### Commits` list. In live mode: `git diff main...HEAD` and `git log main..HEAD --oneline`.

What good looks like: the fix is the only thing visible. The debris tells: debug prints or logging left in (`print`, `eprintln`, `console.log`), commented-out lines or first attempts, dead or unused functions (often marked `allow(dead_code)` or named `_unused`), TODO or "wip" notes, imports reordered or lines re-indented or re-printed with no behavior change, and hunks in files unrelated to the issue. Commit messages like "wip", "misc cleanups while debugging", or "fmt + cleanup" signal it. A small edit the plan names, such as removing an xfail marker, is not debris.

## Standards and comms (harness category: standards-wall)

Where it lives, in an eval bundle: the `## Repo facts` block lists the repo's PR template asks, its contribution policy, and its AI-use policy; the `## Thread highlights` section holds explicit maintainer direction. The `### Description`, `### Commits`, and `### Diff` show whether they are met. In live mode: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the repo, the issue thread from `gh issue view <n> --comments`, and the sections of `pr_draft.md`, including Notes for Reviewers, where the course's AI disclosure goes.

What good looks like: every stated section or checklist item has real content for this change (an issue reference such as `Closes #N`, a filled checklist, a changelog or docs entry that is present in the diff when the repo requires one), boxes are ticked only when true, and maintainer direction in the thread is engaged. Where the repo has an AI policy, the description states AI use in the author's own words with the tool and the extent. Boilerplate, an empty required section, a missing required entry, or no disclosure under a stated policy is a standards wall, even when the code is right. Whether the description's claims match the diff is plan fidelity, not this family.
