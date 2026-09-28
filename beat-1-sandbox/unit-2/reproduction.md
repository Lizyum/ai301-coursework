# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

[Your GitHub username, exactly as it appears on your profile — no `@`, no profile URL. Your
comments upstream are identified by this name.]
GitHub Username: lizyum

---

## Posted upstream

**Claim comment**

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

Link: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/32#issuecomment-5863666283

Comment: I would like to pick this up, I am currently working on reproducing the issue. I'll be working across `api/routes/profiles.py`, `core/services/profile_service.py`, and `rag/retriever/vector_store.py`, and will post a repro report once I've confirmed the behavior.

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

Link: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/32#issuecomment-5863938302

Comment: 

Environment: macOS 13.5.1 (arm64), Python 3.12.10, chromadb 1.5.9, Postgres 16 (Docker),
repo at commit `f89c06f` on `main`.

Steps: seeded a local Postgres, then ran a script against seeded user `user1@example.com`:
create a profile, add one chunk to that profile's Chroma collection (`profile_<id>`, same
name/shape `VectorStore.add_chunks` writes), confirm it's there, call `delete_profile()` —
the same function `DELETE /api/profiles/{id}` calls — then re-check the collection and the
Postgres row.

Expected: after `delete_profile()` returns `True`, the profile's Chroma collection should be
empty.

Actual:
```
Chunks before delete: 1 -> ['resume_f13822cb..._chunk_0']
delete_profile() returned: True
Profile row in Postgres after delete: None
Chunks after delete: 1 -> ['resume_f13822cb..._chunk_0']
```

Reproducible: the Postgres row is deleted, but the embedding remains. Confirmed
why in `core/services/profile_service.py:74-113` — `delete_profile()` deletes `Review` and
`IngestedSource` rows and the `Profile` row, but never calls anything in
`rag/retriever/vector_store.py`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. First full run (rubric with `Comms-follows-conventions` as a single `preferred` check
   covering both boilerplate/over-promising comments and AI disclosure): `agreement: 18/20
   scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)`
   — categories: `clear-accept 8/8  disclosure 0/1  no-evidence 4/4  unfollowable-comms 2/3
   wrong-target 4/4`. `pkg-19` and `pkg-20` both graded `accept` against gold `reject`.
2. `--only pkg-19,pkg-20` (diagnostic, `--out`): `agreement: 0/2 scored items`. Per-check
   output showed both packages failing only `Comms-follows-conventions` (fail for pkg-19's
   boilerplate/guaranteed-fix comment, unclear for pkg-20's missing AI disclosure) — but
   since that check was `preferred`, its failure never changed the verdict.
3. After splitting the check into two `required` checks (`Comms-specific-and-honest` and
   `AI-disclosure`), re-run on the two fixes plus four canaries (`--only
   pkg-19,pkg-20,pkg-07,pkg-03,pkg-01,pkg-06`): `agreement: 5/6 scored items` — pkg-19 and
   pkg-20 now agreed, but `pkg-03` (gold `accept`) newly flipped to `reject`, failing the new
   `AI-disclosure` check.
4. After rewriting `AI-disclosure` (renamed `AI-policy-met`) to distinguish a
   disclosure-required policy from a human-voice-required policy, re-run on the same six
   packages: `agreement: 6/6 scored items`.
5. Full confirming run: `agreement: 20/20 scored items  (bar: 18/20: PASS)` — categories:
   `clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target
   4/4`.
6. Final run, `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`
   — identical breakdown to run 5. This is the run recorded in the committed `eval-run.txt`.

**Package analysis**

`pkg-20` (source: `ghostty-org/ghostty#13604`, category: `disclosure`). Gold label:
`reject`, note: `"excellent repro on every proof check; ghostty's stated AI policy requires
disclosing all AI usage and the comments do not disclose (course packages are treated as
AI-assisted work); the one-item category the floor exists for"`. My rubric's first-pass
verdict: `accept` — every proof check (environment, steps, behavior, honesty) genuinely
passed, and the one check that caught the missing disclosure was graded `unclear` (the
bundle gives no way to tell whether AI was used) and was weighted `preferred`, so per my
own verdict rule it could never flip the package to reject. The package is a real trap: it
has no weak proof anywhere, so any rubric that treats disclosure as a nice-to-have rather
than a gate will accept it. Fixing it took two changes: promoting the disclosure check to
`required`, and writing an explicit rule that a disclosure-required policy's silence counts
as `fail`, not `unclear` — because the course treats every package as AI-assisted by
default, so "the bundle doesn't say" is not neutral information here.

**Check rationale**

From `rubric.md`, the `AI-policy-met` row (quoted as currently written):

> "Pass if the repo has no stated AI-use policy, or the policy is permissive with no
> disclosure or authorship requirement, or the comment meets whatever the policy
> specifically asks for: an explicit disclosure statement if it requires disclosing AI use,
> or genuinely human-voiced writing specific to this issue if it instead requires comments
> be written by a human in their own words. Fail if a disclosure-required policy gets
> silence, or a human-voice-required policy gets a comment that reads as
> generic/interchangeable rather than written by someone who engaged with this issue —
> treat every package here as AI-assisted work by default, so a disclosure-required policy's
> silence is a fail, not unclear."

This wording exists because an earlier, narrower version only recognized one shape of AI
policy — an explicit disclosure statement — and demanded that shape everywhere. That
version fixed `pkg-20` but broke `pkg-03` (`BurntSushi/ripgrep#2779`, gold `accept`):
ripgrep's policy doesn't ask for a disclosure sentence at all, it asks that comments be
"written by humans in their own words," and the candidate comment already met that bar by
being specific and voiced, not generic. The check now reads the policy's actual shape
before deciding what "met" means, instead of assuming every AI policy wants the same thing.

**Trade-offs**

Canary re-run with `--only pkg-19,pkg-20,pkg-07,pkg-03,pkg-01,pkg-06`: after the
`AI-policy-met` fix, `pkg-03` flipped back from `reject` to `accept` (matching gold) while
`pkg-19`, `pkg-20`, `pkg-07`, `pkg-01`, and `pkg-06` all still agreed — so distinguishing
the two policy shapes fixed the miss it was meant to fix without moving any of the
already-agreeing packages I checked. What this check still gives up: it only distinguishes
two policy shapes (disclosure-required vs. human-voice-required) because those are the two
the eval set exercises. A repo whose policy asks for something else entirely (e.g. a
specific disclosure template, or a required label on the PR rather than the comment) would
fall through to whichever branch it resembles more, and the check has no way to flag "this
policy shape isn't one of the two I know how to judge" rather than silently misapplying one
of them.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
