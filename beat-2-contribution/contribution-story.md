# Contribution: Verify Python==3.13 when running `make build`

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Abishek Tony
**GitHub Username:** abishektony
**Issue:** https://github.com/ASFHyP3/hyp3/issues/2551
**Pull Request:** https://github.com/ASFHyP3/hyp3/pull/3211

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout with a Python-focused search strategy**

I turned my unit 1 issue-select skill into issue-scout, with three `gh search issues` queries taken from my fit profile and ranking filters (no assignee, recently updated, few comments, one per repo). The first scan graded five issues: two clear accepts, two rejects on clear-scope (an empty body and an umbrella issue), and one Swift issue that matched the Python filter only because of the repo's main language. All five came from repos a few days old, because "fewest comments, newest first" rewards brand-new repos, so I added a rule that drops issues created in the last 7 days. I haven't re-scanned to confirm it helps.

**Unit 6: Installed the voice guard and wrote a repo memory file for hyp3**

I merged my three voice guide copies into one canonical guide, added upstream rules for claims, questions and pings, and hooked the checker into Claude Code; the wiring test was blocked as it should be, and the guard passed my claim before it posted. Two things broke or fell short: the shipped hook calls `python3`, which on my Windows Git Bash is the Microsoft Store stub, so I changed it to `python`; and `ln -s` in Git Bash made plain copies instead of links, so I recreated them as real symlinks from PowerShell. The guard checks wording, not context, so it did not notice that my earlier claim comment ignored another contributor's claim; repro-check caught that and I posted a corrected follow-up. For the memory file I wrote `CLAUDE.local.md` in my hyp3 clone (conventions, the one Makefile target, test and ruff commands), kept it out of git through `.git/info/exclude`, and skipped the paid orientation run because the whole subsystem is one Makefile target.

**Unit 7: Installed the ship-gate and wrote a manifest for hyp3**

The ship-gate passed my branch (`fix/2551-verify-python-3-13`) on the first by-hand run: my one manifest check, ruff, passed in 3 seconds, and pr-precheck passed all eight checks against the plan, the diff, `test_evidence.md` and the PR description. What fought me was Windows. The hook shipped with `python3`, which is the Microsoft Store stub in Git Bash, so I changed it to `python`, and the gate runs commands through `cmd.exe`, so `make`-based checks and the repo's colon-separated `PYTHONPATH` could not run; `make tests` and mypy already fail on `develop` on my machine, so they went into the manifest's excluded table with the dated baseline errors and are also named as unrun in the PR description. The gate does not grade whether the claim on the issue is settled: pr-precheck noted that rnossair's earlier claim had no reply, so I held the PR until rnossair released the issue on 2026-11-05, and then I re-ran the gate before opening it.

**Unit 8: Installed the PR monitor and wrote a playbook for hyp3**

The monitor read PR #3211 (no checks, no reviews, under a day old) and drafted a wait note, because my own 10-day first-review threshold was nowhere near met; I did not post anything, since a wait note is for me. What fell short: the one draft it made was the least testable one, so I have not yet seen how it handles a reviewer's comments or a stalled thread, and the draft file showed garbled characters (`â€”`) where a dash should be on my Windows terminal. The playbook was the real work. The repo's CONTRIBUTING.md states no wait, and the five merged PRs I checked had first reviews after 1 hour, 3 days, 7 days, 5 minutes and 29 days, so I set the thresholds at 10 quiet days for a first review and 7 after I push changes, based on the slow cases because most merged PRs there come from the core team.

---

## Why I Chose This Issue

I picked this issue from the Issue Selection site, filtered to Python from my fit profile (comfortable with Python, pytest and small backend or command-line changes, wanting practice reading an unfamiliar codebase and reproducing bugs). My own scan with issue-scout graded five issues from fresh small repos: two accepts, two rejects on scope and one Swift issue. Those repos were only days old, so I did not choose any of them. The scout rejected hyp3#2551 (ASFHyP3/hyp3, "Verify Python==3.13 when running `make build`") on one check only, unclaimed. rnossair commented "I would like to claim this issue if possible" on 2026-09-16, with no maintainer reply, no assignee and no linked PR. The other four checks passed, including the patched scope check (one assertion in the `make build` target at `Makefile#L17`).

