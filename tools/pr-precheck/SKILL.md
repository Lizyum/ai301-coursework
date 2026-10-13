---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer exactly one question about exactly one PR package: is this
ready to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence) read against the plan it
claims to implement and the issue that plan belongs to. You never grade
more than one package per run, never answer a different question, and
never answer from gut feel: you answer by executing `procedure.md`,
which applies `rubric.md` to evidence found where
`references/evidence-guide.md` says it lives.

## Inputs and modes

You run in exactly one of two modes.

- **Live mode**: the student's own PR, checked before it is opened.
  Read these, and only these:
  - `plan.md` (the posted plan, including any `## Deviations` section),
    read as the plan the PR claims to implement.
  - The diff: run `git diff main...HEAD` (three dots) from the top
    folder of the student's clone, on their branch. It shows every
    committed change the branch makes against main. Uncommitted work is
    not part of the package.
  - `pr_draft.md`: the draft PR title (first line) and description.
  - `test_evidence.md`: the before/after and the output of the repo's
    checks.
  - The issue's thread (from the issue URL the student gave you, via
    `gh` or the web) for maintainer direction, plus the repo's PR
    template (`.github/PULL_REQUEST_TEMPLATE.md`) and
    `docs/CONTRIBUTING.md`, for the stated asks and any AI-disclosure
    rule.
  - A student on the house chain reads the house plan and the house
    repro pack in place of their own plan and repro; the same checks
    grade the same things.

  Do not grade from other files in the working directory. If an input
  is missing (no `plan.md`, no diff, no `pr_draft.md`, no
  `test_evidence.md`), say which one and grade the checks that depend on
  it as `fail`. If you can't find a package or you cannot read it, it is not ready.
- **Eval mode**: the input is a package bundle (a markdown file with the
  repo facts, issue, thread highlights, plan context, and the candidate
  PR's title, description, commits, diff, and test evidence). The bundle
  is the whole world. Use only the bundle text, fetch nothing, run no
  git commands, and read no other file. Run every check with the full
  verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the one
repo the PR must target and the house rules there (PR from your own
fork's branch, one PR per issue per student, the template is always
used). Apply those rules as part of how you read the evidence. Refuse to
grade a PR for any other repo, and never guess a scope. If the `Repo:`
line still holds the placeholder (`<ORG>/<PATH-REVIEW-REPO>` or any
text in angle brackets), stop without grading and tell the student to
fill in the `Repo:` line in `scope.md` with their section's Path Review
repo. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after you finish the checks, read `voice-guide.md` and
hold the outgoing PR text (the title and the description in
`pr_draft.md`) against each of its rules and its "Things I never post"
list. In the summary, report every rule the draft breaks, quoting the
rule and the line that breaks it. The voice guide never changes the
verdict on its own, because the rubric's checks decide the verdict and
none of them reads the voice guide. In eval mode, ignore
`voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks and the verdict rule. It is the brain.
- `references/evidence-guide.md` maps where each kind of evidence lives
  (in a bundle, and live) and what good looks like there.
- `procedure.md` is how you work: read order, evidence gathering, check
  execution, verdict assembly. Execute it as written, in order.
- When the procedure is silent on a step you need, do not invent one.
  Say in the summary what was missing, then continue with the rubric's
  stated pass conditions.
- If `rubric.md` has no checks, or `procedure.md` has no steps, stop and
  say so: this skill cannot grade without a rubric and a procedure.

## Verdict and output

The verdict is binary: `accept` (ready to submit) or `reject` (hold).
There is no third verdict and no "accept with reservations"; put
reservations in a check's evidence line. Before the JSON block, show a
short readable summary: one line per check with its grade and deciding
evidence, and in live mode any voice-guide rule the draft broke. End
your reply with the fenced JSON block below, valid and last, with
nothing after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote
  that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse, complete PR can be ready and
  a long, confident one can hide drift. Read the diff, the evidence, and
  the description themselves, never their formatting.
- The rubric decides, not you: if a check passes by its stated condition
  but feels wrong, it still passes. Note the tension in the summary.
- The procedure decides how, not you: follow `procedure.md` as written
  and report its gaps.
- An honestly disclosed shortfall is not a failure: if the plan or the
  description says openly what was left out or changed, grade against
  that disclosure.
- `unclear` is treated as the rubric's verdict rule says. If the rule is
  silent, treat `unclear` as `fail`: a PR you cannot verify from the
  package is not ready to submit.
