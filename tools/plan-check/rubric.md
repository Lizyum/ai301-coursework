# Rubric: is this plan ready to post and build from?

Grades are `pass`, `fail`, or `unclear`.

- `pass`: the pass condition is clearly met.
- `fail`: the pass condition is clearly not met, or the package has no text at all about this check.
- `unclear`: there is text about this check, but it is vague or ambiguous, so you cannot tell whether the pass condition is met. If you can judge it, grade it pass or fail instead.

A manual test is fine. The test does not have to be automated.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| cause-fits-repro | The plan's Cause/Diagnosis, compared with each numbered step of the repro evidence. Pay most attention to control runs (same action, one thing changed or removed) and to timings. A cause that is only quoted from the issue thread is not evidence. | Pass: every repro step agrees with the plan's cause. Fail: any control run, timing, or output shows the problem still happening with the named cause removed, or the plan fixes the symptom while the repro points at a different layer. | required |
| scope-bounded | The plan's In scope / Out of scope lines and its Changes list, compared with the issue title and the repro's Actual behavior. | Pass: one bounded change that fixes what the issue shows, names the file or area it touches, and says what it will not touch. Fail: the Changes list adds work nobody asked for (refactors, new options, cleanups), even if the core fix is right. Extras the plan explicitly defers in its out-of-scope line are fine. | required |
| executable | The plan's Changes / Approach text. | Pass: a stranger could open the named file or function and start the change without asking the author. Fail: no file or area is named, or a real decision is left for build time ("somewhere", "whichever is easier", "investigate"). | required |
| test-observable | The plan's Test plan, compared with the repro steps and the Expected/Actual lines. | Pass: names something you can see that flips when the fix works (an exit code, an output line, a timing, a test that fails before and passes after), tied to the repro steps. Re-running the repro with the expected result written down counts. Fail: "run the full suite", "should feel fast", "nothing else should break", or no stated observation. | required |
| comms-fit | The plan comment, compared with the thread highlights (maintainer replies and requests) and the repo-facts block (bug template, contribution policy, AI-use rules). | Pass: the comment follows any explicit maintainer direction (or says why it differs) and meets every contribution rule in repo-facts. If there is no direction and no rule, pass. | required |
| honest-unknowns | The plan's Risks/Unknowns and the comment's confidence, compared with what the repro actually showed. | Pass: open questions are stated as open, and nothing unverified is stated as fact. Never fails the verdict, because it is preferred. | preferred |

## Verdict rule

1. If every `required` check is `pass`, the verdict is **ready** (accept).
2. If any `required` check is `fail`, the verdict is **hold** (reject).
3. An `unclear` on a required check counts as a `fail`, so the verdict is **hold**.
4. `preferred` checks never change the verdict.
