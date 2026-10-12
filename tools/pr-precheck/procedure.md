# Procedure: how this tool grades a PR package

## Read order

1. Live mode only: read `scope.md` first and apply the scope seam in SKILL.md (stop on a placeholder `Repo:` line). Then read `voice-guide.md` and keep its rules for the end.
2. Read the repo facts first (eval: the repo-facts block; live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`). Note every ask: template sections, checklist items, required entries (changelog, docs, issue reference), and the AI-use policy.
3. Read the issue and its thread (eval: issue and thread highlights; live: `gh issue view <n> --comments`). Note the one behavior reported and any explicit maintainer direction.
4. Read the plan before the diff (eval: the plan-context block; live: `plan.md`). Write down: the in-scope list, the not-in-scope line, the named files, each planned deliverable (code, tests, docs), the test plan with its repro steps and every failure mode it names, and any `## Deviations` entries. Reading the plan first is what makes the diff comparison a comparison and not an impression.
5. Read the diff and commit list with the plan's lists beside you (eval: the candidate PR's diff and commits; live: `git diff main...HEAD`, `--stat`, and `git log main..HEAD --oneline`). List every changed file and every hunk's purpose.
6. Read the test evidence against the plan's test plan (eval: the test-evidence section; live: `test_evidence.md`).
7. Read the PR title and description last (eval: the candidate PR's title and description; live: `pr_draft.md`), so that every claim in it is judged against what you already know the diff and evidence contain. Do not let its confidence shape what you read earlier.

## Evidence gathering

1. plan-matches-diff: build two lists. List A: the plan's named files, in-scope items, and deviation notes. List B: every changed file, and every hunk that changes behavior (new flag, config option, rename, refactor, new function). Mark each B item as covered by A, or uncovered. Then mark each planned deliverable in A as present in the diff, absent, or absent-but-disclosed (by the plan, a deviation, or the description).
2. description-matches-diff: copy each concrete claim from the description (what it changed, "exactly as planned", "docs updated", "tests added", "no other changes"). For each, find the hunk or pasted output that proves it, or record "not found" or "contradicted".
3. repro-rerun: copy the plan's repro steps and the failure modes the test plan names. In the evidence, find the pasted command and output for each. Record whether it runs on the changed path or only on a control, and whether before and after outputs differ observably.
4. checks-run: list the checks the repo asks for (template or the four make targets live, the test command the plan names). For each, find the pasted output with a visible outcome, or an honest statement that it was not run or failed. Record any bare claim such as "tests pass".
5. diff-clean: scan every hunk and commit message for debug prints, commented-out lines, unused or dead functions, TODO or wip notes, reformatting or import-order-only hunks, and hunks unrelated to the issue. Quote each hit with its file.
6. template-complete: for each ask recorded in Read order step 2, find where the description or diff meets it (issue reference, checklist, changelog entry in the diff, docs). Record each ask as met, empty, boilerplate, or missing. Record any ticked box the evidence contradicts and any thread maintainer ask left unaddressed.
7. ai-disclosure: record what the AI policy requires. Search the description (live: the Notes for Reviewers section) for a statement of AI use in the author's words, with the tool and the extent. Record present or absent.
8. honest-gaps: list each shortfall visible in steps 1 to 4 and record whether the description or plan states it.
9. Live mode only: record each voice-guide rule the title or description breaks, with the offending words quoted.

## Check execution

1. Run the checks in the order of the rubric table, using the facts recorded in Evidence gathering. Grade each by its stated pass condition, never by tone, length, or polish. Quote the one fact that decided it.
2. Grade `unclear` only when the evidence a check needs is absent from the package (for example, no test-evidence section at all, or no diff). Evidence that is present but weak is a `fail`.
3. Grade each check independently. One fact may fail two checks (an unrelated hunk can fail both plan-matches-diff and diff-clean); that is correct.
4. A deferral the plan, a deviation note, or the description states openly is not drift and not a hidden gap. Grade it by the pass condition, not as a failure just because the diff is smaller than the plan.
5. Do not grade a check as pass because the author is confident or the writing is good, and do not fail one because the PR is short.
6. Re-read the package only when a recorded fact is missing. Otherwise grade from the recorded facts.
7. Where this procedure is silent, add a "Procedure gap:" line to the summary rather than inventing a step.

## Verdict assembly

1. Collect the grade of every `required` check. Report `preferred` checks but ignore them for the verdict.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail` or `unclear`, the verdict is `reject`.
4. For a reject, the deciding check is the first failing (or unclear) required check in rubric order; make its evidence line the quoted fact that decided it, and list every other failing check as well.
5. In the JSON, list every check in rubric order with its grade and a one-line evidence quote.
6. Live mode: put voice-guide findings and procedure gaps in the summary before the JSON, never in it.
7. End the reply with the JSON block and nothing after it.
