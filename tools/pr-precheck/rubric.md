# Rubric: is this pull request ready to submit?

Grades are `pass`, `fail`, or `unclear`.

- `pass`: the pass condition is clearly met.
- `fail`: the pass condition is clearly not met, or the package has no text at all about this check.
- `unclear`: there is text about this check, but it is vague or ambiguous, so you cannot tell whether the pass condition is met. If you can judge it, grade it pass or fail instead.

A manual test is fine. The test does not have to be automated. A shortfall the plan's deviation notes or the description disclose openly is not a failure by itself: grade the change against the plan as amended by those notes.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | The diff's changed files and hunks, read against the plan's scope (the files it names, its In/Out lines, its Changes list) and any deviation notes. | Pass: every changed file and every hunk in the diff falls inside the plan's stated scope or a deviation note, and every change the plan promised is in the diff or is disclosed as left out. Fail: the diff contains a change the plan does not name and no deviation note covers (an extra refactor, rename, new option or setting, another file's edits), or the diff leaves out a promised change with no note. | required |
| description-matches-diff | The description's claims (what changed, "implements the plan exactly", "no other changes", "only touches X"), read against the diff and the commit list. | Pass: every claim the description makes about what the PR does is true of the diff, and nothing the diff changes is hidden by a claim that it does not. Fail: the description claims fidelity, minimality, or coverage that the diff contradicts (for example "no functional changes outside the fix" while the diff adds a setting), or describes changes the diff does not contain. | required |
| decisive-test-evidence | The test evidence, read against the plan's test plan and the repro steps. | Pass: the evidence shows the plan's repro (or the plan's named test) run before and after the change, with an observable result in each (output text, exit code, color, a test that fails before and passes after), and the run exercises the code path the diff changes. Fail: only "tested locally", "tests pass", or "works now" with no named observation; or the before/after is missing; or the evidence runs a path the fix does not touch (for example a control that never broke) while the plan's failing case is never re-run. | required |
| repo-checks-run | The test evidence, read against the commands the repo's PR template or contributing docs say to run (the tests, lint, typecheck, or similar). | Pass: each check the repo names has its command and its output shown, and a check that fails or has nothing to run is reported with its real output and the reason. Fail: a check the repo names is never run or its output is absent, or the evidence claims it passed without output. | required |
| diff-reviewable | The unified diff and the commit list, read hunk by hunk. | Pass: the fix is visible and nothing unrelated rides along. Fail: the diff contains debris that buries or distracts from the change: debug prints or logging added for debugging, commented-out code, dead code, whitespace or formatting churn on lines the fix does not need, unrelated drive-by hunks, or generated or lock files the plan never mentioned. | required |
| template-and-policy | The description, read against the repo-facts block's template asks (live: `.github/PULL_REQUEST_TEMPLATE.md`) and contribution policy (live: `docs/CONTRIBUTING.md`), including any AI-use disclosure rule; and the diff, for asks the policy makes of the diff itself (a changelog or whatsnew entry, a test, a docs update). | Pass: every section the template asks for has real content for this PR (not empty, not left as placeholder text), the issue is referenced the way the template says, any AI-disclosure the policy or template requires is present, and every ask the policy makes of the diff is met in the diff or disclosed as left out. Fail: a required template section or checklist is ignored or left as boilerplate, the stated disclosure is missing, or a stated ask (such as the changelog entry) is absent from the diff with no note. | required |
| maintainer-direction | The issue thread highlights (live: the issue's thread), read against the PR title, description, and diff. | Pass: any explicit maintainer direction is followed or the description says why it was not; if the thread has no direction, pass. | preferred |
| title-and-summary | The PR title concisely describes the change and the description's Summary reads cohesively against the diff. | Pass: the title names the change specifically and concisely in the diff and the summary says why it was needed. | preferred |

## Verdict rule

1. If every `required` check is `pass`, the verdict is **accept** (ready to submit).
2. If any `required` check is `fail`, the verdict is **reject** (hold).
3. An `unclear` on a `required` check counts as `fail`, so the verdict is **reject**.
4. `preferred` checks are graded and reported, but never change the verdict.
5. A shortfall the plan's deviation notes or the description disclose openly does not make a check fail by itself. Judge the change against the plan as the notes amend it.
