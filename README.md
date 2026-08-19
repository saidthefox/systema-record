# systema-constructum — the record, offsite

This repository is a **backup of the published record**, not source code. It exists because every
other copy of the log lives in one building: the DL380 itself and a Mac Mini beside it. A fire
takes both. This is the offsite copy.

## What is here

- `prod/manifest.json` — ties the pieces together and to the World Chain pin
- `prod/seg-*.jsonl` — **sealed segments**, 1,000 events each, immutable forever
- `prod/tail.jsonl` — the live end of the record; changes constantly
- `prod/genesis-state.json` — the genesis snapshot the log folds from
- `prod/checkpoint.json` — the newest anchor receipt

Git is a good home for this *because of the segments*: a sealed segment is written once and never
rewritten, so history does not bloat. Only the tail and the manifest churn.

## Verify it — do not trust it

This repo is a convenience. The proof is the World Chain anchor, which nobody (including the
keeper) can rewrite:

    npx tsx tools/systema-verify.ts ./prod

That folds the record with the published law and checks the result against the pinned digest. A
backup you have not verified is a hypothesis.

## What this is not

It is not writable state, and restoring from it does not restore the kingdom's *keys* — office
keys live encrypted in Postgres under a master key held only in `/srv/docker/.env`. Losing those
is unrecoverable and no copy of the log changes that.
