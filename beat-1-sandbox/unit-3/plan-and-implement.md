# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Lizyum

**Plan comment**

Link: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/32#issuecomment-6115466679

Comment:

Plan to resolve issue, built from my repro above (macOS 13.5.1, chromadb 1.5.9, commit `f89c06f`): after `delete_profile()` returned `True` the Postgres row was gone but the collection still had its chunk (`Chunks after delete: 1`).

Cause: `delete_profile()` in `core/services/profile_service.py` removes the reviews, ingested sources and profile row but never touches the `profile_<id>` collection, and `VectorStore` has no method to delete a whole collection.

Change: add `VectorStore.delete_collection()` in `rag/retriever/vector_store.py`, and call it from `delete_profile()` after the DB commit. If the Chroma delete fails after the commit, I'll log it and still return `True`, since the profile is already gone. Not touching ingestion wiring, `delete_by_source_id`, the route, or cleanup of collections already orphaned.

Test: re-run my repro and expect 0 chunks after delete, plus a new unit test in `tests/unit/test_profile_service.py` that fails on `main` and passes with the change.

Not sure yet how chromadb reports a missing collection, or whether the app uses a non-default persist path; I'll check both before building.

---

## Your branch

**Branch**

fix/32-delete-profile-vector-cleanup

**Evidence**

Same command for both runs, from the top of my fork's clone, with Postgres running via
`docker compose up -d`. `repro_issue32.py` is my Unit 2 repro: it creates a profile for the
seeded user `user1@example.com`, adds one chunk to that profile's Chroma collection
(`profile_<id>`), calls `delete_profile()`, then re-checks the collection and the Postgres
row. SQLAlchemy query echoes and timestamped log lines are left out below; the script's own
printed lines are unchanged.

```
.venv/bin/python repro_issue32.py
```

**Before** (commit `f89c06f`, fix not applied):

```
Created profile 6e5998b1-65fd-4939-8b76-9734b9a1344f
Chunks in 'profile_6e5998b1-65fd-4939-8b76-9734b9a1344f' before delete: 1 -> ['resume_6e5998b1-65fd-4939-8b76-9734b9a1344f_chunk_0']
delete_profile() returned: True
Profile row in Postgres after delete: None
Chunks in 'profile_6e5998b1-65fd-4939-8b76-9734b9a1344f' after delete: 1 -> ['resume_6e5998b1-65fd-4939-8b76-9734b9a1344f_chunk_0']

BUG CONFIRMED: embeddings survive profile deletion.
```

**After** (branch `fix/32-delete-profile-vector-cleanup`, commit `0ae79dd`):

```
Created profile 78fa73b1-f16a-4a83-bd28-fcfa1e2c9b30
Chunks in 'profile_78fa73b1-f16a-4a83-bd28-fcfa1e2c9b30' before delete: 1 -> ['resume_78fa73b1-f16a-4a83-bd28-fcfa1e2c9b30_chunk_0']
delete_profile() returned: True
Profile row in Postgres after delete: None
Chunks in 'profile_78fa73b1-f16a-4a83-bd28-fcfa1e2c9b30' after delete: 0 -> []

No leftover chunks -- could not reproduce.
```

The profile ids differ because each run creates a new profile. The only change is the last
count: 1 chunk before the fix, 0 after. I also added `tests/unit/test_profile_service.py`
(3 tests): all 3 fail without the fix and pass with it, and `make test-unit` gives
378 passed, 53 xfailed.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run (`--save-run eval-run.txt`): `agreement: 19/20 scored items  (bar: 18/20: PASS)`
   — categories: `clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3
   wrong-cause 4/4`. The one miss was `pkg-14` (gold `accept`, my rubric `reject`, failed
   `executable`). I did no other runs, so this is the run recorded in `eval-run.txt`.

**Package analysis**

`pkg-14` (source: `zellij-org/zellij#5174`, category: `clear-accept`). The gold label is
`accept`, with the note: "honestly scoped-down: reattach handshake fix with a regression-window
repro; defers the untestable Windows variant and says so; arguable on the deferral, ready as
scoped". My rubric said `reject`, and `eval-run.txt` records `failed: executable`.

My rubric read it that way because of one sentence in the plan's Files section: "exact
functions to be pinned in the PR after tracing the query issuance with debug logs". My
`executable` check fails a plan when "a real decision is left for build time", and "investigate"
is one of its examples, so a plan that says the function will be found by tracing later reads
as deferred work. The rest of the plan was fine for my other checks: the cause fits the repro
(0.44.1 clean, reattach leaks, fresh attach clean, cache control), the scope is one bounded
change with the Windows variant explicitly deferred, and the test plan names the 5-cycle
reattach loop. Where the gold label and my rubric differ is that the plan does name the layer
(the client attach path in `zellij-server` and `zellij-client`) and the action (drain pending
OSC responses before pane input is wired). A stranger could start from that, so the gold
label treats the open function name as a small gap, not a missing decision. My check treats
it as a missing decision.

**Check rationale**

From `rubric.md`, the `executable` row, as it reads now:

> | executable | The plan's Changes / Approach text. | Pass: a stranger could open the named file or function and start the change without asking the author. Fail: no file or area is named, or a real decision is left for build time ("somewhere", "whichever is easier", "investigate"). | required |

It reads that way because a plan is only ready to build from if the person reading it can
start without going back to the author. The starter's `executable` check already tested that,
and its wording and its examples of deferred decisions are what I kept. What I changed was the
layout: I split the pass and fail conditions into "Pass:" and "Fail:" so each can be read on its
own, and kept it `required`. I did not loosen the substance, because my run got
`unbuildable 3/3` (`pkg-10`, `pkg-17` and `pkg-18` all rejected, as gold says), and
`executable` is the check meant to catch plans that cannot be started. I did not check which
check rejected each of those three, so I can't say `executable` alone is responsible.
Loosening it to pass `pkg-14` risked those three, so I kept it as is.

**Trade-offs**

The `executable` check gives up `pkg-14`: it rejects an honest, well-scoped plan that names
the layer and the action but leaves the exact function to be found by tracing. That is the one
package whose result it changes, and it is the only miss in my full run (19/20). It is also a
case I accept it will keep missing: any plan that names the area and the action but leaves the
function open. I accepted that in exchange for catching plans that name no place at all, which is what
the `unbuildable` category is about (3/3 in my run). I did not re-run with `--only`
or add a canary, because I did not change the check after the full run. The reason nothing
else moved is that I stopped at one full run, so `eval-run.txt` is the record of the rubric I
am submitting.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
