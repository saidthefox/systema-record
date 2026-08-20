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

- `archive/` — **the era before the log**, and the projections beside it: every entry, definition,
  edge, label, challenge, stake, holdback and contribution, plus all 60,000 judge votes with the
  13 MB of written reasoning that is the most interesting content in the system. Month-sharded
  JSONL, with `archive/manifest.json` naming every shard and its sha256.

Git is a good home for this *because of the segments*: a sealed segment is written once and never
rewritten, so history does not bloat. Only the tail and the manifest churn. The archive is built to
the same shape — shards are partitioned by month and byte-stable, so a closed month regenerates
identically forever and a nightly re-export of unchanged data costs git **nothing** (measured: 63 MB
of JSONL, 15 MB in `.git`, and a second identical export added zero bytes).

## What the archive is NOT

An allowlist, never a dump. The database it comes from holds office signing keys, wallet keys,
password hashes, API key hashes and raw World ID nullifiers, and none of that is here: the exporter
names every table and column that may leave, and runs credential patterns over the bytes it actually
produced before any of them are allowed to stand. `archive/manifest.json` lists what was excluded and
why. The hourly push additionally refuses if any file under `archive/` is unlisted, missing, or
altered from the sha256 the manifest recorded.

Two honest limits. The archive is a **projection**, not the record — it is Postgres as it stood at
`asOf`, so derived counters (reputation, totals, activity) are point-in-time, and it carries no
independent proof. The record is `prod/`, and that is the thing the anchor covers.

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
