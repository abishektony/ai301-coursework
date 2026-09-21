# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/59

**Verdict output**

The issue is a bounded, reproducible tier-1 bug. The repository is active,
the issue is unassigned, and there is no linked pull request. The required
checks pass, so this issue is accepted.

```json
{
   "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/59",
   "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "The main branch has human-authored commits dated September 16, 2026, within days of the September 21, 2026 evaluation."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "The repository is public, not archived, and its main branch was pushed within the last week."},
      {"name": "clear-scope", "grade": "pass", "evidence": "The issue identifies one observable _is_supported() defect and gives a focused pytest reproduction with the failing assertion."},
      {"name": "unclaimed", "grade": "pass", "evidence": "The issue has no assignee and no linked pull request or development relationship."},
      {"name": "contribution-policy", "grade": "pass", "evidence": "The repository welcomes contributions and no outright AI-contribution ban was found."}
   ],
   "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

The final full run scored `19/20 scored items` and passed the `18/20` bar.
The committed `eval-run.txt` records: `agreement: 19/20 scored items
(bar: 18/20: PASS)`.

**Issue analysis**

For `issue-01`, my rubric decided `reject`, while the gold label was
`accept`. The disagreement came from `clear-scope`: the issue was a valid
bounded documentation task, but the grader read its requested changes as
less concrete than the rubric required. The other liveness, repository-use,
claim, and policy checks were not the cause of the rejection. This showed me
that a first-issue rubric must distinguish a short bounded task from a true
umbrella issue instead of requiring an overly polished specification.

**Check rationale**

The current `clear-scope` check says: “Pass if the issue names one
observable defect or one bounded docs/UI task with concrete examples, a
target area, or an acceptance target. A single defect may include related
implementation suggestions or updates to a few directly relevant pages.
Fail if it is a tracking/umbrella issue, a pure usage question, requires
codebase-wide changes, lacks a workable specification, or has unresolved
design debate shown by the thread or multiple abandoned PR attempts.”
I used this wording because it accepts a focused bug such as issue #59 while
still rejecting megaissues and issues whose design is not settled.

**Trade-offs**

The scope check may still reject a legitimate older issue when its body is
brief and the acceptance target is implicit. I accepted that trade-off
because requiring evidence of a settled, bounded change is safer for a first
contribution than accepting work that may hide a design project. The full
run also confirmed that the change did not disturb the claimed, dead-repo,
policy, or other scope categories: it still passed `19/20`.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**
1. The issue's fit to your interests and to the time available.
   
   This issue fits my interest in Python, pytest, and focused backend bugs.
   The reproduction is a single unit-test command, so it is realistic to
   investigate and complete within the available time.

2. What the verdict identified correctly, and what you weighed that the rubric could
   not.

   The verdict correctly identified the issue as a small, reproducible
   starter bug: it has a named function, a failing assertion, a target test
   file, and a tier-1 label. I also weighed how much time the repository setup
   and the semantic similarity fix might take, which the rubric cannot measure.

3. The anticipated difficulty in claiming it.
   
   Claiming should be straightforward because the issue has no assignee and
   no linked pull request. The main difficulty is understanding the existing
   faithfulness-scoring behavior well enough to improve it without breaking
   other tests.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
