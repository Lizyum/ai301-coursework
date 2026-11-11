# Contribution: Replace hard-coded red/green/blue text colors with theme-aware ones

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Lizbeth Yumbla
**GitHub Username:** Lizyum
**Issue:** https://github.com/OpenChemistry/avogadrolibs/issues/3187
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and tuned a live issue-scout from the sandbox skill**

I installed the Unit 5 scout over my Unit 1 skill, wrote a fit profile naming Python, JavaScript, C++, SQL, and Markdown/docs work, and wrote three `gh search issues` queries plus ranking filters (drop assigned issues, drop anything stale past 90 days, order by fewest comments first) tracing back to that profile. The first scan accepted all five of its top candidates, but reading the verdicts showed two gaps: the rubric had no check for the language an issue's thread was written in (it accepted a Vietnamese-primary translation issue without flagging the language barrier), and its AI-contribution-policy check only failed on an outright ban, treating silence and ambiguity the same way. I added a required language-fit check (English or Spanish only) and tightened the AI-policy check so hedging or non-committal AI language fails it too, since I'm contributing with AI assistance and don't want to walk into an unclear policy.

**Unit 6: Installed the voice guard and built a repo-memory file for my claimed issue**

I merged my unit 2-4 voice-guide copies into one canonical file, wrote an Upstream section (claims, questions, pings) with wrong/right pairs, and wired the guard hook into `~/.claude/settings.json`; the wiring test stopped `gh issue comment --help` as expected, proving the hook fires before anything posts. I also patched my issue-scout rubric's scope check (the unit-6 scope-feasible patch) after re-grading my three picks: the old check rejected any open design edge, which flipped Ocelot-Social#7127 from reject to accept with an "ambitious" difficulty note instead. Nothing broke in the guard install itself — I verified it by running the step-5 wiring test in a fresh session (settings only load at startup) and getting the expected block. For the repo-memory file, I ran one scoped orientation agent over just the `gamessinput/` subsystem my claimed issue touches, rather than mapping the whole avogadrolibs codebase, and distilled it into `CLAUDE.local.md`, excluded from the PR diff via `.git/info/exclude`.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I picked this issue directly from the AI301 Issue Selection site, filtered to my profile's languages and tags, then graded it with my own scout in direct mode alongside my two other shortlisted picks (Ocelot-Social-Community/Ocelot-Social#7127 and WorldconVotingSystems/nomnom#264). Under last week's rubric, two were accepted and one was rejected; this week, after I patched the scope check to stop rejecting every open edge (the unit-6 scope-feasible patch), I re-graded all three and all came back accepted with the following difficulty notes: avogadrolibs#3187 came back **moderate** (several files listed as separate sub-tasks, each individually bounded with exact line numbers), nomnom#264 also **moderate**, and Ocelot-Social#7127 flipped from a flat reject to accept with an **ambitious** note once the open "which pages?" question stopped being an automatic disqualifier. I chose avogadrolibs because it's the strongest stack fit for me (C++/Qt, and I want more production-grade C++ exposure) and the lowest-risk pick of the three: it's an active, well-reviewed project, the specific sub-task I'm taking (the gamesinput's highlighter's color-contrast and error-cue fix) is fully bounded by exact file and line numbers, and the maintainer is clearly responsive (same-day replies to other contributors' claims on this same issue). I'm hoping to learn how a mature C++ desktop codebase handles theming/accessibility concerns across many small UI surfaces, and how to scope my first PR tightly when an issue is shared across several contributors.

Reading the full thread mattered here: two other contributors had already claimed two of the seven file-level sub-tasks in this issue (`filebrowsewidget.cpp` and `coordinatetextedit.cpp`), so I deliberately claimed a different, still-open one (`gamesshighlighter.cpp`) rather than duplicating their work. An honest read of my own tool: the scout's old strict scope check would have been right to flag this issue as borderline-umbrella (it does list seven separate file-level deliverables), but it's genuinely *not* an umbrella issue in the harmful sense — the maintainer explicitly invites "one PR per file," so each sub-task is its own clean, independently-completable first contribution. The scope-feasible patch handled this correctly: it still passed the issue (one identifiable piece of work per sub-task), and the moderate difficulty note communicated the real shape of the issue instead of either rejecting it outright or silently ranking it like a single-file issue.

**Claim comment:** https://github.com/OpenChemistry/avogadrolibs/issues/3187#issuecomment-6785796862

**Toolkit**

- Tool: issue-scout (my search strategy, rubric, and this week's scope-feasible patch), used to grade my three Issue Selection site picks in direct mode
- What it produced: all three picks accepted, each with a difficulty note (avogadrolibs#3187: moderate; nomnom#264: moderate; Ocelot-Social#7127: ambitious, flipped from last week's reject)
- Difficulties: the old strict scope check rejected Ocelot-Social#7127 purely for an open "which pages?" question even though the core deliverable (a sticky toolbar) was clearly specified; it would likely have rejected this multi-file avogadrolibs issue too under a stricter reading
- What changed: replaced that check with the scope-feasible check plus a difficulty note (straightforward/moderate/ambitious), so an open edge informs the choice instead of silently filtering out real, claimable wild issues

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
