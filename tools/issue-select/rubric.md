# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | "last 5 default-branch commits" and "maintainer first-response sample" (Family 1 in evidence-guide.md). | At least 1 of the last 5 default-branch commits is dated within 90 days of the capture date (eval) or today (live), OR at least one reply in the sample comes from someone with Owner, Member, or Collaborator association within 30 days of the comment it replies to. | required |
| Repo in use | "archived:" flag, "latest release", and "last push to any branch" (Family 2 in evidence-guide.md). | Not flagged/shown as archived, AND (latest release is within 365 days of capture/today OR last push to any branch is within 180 days of capture/today). | required |
| Not already claimed | Assignees, linked PRs, and claim comments in the thread (Family 4 in evidence-guide.md). Per the Path Review house rule in scope.md, claim comments from other students on the target repo do not count as a claim. | No assignee is set, AND no linked or thread-mentioned PR is currently open against this issue. | required |
| AI-contribution policy allows it | The contribution policy (the fifth surface in evidence-guide.md: CONTRIBUTING.md, dedicated AI policy files, PR/issue templates). | No outright ban on AI-generated or AI-assisted contributions is stated. Disclosure, personal-understanding, testing, or human-review conditions are allowed and do not fail this check; silence on AI use also passes. | required |
| Scope fits a newcomer | The issue body and full comment thread (Family 3 in evidence-guide.md). | Decision test for sub-items: could one single PR reasonably implement or address all of them together, in service of one named feature, page, or fix? If yes, they are focused, not umbrella, even when they span several files or list several steps. If the sub-items are independent capabilities, unrelated bugs, or pointers to separate ticket/issue numbers — each of which could ship as its own standalone PR with no shared feature tying them together — they are umbrella. Apply this test before any other fail clause below. Pass (focused): the issue describes one bounded change serving a single feature/page/fix per that test. Fail (umbrella or unresolved) if any of: the issue is explicitly a tracking issue or index of other issues (a list of links/pointers to separate issue numbers is always umbrella, regardless of the test above); the sub-items fail the decision test above; the thread shows an unresolved design debate with no maintainer decision; a maintainer states the fix touches core internals (e.g. "this needs changes to the parser"); the ask is a pure usage/support question ("how do I get this to work?"); the issue's history shows multiple closed-and-unmerged PRs against it or repeated claim-then-unclaim cycles, regardless of how simple the current description reads; or the CORE deliverable's own text is genuinely underspecified — vague or open-ended success criteria, hedging language ("possibly," "if needed," "TBD"), or an explicit unresolved design/product question (scope boundaries, configurability, branding/policy, etc.) applied to the primary ask — AND no maintainer has confirmed or accepted it on record. Judge underspecification only against the core/primary deliverable. Hedging language attached to a part of the issue that the text itself marks as optional, a stretch goal, or lower-priority ("lower priority, but worth naming," "if needed," "consider also...") does not count against scope as long as the core deliverable has concrete file names, sections, or acceptance criteria — dropping that optional part entirely would still leave a complete, shippable core task. A fully specified core plan is not underspecified just because no maintainer has commented yet; lack of maintainer engagement only fails this check when paired with genuine underspecification of the core deliverable itself. Evidence too thin to apply any of these conditions (e.g. no comment thread and an ambiguous body) grades unclear, and unclear is treated as fail here: an issue whose real scope cannot be verified is not a bounded first issue. | required |
| Stack fit | Issue labels and any file paths/extensions named in the issue body, thread, or linked PR diffs, compared against the fit profile in scope.md (Python, JavaScript, C++, SQL, Markdown/docs, React/Express/Vue.js-adjacent work). | Issue's primary language/file type (from labels or referenced paths) matches one of: Python, JavaScript, C++, SQL, or Markdown/documentation. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if all five required checks (maintainer alive, repo in use, scope fits a newcomer, not already claimed, AI-contribution policy allows it) grade pass. Any required check graded fail or unclear rejects the issue. "Stack fit" never affects the verdict; it only ranks accepted issues, matched ones ranking above unmatched ones.
