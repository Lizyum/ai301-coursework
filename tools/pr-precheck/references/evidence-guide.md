# Evidence guide: where evidence lives in a PR package

In an eval bundle the sections are, in order: Repo facts, Issue, Thread highlights, Plan context (the plan, its repro evidence, and its test plan), then the Candidate PR: Title, Description, Commits, Diff, Test evidence. Use only the bundle text. In live mode the same things live in your working copy and on GitHub, as each heading below says.

## Plan fidelity (harness category: silent-drift)

Where it lives: in a bundle, the plan's scope in the Plan context block (the files it names, the "Not in scope" or Out line, the Changes list) and any deviation note, read against the Diff and the Description. Live: `plan.md` (including its `## Deviations` section) read against the output of `git diff main...HEAD` and the description in `pr_draft.md`.
What good looks like: every changed file and hunk falls inside the plan's named files and scope or a deviation note, and every promised change is in the diff or disclosed as left out. The description's claims about fidelity ("implements the plan exactly", "no other changes", "no functional changes outside the fix") are true of the diff. Silent drift looks like an unrequested rename, refactor, new option or setting, or a second file's edits that no note covers; or a promised change that is missing with no note; or a description claiming fidelity the diff contradicts. A deviation recorded in the plan is honest work; one that exists only in the diff is drift.

## Test evidence (harness category: not-tested)

Where it lives: in a bundle, the Test evidence section, read against the plan's test plan and the repro steps in the Plan context block, and against the checks the Repo facts block says the repo runs. Live: `test_evidence.md` (the before/after of your unit 2 repro, and the output of `make test-unit`, `make test-integration`, `make lint`, `make typecheck`), read against `plan.md`'s test plan and the commands in the PR template.
What good looks like: the plan's repro or named test is shown run before and after, each with an observable result (output text, exit code, color, a test that fails before and passes after), and the run exercises the code the diff changes. Each check the repo names has its command and real output; a failing check or "no tests ran", reported with its reason, counts as honest evidence. Not good: "tested locally", "tests pass", "works now" with nothing named; a before without an after; evidence that runs only a control path that never broke while the plan's failing case is never re-run; a repo check never run.

## Diff quality (harness category: unreviewable)

Where it lives: in a bundle, the Diff and Commits sections. Live: the output of `git diff main...HEAD` and `git log main..HEAD`.
What good looks like: the fix is visible and nothing unrelated rides along. Debris tells: added debug prints or logging, commented-out code, dead code, whitespace or formatting churn on lines the fix did not need, drive-by edits to other files, and generated or lock files the plan never mentioned. A small, mechanical hunk that the fix requires (such as an import) is not debris. Live, `git diff main...HEAD --stat` should list only the files the fix changes; `plan.md`, `comment.md`, `pr_draft.md`, and `test_evidence.md` must not appear in it.

## Standards and comms (harness category: standards-wall)

Where it lives: in a bundle, the Repo facts block (the PR template's asks, the contribution policy, any AI-use policy), read against the Description and the Diff, and the Thread highlights for maintainer direction. Live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the Path Review repo, read against `pr_draft.md`, with the issue thread for direction.
What good looks like: every template section has real content for this PR (Summary, Closes # with the issue number, one bullet per change in the Changes section, the Testing boxes ticked only where true, Screenshots or "N/A" with a reason, Notes for Reviewers with the AI-use disclosure). Asks the policy makes of the diff, such as a changelog or whatsnew entry, are in the diff or disclosed as left out. Explicit maintainer direction in the thread is followed or answered. Not good: sections left empty or as placeholder text, a required checklist ignored, the disclosure missing, the stated entry absent with no note, a ticked box that the evidence does not support. (Whether the description's claims match the diff is plan fidelity, above.)
