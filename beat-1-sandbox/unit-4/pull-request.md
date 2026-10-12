# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

[Pull Request](https://github.com/codepath/pathreview-ai301-fa26-s3/pull/173)

**Branch**

fix/59-faithfulness-token-overlap

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

19/20 (first and only full run; the last score matches the agreement line in `eval-run.txt`: "agreement: 19/20 scored items  (bar: 18/20: PASS)").

**Package analysis**

pkg-16 (kubernetes/minikube#23471). The gold label is accept ("honestly disclosed shortfall: image-load path fails loudly with before/after transcripts; the start-path behavior deliberately deferred with the maintainer discussion cited in plan and description"). My rubric said reject: it graded `checks-run` as fail and the other required checks as pass. The package's test evidence has the full repro transcript (corrupt tarball gives an error and exit 1, valid tarball control unchanged) but its only mention of the repo's own checks is the bare line "`go test ./pkg/minikube/machine/... ./cmd/...` passes", with no output. My `checks-run` pass condition says "Fail if ... the evidence says only 'tests pass' or 'cargo test passes' with no output and no named result", so the rubric held a correct, honestly scoped fix over one unpasted test claim. That is a strictness gap in my rubric, not a gold-label error: the repro, the diff, the deferral and the template were all fine.

**Check rationale**

"| plan-matches-diff | The changed files and hunks in the diff (live: `git diff main...HEAD`), read against the plan's in-scope list, its not-in-scope line, its named files, and any deviation note. | Pass if every changed file and every behavioral hunk is something the plan names or a recorded deviation covers, and every plan item the description presents as done is in the diff. A plan item the diff leaves out passes only when the plan, a deviation note, or the description itself says it was deferred. Fail if the diff adds behavior, a new flag or config option, a rename, a refactor, or an edit to a file the plan scopes out or never names, with no deviation note. Also fail if a plan item is missing from the diff and nothing discloses it. | required |"

It reads this way because the silent-drift packages fail in two directions: extra behavior the plan scoped out (pkg-03, pkg-06, pkg-09) and a promised piece missing (pkg-17). The check compares the diff to the plan's lists, not to the description. I added the "passes only when ... deferred" sentence so that disclosed shortfalls (pkg-13, pkg-16) are not held as drift; I rejected a simpler "every plan item must be in the diff" rule because it would fail the honest-deferral accepts. Claims in the description are graded separately in description-matches-diff so one check does not carry both jobs.

**Trade-offs**

I stopped at 19/20 because the bar is 18/20 and all five categories matched (clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3). The one miss is pkg-16 on `checks-run`. Loosening `checks-run` so that a bare "go test passes" line is accepted when the repro evidence is decisive would probably fix pkg-16, but it risks flipping the not-tested rejects (pkg-04 and pkg-10 both rest on "tests pass"-style claims), so I would re-run those two as canaries with `--only` before keeping the change. I accept that the tool is stricter than gold on honest, well-scoped PRs that skip pasting the suite output.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
