# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

abishektony

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/59#issuecomment-6001185860

Plan for #59, based on my reproduction above (Windows 11, Python 3.11.7).

**Diagnosis.** In `rag/evaluator/faithfulness_checker.py`, `_is_supported()` needs at least 2 shared non-stop tokens. In my repro, `_is_supported("Knows Python", "python expert")` returned `False` while `_is_supported("Knows Python", "The candidate knows Python")` returned `True`. Separately, `test_multiple_context_chunks` fails because the claim's tokens keep commas (`"python,"`), so only `"docker"` overlaps; removing the commas by hand flipped it to supported.

**Scope.** I plan to change only `_is_supported()`: strip punctuation from tokens, and replace the fixed `>= 2` count with a rule that also accepts a short claim when most of its meaningful tokens appear in the context. I will also remove the `issue #59` xfail markers that then pass and add regression tests for the two repro calls. I will not touch `test_none_context_chunk_text` (#60), `check()`, `_extract_claims()`, or add any semantic matching.

**Test plan.** Re-run `pytest tests/unit/test_faithfulness_checker.py -v -rx` (before: `18 passed, 4 xfailed`) and with `--runxfail` (before: three #59 failures), and the two direct `_is_supported` calls above. I expect the first call to return `True`, the #59 tests to pass, and only the #60 `TypeError` to remain.

**Unknowns.** `test_partial_support_returns_middle_score` uses a single sentence but asserts `0.2 < score < 0.8`, which one claim cannot produce, so I am not sure it can pass from this change alone; if not I will say so here rather than widen scope. The token-overlap threshold is untuned, and I have not tested tokens like `c++`.

**Overlap with others.** I saw Znasif's plan above and PR #90 (mohtashim-syed), which also rewrite `_is_supported()`. Mine is built from my own repro and is narrower: it does not touch `check()`, `_extract_claims()`, or add a filler-word list, so it will conflict with #90 if both land. Per the Path Review house rules I am posting my own plan, and I will build only on my fork.

I have not started the code change yet; I will post the result of the re-run here.

---

## Your branch

**Branch**

fix/59-faithfulness-token-overlap

**Evidence**

Unit 2 repro steps re-run before and after the fix (commands: `pytest tests/unit/test_faithfulness_checker.py -v -rx`, the same with `--runxfail`, and the two direct `_is_supported` calls). Full output is below.

```
## BEFORE (main, unmodified)
=========================== short test summary info ===========================
XFAIL tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_partial_support_returns_middle_score - issue #59: faithfulness checker can never mark short claims as supported
XFAIL tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks - issue #59: faithfulness checker can never mark short claims as supported
XFAIL tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_claims_varying_support - issue #59: faithfulness checker can never mark short claims as supported
XFAIL tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text - issue #60: faithfulness checker crashes when a context chunk has text: None
18 passed, 4 xfailed in 0.55s
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_partial_support_returns_middle_score
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_context_chunks
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_multiple_claims_varying_support
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
4 failed, 18 passed in 0.66s
_is_supported(Knows Python, python expert) -> False
_is_supported(Knows Python, The candidate knows Python) -> True

## AFTER (fix/59-faithfulness-token-overlap)
..x...............x.....                                                 [100%]
=========================== short test summary info ===========================
XFAIL tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_partial_support_returns_middle_score - issue #59: faithfulness checker can never mark short claims as supported
XFAIL tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text - issue #60: faithfulness checker crashes when a context chunk has text: None
22 passed, 2 xfailed in 0.54s
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_partial_support_returns_middle_score
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
2 failed, 22 passed in 0.55s
_is_supported(Knows Python, python expert) -> True
_is_supported(Knows Python, The candidate knows Python) -> True
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Full run 1: 18/20 (PASS, bar 18/20), with the category floor held (clear-accept 6/7, scope-creep 4/4, thread-convention 1/2, unbuildable 3/3, wrong-cause 4/4). Then a partial `--only pkg-14,pkg-20` re-grade (no bar verdict): 1/2. Two earlier launches crashed before grading (wrong skill path, then a Windows cp1252 encoding error) and produced no score. The last full-run score, 18/20, matches the agreement line in `eval-run.txt`.

**Package analysis**

`pkg-14` (zellij-org/zellij#5174, category clear-accept). Gold said `accept`; my rubric said `reject`, failing `executable`. The gold note reads: "honestly scoped-down: reattach handshake fix with a regression-window repro; defers the untestable Windows variant and says so; arguable on the deferral, ready as scoped". My `executable` check failed it because the plan says "exact functions to be pinned in the PR after tracing the query issuance with debug logs", so no concrete location is named (re-grade evidence, confirmed on a second run). My rubric reads a plan that defers where the change goes as not startable by a stranger; the gold label treats that same plan as ready because the cause, scope, test and the deferral are all stated honestly. Every other check passed it (cause-grounded, scope-bounded, test-observable, honest-unknowns, thread-aware).

**Check rationale**

`| executable | The plan's named files or areas, its approach, and its order of work, read against the repo-facts block. | Pass if a stranger who has read only the plan could open the named file or area and start making the described change without asking the author anything. Fail if the location or the change is left vague ("improve the handling", "refactor as needed") or the plan depends on a step it never names. | required |`

It reads this way because the lecture's "a stranger could not start executing it" family is easy to turn into a structure check ("names a file", "has numbered steps"), and the check template warns that structure-shaped checks make graders disagree with themselves. So the pass condition is about the outcome: could someone who has read only the plan open a place and begin the change without asking the author. I rejected a version that only asked whether a file is named, because a plan can name a file and still say "refactor as needed". I did not revise this check after the full run: it was written this way from my first draft, and the run agreed on all three unbuildable packages (pkg-10, pkg-17, pkg-18).

**Trade-offs**

`executable` is strict about naming a location, which costs me pkg-14: a plan that is honest about not yet knowing the exact function is rejected. I accept that for now because loosening it ("a named area or a stated way to find the location passes") could flip the unbuildable packages (pkg-10, pkg-17, pkg-18), which currently agree and are the vague plans the check exists to catch. I did not change the rubric after the full run, so I made no further full run.

The other miss, pkg-20 (gold reject, accepted in the full run), did not repeat: the `--only` re-grade rejected it on `thread-aware`, quoting ghostty's "All AI usage in any form must be disclosed" with no disclosure in the comment. So that miss was grader variance, not a hole in the check, and it means 18/20 is a thin margin: another full run could land at 17 or 19. I would add a canary from thread-convention (pkg-04) to any future `--only` list for that reason.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
