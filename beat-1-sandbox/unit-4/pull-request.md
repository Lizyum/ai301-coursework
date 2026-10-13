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

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/171

**Branch**

fix/32-delete-profile-vector-cleanup

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (the only full run): 19/20 scored items, bar 18/20 PASS. I made no revisions after it, so the last score matches the agreement line in `eval-run.txt`: "agreement: 19/20 scored items  (bar: 18/20: PASS)".

**Package analysis**

pkg-08 (lazygit#5883, category clear-accept). Gold label: accept ("error toast instead of the dead stash prompt ... integration test added, lazygit checklist (i18n, tests, cheatsheets) satisfied"). My tool decided reject: the run table says "failed: repo-checks-run". My reading of why: the repo-facts block says the lazygit template's checklist includes "code formatted" and "text internationalised", while the package's test evidence shows `go test ./...`, the integration test, and `go generate ./...` but no output for a formatting check. My `repo-checks-run` pass condition asks for "each check the repo names has its command and its output shown", so a checklist item with no output read as a missing check. I have not re-graded pkg-08 to see the check's exact evidence line, so this is an inference from the package text and the check's wording.

**Check rationale**

> | repo-checks-run | The test evidence, read against the commands the repo's PR template or contributing docs say to run (the tests, lint, typecheck, or similar). | Pass: each check the repo names has its command and its output shown, and a check that fails or has nothing to run is reported with its real output and the reason. Fail: a check the repo names is never run or its output is absent, or the evidence claims it passed without output. | required |

It reads this way because the "not tested" failure family includes "the repo's own checks were never run", and I wanted a condition that someone else could apply by looking for a command and its output. I wrote the "fails or has nothing to run" clause so an honestly reported failure still passes: on my own PR, `make test-integration` printed "no tests ran" and `make typecheck` failed on numpy's stubs, and both counted as shown because the reason was given. I did not revise this check between runs, because I only ran the harness once. Its live-mode result did what I wanted: on my PR it failed the first time because the frontend check from CONTRIBUTING.md was never run, and it passed once I ran it and added the output.

**Trade-offs**

`repo-checks-run` gives up pkg-08: a PR whose repo checklist items (formatting, i18n) go unmentioned in the evidence gets rejected even though gold says accept. I accept that miss. Loosening the check to forgive unshown checklist items would risk flipping the not-tested packages that agree today (not-tested is 4/4), and I did not re-run any canaries with `--only`, so I can't say that risk is zero. I left it as is because 19/20 already clears the 18/20 bar and every category has a match (clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3).

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