I chose it against the scout's reject because the fix is small, in a Python repo with an active maintainer team, and the claim looked stale after about seven weeks of silence, but I knew I might be wrong about that. So my claim says I saw rnossair's request, asks a maintainer whether it is still active, and says I will not open a PR until I hear back. I also posted an earlier, shorter claim that did not mention rnossair, and my follow-up corrects that. About four hours after my follow-up, rnossair replied on the issue: "Hey there, I'm not claiming this issue anymore. You should be free to work on it." (https://github.com/ASFHyP3/hyp3/issues/2551#issuecomment-6651123693). That settled the claim, so the one check the scout rejected on is now resolved; no maintainer has replied yet. I hope to learn how a real Makefile-driven build and its tests work, and how to handle a shared claim with maintainers. On the tool: the unclaimed check did what I wrote it to do, but it only reads comment dates, so it cannot tell a stale claim from an active one, and my first scan crowded out better repos by ranking brand-new ones first, which I patched with a 7-day age filter.

Claim comment: https://github.com/ASFHyP3/hyp3/issues/2551#issuecomment-6647938681 (a follow-up to my earlier short comment at https://github.com/ASFHyP3/hyp3/issues/2551#issuecomment-6647174287).

**Toolkit**

- Tool: issue-scout (my search strategy with three `gh search issues` queries, my five-check rubric), run as a scan and then as grading of my picks.
- What it produced: a scan of five issues (two accepts, three flagged), then grades of my picks. hyp3#2551 was rejected only on unclaimed, with the patched scope-feasible check passing; Xplore-workshop#27 was rejected on scope; Nimbus#1262 was accepted with a moderate difficulty note but was a docs task.
- Difficulties: the scan favored brand-new repos; the language filter matched the repo and not the issue (one Swift issue came back for a Python search); the nested `claude` runs could not use `gh` until I allowed read-only commands; and the unclaimed check cannot judge whether a claim is stale.
- What changed: added a ranking rule dropping issues created in the last 7 days; replaced my strict clear-scope check with the staff scope-feasible row and added the difficulty note (scope patch, 2026-11-04); logged both in scan-notes.md. I would still add a note to the unclaimed check about asking the claimant or a maintainer before taking a claimed issue. In this case the flag was right to be raised and wrong to be final: the claimant released the issue once asked, so asking was the fix, not walking away.

---

## Understanding the Issue

**Problem Description**

HyP3 deploys its AWS Lambda functions on a Python 3.13 runtime, and `make build` installs each function's dependencies with whatever `python` is first on the PATH. Nothing checks that this is 3.13, so someone building with another version gets dependencies built for the wrong version, and the problem only shows up as import or runtime errors after deployment.

**Expected Behavior**

`make build` should stop right away with a clear message when `python --version` does not start with `3.13`, before it installs anything.

**Current Behavior**

`make build` runs on any Python version without a warning. On Python 3.11.7 the dry run `gmake -n build` lists nine pip install commands and exits 0 (see Reproduction Process).

**Affected Components**

The root `Makefile` (the `build: render` target, now on line 18) and `CHANGELOG.md`, which this repo's CI requires a PR to update. No Python code or tests are involved.

**Toolkit**

- Tool: repro-check and my CLAUDE.local.md notes, used to read the issue against the current repo.
- What it produced: the finding that the Makefile has no version check, plus two stale details in the issue (the line number, and a README line that no longer exists).
- Difficulties: none met for this section.
- What changed: nothing; the stale details went into my plan and PR description instead.

---

## Reproduction Process

**Environment Setup**

Windows 11 Home (10.0.26220), commands run in Git Bash. Python 3.11.7 (Anaconda), deliberately not 3.13, which is the version the issue says the build needs. GNU Make 4.4.1, run as `gmake` from Strawberry Perl because there is no `make` on my PATH. The repo is ASFHyP3/hyp3 on `develop` at commit `ffbc86cc4794daa07cbaae6b77a64514e215d4b7` (2026-11-04), cloned with `git clone --depth 50 https://github.com/ASFHyP3/hyp3.git` (upstream clone, not a fork yet). Challenges: `make` was not installed, so I used the Strawberry `gmake`; `python3` in Git Bash is the Microsoft Store stub, so I used `python`. I did not run the full build.

**Steps to Reproduce**

1. `git clone --depth 50 https://github.com/ASFHyP3/hyp3.git && cd hyp3`
2. `python --version` prints `Python 3.11.7`.
3. `grep -n -i "version\|3\.13" Makefile` prints nothing, so the Makefile has no version check.
4. `export PATH="$PATH:/c/Strawberry/c/bin"; gmake -n build; echo $?` prints the `render` step and nine `python -m pip install --upgrade -r requirements-apps-*.txt -t apps/<app>/src` commands, exits 0, and gives no warning about the Python version.

**Reproduction Evidence**

- **Commit showing reproduction:** none (no change made; I only ran commands on upstream `develop` at `ffbc86c`).
- **Screenshots/logs:** step 3 output was empty; step 4 listed the nine pip install commands and exited 0. A dry run of one pip step, `python -m pip install --dry-run -r requirements-apps-scale-cluster.txt -t "$TEMP/hyp3-repro-target"`, also exited 0 on 3.11.7.
- **My findings:** nothing in `make build` stops a build on the wrong Python version, so the issue still holds on current `develop`. The build target is now on `Makefile` line 18, not line 17 as in the issue. The README text quoted in the issue ("requires pip for python 3.13") is gone and `git grep "make build"` finds nothing, so that part of the issue is out of date. I did not reproduce the import or runtime errors on an AWS Python 3.13 runtime that the issue describes, only that the build accepts a non-3.13 Python without a check.

**Toolkit**

- Tool: repro-check, run on `claim-draft.md` and `repro-draft.md` together, with the unit 6 wild scope and my canonical voice guide.
- What it produced: on the first claim-only run, a fail on communication-ready because my earlier comment on the thread ignored rnossair's claim; after I rewrote the claim as a follow-up, accept on all five checks for the claim and repro together.
- Difficulties: it cannot run the commands, so it graded my pasted output as given; and it flagged a problem with a comment I had already posted, which no draft edit could undo.
- What changed: rewrote the claim as a follow-up that names rnossair's claim, and added to the repro report what I did not show (no full build, stale README, line number moved).

---

## Solution Approach

**Analysis**

`make build` in the root `Makefile` (`build: render`, line 18) runs nine `python -m pip install --upgrade -r requirements-apps-*.txt -t apps/<app>/src` commands with whichever `python` is first on the PATH. The Makefile never looks at the Python version: `grep -n -i "version\|3\.13" Makefile` finds nothing. The Lambda functions run on a Python 3.13 runtime and `pyproject.toml` says `requires-python = "==3.13.*"`, so installing with another version puts dependencies built for the wrong version into `apps/*/src`, and the failure shows up only after deployment. I confirmed on `develop` at `ffbc86c` with Python 3.11.7 that `gmake -n build` lists the install commands and exits 0 with no warning.

**Proposed Solution**

Add a small `check-python` target to the Makefile and make it the first prerequisite of `build`:

```
build: check-python render
```

`check-python` runs `python --version`, and stops with a non-zero exit and one message (for example `make build requires Python 3.13.x but found: Python 3.11.7`) unless the output starts with `Python 3.13.`. It uses only a POSIX `case` statement, so it needs no extra tools. It runs before `render`, so a wrong version fails before any work is done. I will also add a `CHANGELOG.md` entry, because this repo's CI has a changelog check.

I am deliberately not changing: the other Makefile targets, `render`, the requirements files, the README, the CI workflows, or any Python code. I am not adding a repo pytest, because no existing test covers the Makefile and the `tests/` conftest needs the full rendered environment, which does not render on my Windows machine. I will verify the target by running it directly instead.

**Implementation Plan**

1. In `Makefile`, add the `check-python` target and change `build: render` to `build: check-python render`.
2. Run `gmake check-python` with Python 3.11 (expect exit 1 and the message), 3.12 (expect the same) and 3.13 (expect exit 0), and run `gmake -n build` to confirm the check comes first.
3. Add a `CHANGELOG.md` entry for the next version, `10.18.2`, since `10.18.1` is already released.
4. Run `ruff check . && ruff format --diff .` and save the output with the before and after runs in `test_evidence.md`.

**Toolkit**

- Tool: plan-check, run on `plan.md` and `plan-comment.md` against the issue and my repro evidence.
- What it produced: accept on all six checks (cause-grounded, scope-bounded, executable, test-observable, thread-aware, honest-unknowns), with a reminder that the plan comment stays off the issue thread.
- Difficulties: none met; the repo's `make tests` could not run on my machine, so my plan says up front that I verify by running the target directly.
- What changed: nothing in the plan; I kept the test plan to observable exit codes.

---

## Testing Strategy

I re-ran my reproduction on the branch. Before the fix, `gmake -n build` on Python 3.11.7 listed the render step and nine pip installs and exited 0. After the fix, `gmake check-python` prints `make build requires Python 3.13.x but found: Python 3.11.7` and exits 2 on 3.11.7 and again on 3.12.2, and exits 0 on 3.13.3. A real `gmake build` on 3.11.7 now stops at `check-python` before `render` or any pip install, and on 3.13.3 the dry run shows the check first and the original steps unchanged. The repo's own check, `ruff check . && ruff format --diff .`, passes. I could not run `make tests` or `mypy .`: both already fail on `develop` on my Windows machine (the `render` step fails with `TemplateNotFound: apps\compute-cf.yml.j2`, and mypy reports an unused `type: ignore` in `validation.py`), and my change touches only the Makefile and CHANGELOG.md. All the output is in `test_evidence.md` in my clone.

**Toolkit**

- Tool: ship-gate manifest (`ship-manifest.md`), the manifest's ruff command run in a fresh `cmd.exe` shell, plus the commands in `test_evidence.md`.
- What it produced: one must-pass check (ruff, passing) and three dated exclusions with reasons (`make tests`, mypy, openapi-validate and cfn-lint).
- Difficulties: the ship-gate runs commands through `cmd.exe` on Windows, so `make`-based checks and the project's colon-separated `PYTHONPATH` don't work; two of the project's checks already fail on `develop` here; and my test plan can only run the new target, not the CI suite.
- What changed: I wrote the ruff command with `.venv\Scripts\ruff`, moved the failing checks to the excluded table with the baseline errors, and said in the PR draft and evidence which checks I could not run.

---

## Implementation Notes

**2026-11-04**

Set up a Python 3.13 virtualenv in my hyp3 clone with `uv` and installed `requirements-all.txt`. Ran the baseline on `develop`: ruff passes, `make tests` fails on my Windows machine (the `render` step cannot find `apps\compute-cf.yml.j2`, and the colon-separated `PYTHONPATH` in the Makefile does not work with Windows paths), mypy reports one unused `type: ignore`. A wrong turn: I tried to get a Linux test run through WSL, but its Python has no `ensurepip`, so I dropped it and removed the empty venv. Decided not to add a pytest because no test covers the Makefile and the `tests/` conftest needs the rendered environment.

**2026-11-04**

Built the fix on branch `fix/2551-verify-python-3-13`: a `check-python` target in the Makefile that uses a POSIX `case` on `python --version`, made the first prerequisite of `build` so it fails before `render`. Chose the `case` statement over `grep` so it needs no extra tool, and a prerequisite over an in-recipe line so the error comes before any work. Ran it under Python 3.11.7, 3.12.2 and 3.13.3 (see Testing Strategy). Added a `[10.18.2]` entry to CHANGELOG.md because 10.18.1 is already released and the repo's CI checks that the changelog is updated. Two local commits at this point, nothing pushed, and I held off on a PR because rnossair's earlier claim on the issue had no reply.

**2026-11-05**

rnossair replied on the issue that they are no longer claiming it and that I am free to work on it, so I removed my own hold on opening a PR. I added a line about that to the PR description draft, re-ran the voice guard and the ship-gate, and set my fork (`abishektony/hyp3`) as `origin` and `ASFHyP3/hyp3` as `upstream` in my clone. Nothing about the fix changed. No maintainer has answered my 3.13 vs 3.13.x question, so the PR description keeps the any-3.13.x choice stated as an assumption.

**2026-11-05 (later)**

Opened PR #3211 against `ASFHyP3/hyp3` `develop`. Checked it through the REST API: 2 files, +12/-1, body matching my draft, no checks, comments or reviews yet. Installed the PR monitor, wrote my playbook, and ran it once; it drafted a wait note and I posted nothing. I will check the PR again on or after 2026-11-15, my first-review nudge date.

**Toolkit**

- Tool: Claude Code, working in my hyp3 clone with `CLAUDE.local.md` for the repo's commands and conventions, and `uv` for the Python 3.13 environment.
- What it produced: the Makefile change, the CHANGELOG entry, the before and after runs in `test_evidence.md`, and the dated notes above.
- Difficulties: a safety check blocked one multi-line command that had no deletion in it (a false alarm, solved by using the edit tool and simpler commands); the project's test setup does not run on Windows; the WSL attempt failed.
- What changed: I recorded the Windows limits as exclusions instead of hiding them, and checked the fix by running the new target directly under three Python versions.

---

## Pull Request

**PR Link:** https://github.com/ASFHyP3/hyp3/pull/3211

**PR Description:** Title: "Stop make build unless Python is 3.13.x", opened against `ASFHyP3/hyp3` `develop` from my fork branch `fix/2551-verify-python-3-13` (2 commits, 2 files, +12/-1). The description says `make build` installed the Lambda dependencies with whatever `python` was first on the PATH and never checked for 3.13, and that the PR adds a `check-python` target as the first prerequisite of `build`, which stops with a message unless `python --version` starts with `Python 3.13.`, plus a `[10.18.2]` CHANGELOG entry. It says what I did not change, notes that the issue's line number and README quote are out of date, includes `Fixes #2551`, and shows the before and after output: exit 0 on Python 3.11.7 before, and exit 2 on 3.11.7 and 3.12.2 and exit 0 on 3.13.3 after. It says ruff passes, that I could not run `make tests` or mypy because both already fail on `develop` on my Windows machine, that I tested only on Windows with `gmake`, and that Claude Code helped me read the files and draft the recipe and changelog wording while I ran and reviewed everything. It also says rnossair, who had asked to work on the issue first, replied on the issue that they were no longer claiming it.

**Maintainer Feedback:**

- 2026-11-05: none yet. No maintainer comments or reviews on the PR, and no CI checks reported on it yet.

**Status:** Awaiting review

**Toolkit**

- Tool: ship-gate (the hook, my manifest, and pr-precheck), run by hand on the branch before the PR, and again after the claim was released.
- What it produced: PASS both times. My one manifest check (ruff) passed in 3 to 5 seconds, and pr-precheck passed all eight checks (plan-matches-diff, description-matches-diff, repro-rerun, checks-run, diff-clean, template-complete, ai-disclosure, honest-gaps).
- Difficulties: the gate only fingerprints what changed, so it let a description pass that had no mention of the earlier claim until I added it myself; and the gate pass lasts 30 minutes, so a slow decision means another run.
- What changed: I added the claim line to the PR description, re-ran the gate, and recorded the checks I could not run in the description so reviewers know what to rely on CI for.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how a Makefile prerequisite stops a build early: `build: check-python render` runs the check before `render` or any pip install, and a POSIX `case` on `python --version` needs no extra tool. I learned that GNU Make recipes run through `sh`, so `$$(...)` escaping matters and the repo's colon-separated `PYTHONPATH` fails on Windows. I also learned to use `gh api` (REST) when the token lacks `read:org`, and to read a repo's changelog convention: 10.18.1 was already released, so my entry needed a new `[10.18.2]` heading.

**Challenges Overcome**

The hardest part was the claim. Another contributor, rnossair, had asked to work on the issue in September, and my first comment ignored that; repro-check caught it, I posted a corrected follow-up, and I held the PR until rnossair released the issue on 2026-11-05. The second was Windows: the hooks shipped with `python3`, which is the Microsoft Store stub, so I changed them to `python`; `make tests` and mypy already fail on `develop` on my machine, so I recorded them as excluded with dated reasons and said so in the PR description instead of claiming they passed.

**What I'd Do Differently Next Time**

I would read the issue's full comment history before drafting any comment, because that would have avoided the first claim I had to correct. I would also test on Linux from the start, for example in a container, so the PR could say the full test suite passed instead of leaving it to CI. And I would ask the maintainers about the open choice (exactly 3.13 or any 3.13.x) earlier, since nobody has answered it and the PR rests on my assumption.

---

## Resources Used

- Issue #2551 and its comments (https://github.com/ASFHyP3/hyp3/issues/2551): the problem statement, the earlier claim, and rnossair's release of it.
- The hyp3 `Makefile`, `pyproject.toml` (`requires-python = "==3.13.*"`) and `CHANGELOG.md`: where the check belongs and how the changelog entry has to look.
- hyp3 CONTRIBUTING.md and its recent merged PRs (3154, 3144, 3179, 3175, 3177): the repo's workflow, and the review timings behind my playbook thresholds.
- rnossair, who replied on the issue and let me go ahead.
