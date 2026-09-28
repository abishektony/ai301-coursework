# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

abishektony

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/59#issuecomment-5876503365 

Hi! I'd like to investigate issue #59, "Faithfulness checker scores claims unsupported when the context uses different words,". I will set up the reported pytest case, check the behavior against the issue's examples, and post the environment, commands, and observed result here before proposing a code change. I will also note any difference between my environment and the report's environment.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/59#issuecomment-5876520468 

**Environment**
- OS: Windows 11 (win32)
- Python: 3.11.7 (Anaconda)
- pytest: 7.4.0
- structlog: 26.1.0 (repo pins `structlog>=24.1.0`; I'm on a newer minor than the floor, calling it out since it's the one dependency the module imports)
- Code state: `main`, `rag/evaluator/faithfulness_checker.py` / `tests/unit/test_faithfulness_checker.py` unmodified

**Steps**
1. `pip install structlog` (not otherwise present in my environment)
2. `pytest tests/unit/test_faithfulness_checker.py -v -rx`
3. Directly exercise `_is_supported()` from a Python shell:
   ```python
   from rag.evaluator.faithfulness_checker import FaithfulnessChecker
   c = FaithfulnessChecker()
   c._is_supported("Knows Python", "python expert")
   c._is_supported("Knows Python", "The candidate knows Python")
   ```

**Observed**
- Test run: `18 passed, 4 xfailed`. `test_multiple_context_chunks` is XFAIL with reason `issue #59: faithfulness checker can never mark short claims as supported` — it does not appear as a bare failure because the suite already marks it as an expected failure for this issue.
- Re-running with `--runxfail` (ignoring the xfail markers) turns three of those into real failures, all tagged for #59:
  ```
  E       assert 0.2 < 0.0
  E       assert 0.0 > 0.5
  E       assert 0.2 < 0.0
  FAILED ...test_partial_support_returns_middle_score
  FAILED ...test_multiple_context_chunks
  FAILED ...test_multiple_claims_varying_support
  ```
  A fourth failure also shows up under `--runxfail` on the whole file (`test_none_context_chunk_text`, a `TypeError` at `faithfulness_checker.py:38`), but that one is tagged for issue #60, not #59, so it's a separate bug and out of scope here.
- Direct call output matches the issue's claim exactly:
  - `_is_supported("Knows Python", "python expert")` → `False`
  - `_is_supported("Knows Python", "The candidate knows Python")` → `True`
  - Same claim, same underlying fact, opposite verdicts depending only on whether the context happens to reuse two or more of the claim's words.
- `test_multiple_context_chunks` fails for a related but distinct reason: the claim and context share the words Python, JavaScript, and Docker, but `_extract_claims`'s sentence split only strips the trailing `.`/`!`/`?`, so the claim's tokens keep their commas (`"python,"`, `"javascript,"`). Those don't match the context's comma-free tokens, so only `"docker"` overlaps — one word short of the `>=2` threshold. Removing the commas by hand (no other change) flips the same call to supported. So the same `_is_supported` threshold produces two different failure paths: genuinely different wording (the issue's example) and identical wording that fails to tokenize the same way (this test's example).

**Outcome**
Reproduced, both variants. The behavior matches the issue's description: `_is_supported()` requires ≥2 shared non-stop *tokens* between claim and context, so support tracks literal token overlap rather than the underlying claim — and that overlap is sensitive to punctuation as well as wording. I have not yet looked into a fix approach — that's the next step before opening a PR.


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One run: agreement 20/20 scored items against the 18/20 pass bar (PASS). This is the run recorded in `eval-run.txt`.

**Package analysis**

`pkg-02` (source: `sharkdp/bat#3845`, gold category `wrong-target`, gold verdict `reject`). My rubric also rejected it, agreeing with gold. The candidate repro report ran `bat --line-range '18446744073709551614:'` — a *prefix* range (`N:`) — instead of the issue's *offset-from-end* range (`:-N`) that actually triggers the `capacity overflow` panic. The candidate's command produced a graceful CLI validation error (`Invalid value for '--line-range'`, exit 1), not the reported crash (`capacity overflow`, exit 101), and the report narrates this mismatched output as "confirming the reported crash." My `behavior-faithful` check failed this: the artifact shows a different exit code and a different error class than the issue's exact trigger, so it does not demonstrate the issue's specific behavior — it demonstrates an adjacent one (an off-by-syntax input hitting ordinary arg validation instead of the unbounded-`usize` allocation path).

**Check rationale**

`behavior-faithful | The issue context's described trigger and expected/observed behavior, compared with the repro report's output excerpt, log, screenshot, or other artifact and any control run. | Pass if the artifact demonstrates the issue's specific behavior, or if an exact or near-exact attempt includes the relevant output, control or expected comparison, and a meaningful environment difference that plausibly explains a cannot-reproduce result. A neighboring error, altered trigger, or unsupported inference fails. | required`

I wrote it this way after the wrong-target packages (`pkg-02`, `pkg-08`, `pkg-16`, `pkg-17`) showed the same failure shape from different causes: a changed trigger (`pkg-02`'s prefix range instead of offset-from-end, `pkg-08`'s unbound variable instead of the reported path expression), a stale environment presented as current (`pkg-16`'s pandas 1.5.3 run against an issue confirmed on main), and a superficially matching artifact that on inspection shows the process still alive rather than crashed (`pkg-17`). An earlier draft of this check only asked "does the report include an output excerpt?", which every one of those four would have passed — they all have logs. I rejected that version because it rewards the presence of an artifact over whether the artifact is evidence of *this* issue, which is exactly what let the wrong-target packages slip through. The current wording forces a comparison against the issue's stated trigger and symptom, not just a check that *some* output exists.

**Trade-offs**

This wording gives up leniency toward honest near-misses: a report that attempts the right trigger but on a meaningfully different input (e.g. a slightly different large-N value, or a shell-quoting difference) and gets a similar-but-not-identical crash could still fail `behavior-faithful` even if the underlying bug is genuinely the same, unless the report explicitly draws that connection. I accept this because the alternative — grading on "plausibly the same bug" — is exactly what let `pkg-08`'s confident-but-wrong artifact pass a looser check. I did not need to re-run `--only` to confirm this: the four wrong-target packages in `gold-labels.json` already isolate the failure mode `behavior-faithful` is meant to catch, and my run agreed with gold on all four (`pkg-02`, `pkg-08`, `pkg-16`, `pkg-17`), so nothing needed adjusting there.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
