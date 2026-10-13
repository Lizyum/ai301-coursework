# Procedure: how this tool grades a PR package

These steps are for grading a PR, not for writing one. Follow them in order and do only what each step says. "Bundle" means the eval package; in live mode, the live source named in `references/evidence-guide.md` stands in for each bundle section.

## Read order

1. Read the repo facts first (bundle: the Repo facts block; live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`). Write down every section the template asks for, every contribution rule, and any AI-use disclosure requirement. Note any ask the policy makes of the diff itself (a changelog entry, a test, a docs change).
2. Read the issue and the thread highlights. Write down any explicit maintainer direction (a requested action, a stated cause, a question that was asked).
3. Read the plan (bundle: the plan-context block; live: `plan.md`) before the diff. Write down: the files it names, its In and Out scope lines, each item in its Changes list, its test plan, the repro steps it built on, and any deviation notes (live: the `## Deviations` section). This is the yardstick; the diff is read against it, so it must be in your head first.
4. Read the diff (bundle: the Diff section; live: the output of `git diff main...HEAD`) and the commit list. List every changed file with a one-line note on what each hunk does. Mark any hunk that is a debug print, commented-out code, dead code, or formatting-only.
5. Read the test evidence (bundle: the Test evidence section; live: `test_evidence.md`).
6. Read the PR description last (bundle: the Description section; live: `pr_draft.md`, with the title on its first line). Write down each claim it makes about what the PR does, and which template sections it fills.

## Evidence gathering

Take each check in the rubric's table order and write notes for it.

1. For diff-matches-plan: put the list of changed files from Read order step 4 next to the plan's named files and Changes list. Write "in plan", "in deviation note", or "not in plan" next to each changed file and hunk. Then list each Changes item from the plan and write "in diff" or "missing" (and, if missing, whether a note or the description discloses it).
2. For description-matches-diff: take each claim from Read order step 6 and write "true of diff" or "contradicted" with the diff line or file that decides it. Pay most attention to words like "exactly", "only", "no other changes", and "no functional changes". Also list any diff change the description never mentions.
3. For decisive-test-evidence: copy the plan's test plan and its repro steps. Find the matching before and after in the test evidence and copy the observable result in each (the output, exit code, color, or test result). Write which file or function the evidence's command exercises, and whether the diff changes it. If only one side (or neither) is shown, write "before missing" or "after missing".
4. For repo-checks-run: list every command the repo's template or docs name. Next to each, copy the output line the evidence shows, or write "not shown". A check that failed or had nothing to run, with its real output and a reason, counts as shown.
5. For diff-reviewable: from the hunk notes in Read order step 4, list each piece of debris (a debug print, commented-out code, dead code, formatting churn, an unrelated hunk, an unmentioned generated or lock file) with its file and line, or write "none found".
6. For template-and-policy: for each template section from Read order step 1, quote the description text that fills it or write "empty" or "boilerplate". Write whether the issue is referenced the way the template says. If a disclosure is required, quote it or write "missing". For each ask the policy makes of the diff, write the changed file that meets it or "absent".
7. For maintainer-direction: for each direction from Read order step 2, quote where the title, description, or diff engages it, or write "not engaged". If there is none, write "no direction".
8. For title-and-summary: write the title and the first line of the Summary, and whether each matches what the diff changes.

## Check execution

1. Run the checks in the rubric's table order. Judge each one from your evidence notes. Re-read the package only where your notes for that check are empty.
2. Apply the pass condition exactly as written. If the notes show a fail condition, grade `fail` even when the rest of the PR is strong. If they show the pass condition clearly met, grade `pass` even when the PR is terse.
3. If the package has no text at all on that check (for example, no test evidence section), grade `fail`. If text exists but is vague or ambiguous, so you cannot tell whether the pass condition is met, grade `unclear`. Use `unclear` only after you tried to apply the pass condition and could not decide.
4. A shortfall that the plan's deviation notes or the description disclose openly is judged against the plan as amended. A change in the diff that exists only in the diff, with no note, is not covered.
5. For each check, write one line: the grade, and the quote or fact that decided it.
6. Do not accept the description's claims as evidence of the diff. The diff is the evidence; the description is graded against it.

## Verdict assembly

1. List the grades of the required checks in rubric order: diff-matches-plan, description-matches-diff, decisive-test-evidence, repo-checks-run, diff-reviewable, template-and-policy.
2. If all required criteria are `pass`, the verdict is `accept`.
3. If any required criteria are `fail` or `unclear`, the verdict is `reject`.
4. Report maintainer-direction and title-and-summary, but ignore them when deciding.
5. In the output summary, put the deciding check first. When more than one required check failed, the deciding check is the first failing required check in rubric order; for it, quote the fact or line that decided it. Then give one line per remaining check.
6. In live mode, after the check lines, add the voice-guide report from SKILL.md. Then write the JSON block, with the verdict last.
