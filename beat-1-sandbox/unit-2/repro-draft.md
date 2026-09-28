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
