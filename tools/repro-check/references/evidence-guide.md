# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->

Where it lives: in an eval bundle, the repro report's "Environment" section, read against the target environment named in the issue context / repo-facts block. In live mode, the draft claim or repro comment's environment section, read against what the issue thread states.

What good looks like: the OS, application version, and hardware/software named match what the issue targets, or any difference is explicitly named and justified as still producing the issue (not silently substituted).

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->

Where it lives: in an eval bundle, the repro report's "Steps" section, read against the issue's description of how to trigger the bug. In live mode, the draft claim/repro comment's steps versus the issue thread's own repro steps.

What good looks like: a stranger could follow the same starting state to the same trigger without guessing a missing step. Steps may diverge from the issue's own (e.g. a different OS) as long as the divergence is named, not silently substituted.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
Where it lives: in an eval bundle, the repro report's "Behavior shown" / artifacts section (logs, output excerpts, screenshots, code blocks), read against the issue's stated symptom. In live mode, the artifacts pasted into the draft claim/repro comment.

What good looks like: the artifact shows the same type of error or failure the issue describes, even if the exact wording or syntax differs (e.g. a different stack trace formatting for the same underlying exception) — and it's consistent with the contributor's own explanation of what it shows. An artifact depicting a different problem than the one filed does not count, even if it shows something is clearly broken.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->

Where it lives: in an eval bundle, the claim comment's stated outcome, read against the artifacts and steps in its own repro report. In live mode, the draft comment's conclusion versus its own attached evidence.

What good looks like: the comment claims exactly what its evidence shows, no more. An honest "could not reproduce" backed by a clean, followable attempt is a pass; a confident "reproduced" or diagnosis resting on evidence that doesn't actually back it up is not — flag the mismatch rather than taking the claim at its word.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->

Where it lives: in an eval bundle, the claim comment's text, read against the repo's CONTRIBUTING.md (and AI-use disclosure policy file, if one exists) and the issue it responds to. In live mode, the draft comment versus the repo's contribution docs, plus the issue thread itself: look for maintainer comments that spell out expected format/content, or that reject a prior contribution for missing something — treat those as the standard this comment must meet.

What good looks like: the comment speaks to this specific issue (references its actual symptom or steps, not boilerplate that could be pasted onto any issue), follows the repo's stated conventions, discloses AI assistance where the policy requires it, and meets any standard a maintainer already stated or enforced in the thread. A comment that only reads well because it's vague or templated does not pass.


