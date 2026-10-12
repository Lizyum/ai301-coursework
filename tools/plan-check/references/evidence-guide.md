# Evidence guide: where evidence lives in a plan package

In an eval bundle the sections are, in order: Repo facts, Issue, Thread highlights, Repro evidence, Candidate plan, Candidate plan comment. Use only the bundle text. In live mode the same things live in your posted repro comment and the issue thread on GitHub (repro evidence), `plan.md` (plan), `comment.md` (comment), and the repo's CONTRIBUTING and issue template (repo facts).

Inside "Candidate plan", look for these labeled parts: Cause, Change (with In/Out lines), and Test. Risks or Unknowns appear only in some plans.

## Diagnosis and grounding

Where it lives: the plan states its cause under "Cause" or "Diagnosis". The behavior that cause must explain is in the "Repro evidence" block: the numbered steps, the control runs, and the Expected/Actual lines. Live: your posted repro comment.
Example (calib-01): the Cause says the branch-commits view is not refreshed after a push. Repro steps 3 and 4 back it up: the color stays stale until the view is re-entered.
What good looks like: the cause explains every repro step, including the controls. A cause is wrong if a control shows the problem with the named component bypassed or off (for example, slow even with no pager), or if a step shows the damage is already done before the stage the plan blames. A cause cited only as "identified in this thread" is not evidence.

## Scope

Where it lives: the "In" and "Out" lines inside the plan's "Change" text, and the "Change"/"Changes" list. Compare these with the issue title and the repro's Actual line.
Example (calib-01): "In: the push completion callback in `pkg/gui/controllers/sync_controller.go`... Out: any change to how push status is computed, or to other views' refresh behavior."
What good looks like: one change, one or two named files, an explicit "not touching X" sentence. Warning signs: a Changes list with several unrelated fronts (refactor, migration, new option, redesign, new settings). Deferring extras in the out-of-scope line is fine. If there is no Out line at all, that is a gap, not a pass.

## Executability

Where it lives: the "Change", "Changes", or "Approach" text in the plan, plus any ordered steps.
Example (calib-01): it names the file and the action, "adds the commits context to its post-push refresh scope".
What good looks like: a named file or function and a concrete action, so a stranger can start without asking. Not good: "profile and optimize", "somewhere in the input stack", "upstream or vendored, whichever is easier", or an investigation with no chosen layer.

## Test plan

Where it lives: the plan's "Test" or "Test plan" text, read against the repro steps and the Expected line.
Example (calib-01): "repro steps above; at step 3 the color must flip without leaving the view." This is a manual re-run, and it names what will be seen.
What good looks like: it re-runs the repro steps and names what will be seen after the fix (an output, color change, exit code, timing, or a test that fails before and passes after). A manual check is fine. Not good: "run the full test suite", "should feel fast", "nothing else should break".

## Honesty

Where it lives: the plan's "Risks", "Unknowns", or "Deviations" text, and how confident the comment sounds.
What good looks like: unknowns are named as unknowns ("not sure whether tool X also needs this, will check"), and claims are limited to what the repro showed. False confidence is an unverified claim written as fact. Mid-build changes are recorded under "Deviations". A plan with no risks section is not a fail, because this check is preferred.

## Comms

Where it lives: the "Candidate plan comment", read against "Thread highlights" (maintainer replies, requests, stated direction) and the "Repo facts" block (bug-report template asks, contribution policy). Live: the issue thread, CONTRIBUTING.md, and the issue template.
Example (calib-01): the thread has 0 comments, so there is no maintainer direction. The policy asks for minimal PRs, and the comment says "keeping it minimal given the review-bandwidth note in CONTRIBUTING". The policy states no disclosure requirement.
What good looks like: the comment acknowledges what the maintainer asked for (follows it or says why not) and sticks to every rule in the repo's stated policy. If the thread has no direction and the policy has no requirement, the check passes. Boilerplate that never mentions the thread when the thread has maintainer direction is a fail.
