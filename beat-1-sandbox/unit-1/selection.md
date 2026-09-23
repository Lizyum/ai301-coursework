# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/32

**Verdict output**

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
Scope: `codepath/pathreview-ai301-fa26-s1` confirmed in scope. Path Review house
rule applies (student claim comments don't block).

Checks:
- Maintainer alive — pass: last commit 2026-09-16, 6 days before today (2026-09-22)
- Repo in use — pass: archived: false; last push 2026-09-16
- Not already claimed — pass: assignees: none; 0 comments; no genuine linked/mentioned PR
- AI-contribution policy allows it — pass: docs/CONTRIBUTING.md has no statement on AI/generative tooling; silence passes
- Scope fits a newcomer — pass: one bounded fix (deleting a profile leaves its embeddings behind in the vector store) touching three named files (api/routes/profiles.py, core/services/profile_service.py, rag/retriever/vector_store.py) in service of a single change; concrete estimated effort (4-6 hours) given; no hedging, tracking-issue structure, unresolved design debate, or abandoned-attempt history
- Stack fit (preferred) — pass: Python backend work (API route, service, vector store), matches fit profile

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/32",
  "checks": [
    {"name": "Maintainer alive", "grade": "pass", "evidence": "last commit 2026-09-16, 6 days before today (2026-09-22)"},
    {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; last push 2026-09-16"},
    {"name": "Not already claimed", "grade": "pass", "evidence": "assignees: none; 0 comments; no genuine linked/mentioned PR"},
    {"name": "AI-contribution policy allows it", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no statement on AI/generative tooling"},
    {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "one bounded fix (delete_profile leaves embeddings in vector store) across api/routes/profiles.py, core/services/profile_service.py, rag/retriever/vector_store.py, all in service of one change; estimated effort 4-6 hours; no hedging or unresolved design question"},
    {"name": "Stack fit", "grade": "pass", "evidence": "Python backend (api/service/vector-store), matches fit profile"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Runs in order, each with the harness's own `agreement:` line quoted verbatim:

1. Smoke test, `--limit 3`: `agreement: 2/3 scored items` — issue-01 failed on Maintainer
   alive and Scope fits a newcomer.
2. `--only issue-01`, after loosening Maintainer alive from AND to OR: `agreement: 1/1
   scored items` — issue-01 now agreed with gold.
3. First full run: `agreement: 17/20 scored items  (bar: 18/20: below the bar)` —
   categories: `claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 1/4`.
4. `--only issue-01,10,15,20`, after moving Scope fits a newcomer back to `required` with
   two new fail clauses (abandoned-attempt history; hidden/unmade product decision):
   `agreement: 3/4 scored items` — issue-01 regressed (failed: Scope fits a newcomer).
5. `--only issue-01` alone (diagnostic, `--out`): `agreement: 1/1 scored items` — passed
   in isolation, revealing the failure was run-to-run model variance on a borderline case,
   not a code-level batching artifact (confirmed by reading `run_eval.py`'s `grade_one()`,
   which spawns one independent `claude -p` subprocess per item with no shared state).
6. `--only issue-01,10,15,20` re-run to confirm the flakiness: `agreement: 3/4 scored
   items` — same failure recurred.
7. After tightening the umbrella-vs-focused wording with an explicit decision test
   ("could one single PR reasonably implement all the sub-items together?"):
   `--only issue-01,10,15,20` run 1: `agreement: 4/4 scored items`; run 2: `agreement: 3/4
   scored items` — still flaky.
8. `--only issue-01` solo, three runs to capture the failing reasoning (`--out`):
   `agreement: 0/1 scored items`, `agreement: 1/1 scored items`, `agreement: 0/1 scored
   items` — the two failures both cited hedging language ("if needed," "lower priority")
   attached to an explicitly optional part of the issue, not the core deliverable.
9. After rewriting the underspecification clause to judge only the CORE deliverable, and
   to explicitly exempt hedging on parts the issue itself marks optional/lower-priority:
   `--only issue-01` solo, three runs: `agreement: 1/1 scored items` (all three).
10. `--only issue-01,10,15,20`, two more runs to confirm stability: `agreement: 4/4
    scored items` (both runs).
11. Full run: `agreement: 18/20 scored items  (bar: 18/20: PASS)` — categories:
    `claimed 4/4  clear-accept 6/8  dead-repo 3/3  policy 1/1  scope 4/4`. New misses:
    issue-04 and issue-19, both `clear-accept`, both failing Scope fits a newcomer.
12. Final confirming run, `--save-run eval-run.txt`: `agreement: 18/20 scored items
    (bar: 18/20: PASS)` — identical breakdown to run 11. This is the run recorded in the
    committed `eval-run.txt`.

**Issue analysis**

`issue-04` (source: `zxcalc/zxlive#555`, category: `clear-accept`). Gold label:
`accept`, note: `"small active repo, maintainer-filed bounded bug, unclaimed"`. My
rubric's verdict: `reject`, failing the check `Scope fits a newcomer`.

The issue's full text is: *"Missing several basic rule previews (#555) ... Including
remove identity, fuse spiders, remove self loops, etc."* — opened by `RazinShaikh`
(`COLLABORATOR`), labeled `good first issue`, with zero comments. My rubric's Scope
check reads the open-ended `"etc."` combined with the total absence of any maintainer
comment as evidence the core deliverable is underspecified, so it fails the check's
"CORE deliverable's own text is genuinely underspecified ... AND no maintainer has
confirmed or accepted it on record" clause. What the check's wording misses is that the
issue was *filed by a COLLABORATOR*, not merely commented on by one after the fact — the
gold note's phrase "maintainer-filed" is doing exactly this work. A maintainer opening
the issue themselves is already a form of confirmation; my rubric currently only credits
confirmation that happens in a comment thread *after* an issue exists, so a terse,
maintainer-authored bug report with no comments reads as unconfirmed when it should not.

**Check rationale**

From `rubric.md`, the `Scope fits a newcomer` row (quoted as currently written):

> "Decision test for sub-items: could one single PR reasonably implement or address all
> of them together, in service of one named feature, page, or fix? If yes, they are
> focused, not umbrella, even when they span several files or list several steps. ...
> Fail (umbrella or unresolved) if any of: ... the CORE deliverable's own text is
> genuinely underspecified — vague or open-ended success criteria, hedging language
> ("possibly," "if needed," "TBD"), or an explicit unresolved design/product question
> ... — AND no maintainer has confirmed or accepted it on record. ... A fully specified
> core plan is not underspecified just because no maintainer has commented yet; lack of
> maintainer engagement only fails this check when paired with genuine underspecification
> of the core deliverable itself."

This wording exists in its current form because an earlier version failed in two
opposite directions. First, as `preferred`, it let three genuinely bad-scope issues
(a self-described "megaissue," a request with years of abandoned PRs behind it, and a
bot-filed feature wish with no spec) get accepted, because no other required check could
catch those failure modes — moving it back to `required` fixed that. Second, once
required, an early version of the underspecification clause used "no maintainer comment"
as a trigger on its own, which wrongly failed a fully-specified docs issue (`issue-01`)
that simply had not been reviewed yet. The "CORE deliverable" and "optional part"
carve-outs were added specifically so hedging on a minor, explicitly-optional stretch
goal would not sink an otherwise complete plan.

**Trade-offs**

What this check's current wording gives up: it still treats "no maintainer has confirmed
or accepted it on record" as one half of the fail condition, without distinguishing
*who opened the issue*. `issue-04` and `issue-19` (re-run solo via `--only issue-04,
issue-19` during review) both flip to `reject` under this wording specifically because
they were opened by a `COLLABORATOR` with zero follow-up comments — the check has no way
to credit "the maintainer who filed this already implicitly signed off on it" the way it
credits "a maintainer replied to confirm this." The trade-off bought by not adding that
distinction: the wording stays a single, simpler test (confirmation-in-thread only)
instead of a three-way branch on author role, minimizing the room for the model to
misjudge who counts as a maintainer from bundle text alone. The cost is two known misses
in the `clear-accept` category, both maintainer-filed issues with no subsequent comments.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit to interests and time available: I'm full-stack with backend Python experience
   and wanted a task that crosses more than one layer of a real service rather than a
   single-file fix, since that's closer to the kind of debugging I'd actually do on the
   job. Issue #32 touches an API route, a service, and the vector store together, and
   its own estimated effort (4-6 hours) fits the time I have for this unit.

2. What the verdict identified correctly, and what I weighed that the rubric could not:
   the rubric correctly confirmed the mechanical facts — repo active, unclaimed, no
   AI-contribution restriction, and a bounded, concretely-specified fix. What it can't
   weigh is that #32 and #19 (my other accepted top choice) scored identically on every
   check, including Stack fit; choosing #32 over #19 was a personal preference for
   working across a request's full path (route → service → vector store) over a
   single-file algorithmic tool, which the rubric has no mechanism to rank on.

3. Anticipated difficulty in claiming it: the issue names the three relevant files
   directly, so the main difficulty I expect is understanding how `ingestion/pipeline.py`
   associates chunks with a `profile_id` well enough to delete the right ones from the
   vector store without a helper for that already existing — likely a half-day task
   given the stated 4-6 hour estimate, most of it spent reading the vector store's
   deletion API rather than writing the fix itself.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
