# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|:--:|---|---|---|
| Environment-reproduced | The repro report's Environment section, read against the target environment named in the issue context / repo-facts block | Pass if the OS/version/hardware named match what the issue targets, or any difference is explicitly named and justified as still producing the issue; fail if the environment is unstated or a mismatch is unacknowledged | Required |
| Steps-followable | The repro report's Steps section, read against the issue's description of how to trigger the bug | Pass if a stranger could follow the same starting state to the same trigger without guessing a missing step; divergence from the issue's own steps (e.g. a different OS) is fine if it's named, not silent | Required |
| Behavior-matches-issue | The repro report's Behavior shown / artifacts (logs, output excerpts, screenshots, code blocks), read against the issue's stated symptom | Pass if the artifact depicts the same type of error or failure the issue describes (exact wording/syntax may differ) and is consistent with the contributor's own explanation of it; for an honest cannot-reproduce, pass if the report shows what was tried and why it didn't manifest, rather than requiring a matching artifact | Required |
| Outcome-stated-honestly | The claim comment's stated outcome, read against the artifacts and steps in its own repro report | Pass if the comment claims exactly what its evidence shows, no more; an evidenced cannot-reproduce is a pass, a confident "reproduced" or diagnosis resting on evidence that doesn't back it up is not | Required |
| Comms-specific-and-honest | The claim comment's text, read against the issue it responds to and any maintainer comments in the thread stating expected format or past rejections | Pass if the comment speaks to this issue's actual specifics (not boilerplate that could sit on any issue) and doesn't promise a timeline or certainty it can't back up; fail if it's generic/interchangeable or over-promises (e.g. a guaranteed fix-by date) | Required |
| AI-policy-met | The claim comment's text, read against the repo's stated AI-use policy (CONTRIBUTING.md section, AI_POLICY.md, or similar), if one exists | Pass if the repo has no stated AI-use policy, or the policy is permissive with no disclosure or authorship requirement, or the comment meets whatever the policy specifically asks for: an explicit disclosure statement if it requires disclosing AI use, or genuinely human-voiced writing specific to this issue if it instead requires comments be written by a human in their own words. Fail if a disclosure-required policy gets silence, or a human-voice-required policy gets a comment that reads as generic/interchangeable rather than written by someone who engaged with this issue — treat every package here as AI-assisted work by default, so a disclosure-required policy's silence is a fail, not unclear | Required |

## Verdict rule

Accept (ready to post) if every Required check passes. A single Required check that fails holds the package (reject).

`unclear` on a Required check counts as fail: if the package doesn't contain what's needed to judge it, that absence is itself a reason the package isn't ready — don't guess a pass. The one exception is AI-policy-met under a disclosure-required policy: there, "the bundle doesn't say whether AI was used" is not treated as unclear, because the package is assumed AI-assisted by default (see that check's pass condition) — silence resolves to fail, not unclear.

A package whose claim comment honestly states it could not reproduce the issue, and whose Environment, Steps-followable, and Behavior-matches-issue checks show a genuine, well-documented attempt, still accepts — the bar is proof of an honest attempt, not proof the bug exists.
