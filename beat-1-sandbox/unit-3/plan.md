# Plan: delete_profile leaves embeddings behind (issue #32)

## Diagnosis

`delete_profile()` in `core/services/profile_service.py` (lines 74-113) deletes the `Review` rows, the `IngestedSource` rows and the `Profile` row, then commits. It never touches the vector store. A profile's embeddings live in a Chroma collection named `profile_<profile_id>` (the name `rag/retriever/hybrid.py:54` queries and `VectorStore.add_chunks` writes to), and nothing in the delete path removes that collection. `VectorStore` (`rag/retriever/vector_store.py`) has no method that deletes a whole collection; the only delete is `delete_by_source_id`, which removes chunks of one source and keeps the collection.

Repro evidence I rely on (my unit 2 repro comment on #32, macOS 13.5.1 arm64, Python 3.12.10, chromadb 1.5.9, Postgres 16, commit `f89c06f`):

```
Chunks before delete: 1 -> ['resume_f13822cb..._chunk_0']
delete_profile() returned: True
Profile row in Postgres after delete: None
Chunks after delete: 1 -> ['resume_f13822cb..._chunk_0']
```

The Postgres row is gone and `delete_profile()` returns `True`, but the chunk is still in the collection. That matches the cause: the SQL half of the delete runs, the vector half never does. The repro seeded the chunk directly through `VectorStore.add_chunks`, because the app's ingestion path (`_run_ingestion_pipeline` in `core/services/review_service.py`) does not write to the vector store yet. So this is a gap in the delete path, not something that depends on ingestion.

## Scope

In scope: deleting a profile's Chroma collection when the profile is deleted.

Not in scope: wiring the ingestion pipeline to the vector store, changing `delete_by_source_id`, per-source cleanup when an `IngestedSource` is removed on its own, the API route's response codes, anything in `rag/retriever/hybrid.py`, and a migration to clean up collections already orphaned by earlier deletes.

## Files I'll touch

- `rag/retriever/vector_store.py`: add `VectorStore.delete_collection(name)`.
- `core/services/profile_service.py`: call it from `delete_profile()` after the DB commit succeeds.
- `tests/unit/test_profile_service.py` (new): regression test.

## Approach

1. Add `delete_collection(self, collection_name)` to `VectorStore`. It calls `self.client.delete_collection(name=collection_name)` and treats "collection doesn't exist" as a no-op, since a profile that was never ingested has no collection. It logs `deleted_collection`.
2. In `delete_profile()`, after `await db.commit()`, delete `f"profile_{profile_id}"` through a `VectorStore`. The function gets an optional `vector_store` argument that defaults to `VectorStore()` (the same `.chromadb` default the class already has), so existing callers like the route need no change.
3. Order: Postgres first, Chroma second. If the Chroma delete fails after the commit, log it with `log.error("profile_vector_cleanup_failed", ...)` and still return `True`, because the profile is already deleted and the user's request succeeded. An orphaned collection is the current behavior, so this is never worse than today.
4. Write the test (below), then re-run `repro_issue32.py`.

## Test plan

Re-run my unit 2 repro steps (`repro_issue32.py`: create profile, add one chunk to `profile_<id>`, confirm it's there, call `delete_profile()`, re-check). Before the fix: `Chunks after delete: 1`. After the fix I expect `delete_profile() returned: True`, `Profile row in Postgres after delete: None`, and the collection gone or empty: `Chunks after delete: 0` and the script printing "No leftover chunks".

Automated: `tests/unit/test_profile_service.py` uses a `VectorStore` on a pytest `tmp_path` and a mocked DB session. It adds a chunk to `profile_<id>`, calls `delete_profile(db, profile_id, user_id, vector_store=store)`, and asserts the collection no longer exists. It must fail on `main` and pass with the change. A second case checks that deleting a profile with no collection still returns `True`. A third makes `delete_collection` raise and checks `delete_profile()` still returns `True`. Then `make check && make test-unit`.

## Risks and unknowns

- I haven't confirmed how chromadb 1.5.9 signals a missing collection (exception type) on `delete_collection`. I'll check against the installed version and catch only that case.
- The default `VectorStore()` creates a `.chromadb` directory relative to the working directory. I don't know yet whether the deployed app uses a different persist path or a shared instance. If it does, the optional argument is the hook, but the route would then need to pass the store in. I'll check how the app is configured before building.
- I haven't read the original issue text for any maintainer preference (for example, deleting by metadata filter instead of dropping the collection). I'll re-read the thread before posting.
- Mocking the DB session in the unit test means the SQL half isn't exercised by the automated test; the repro script covers that against real Postgres.

## Deviations

No changes fall under this category.
