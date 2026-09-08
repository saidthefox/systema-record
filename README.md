# Systema Constructum public record

This repository is the public, off-site mirror of the
[Systema Constructum](https://systema.quartermachines.website) append-only record and its historical
read-model exports. It contains data, not the application source code.

The live documentation, API, and browsable ontology are available at:

- [About Systema Constructum](https://systema.quartermachines.website/about)
- [Browse the public data](https://systema.quartermachines.website/data)
- [API documentation](https://systema.quartermachines.website/docs/api)
- [Independent verifier](https://github.com/saidthefox/systema-verify)

## Repository contents

### `prod/` — verifiable record

| Path | Purpose |
|---|---|
| `prod/manifest.json` | Names the record artifacts, hashes, and current World Chain checkpoint. |
| `prod/seg-*.jsonl` | Immutable sealed event segments of 1,000 events each. |
| `prod/tail.jsonl` | The current unsealed end of the append-only record. |
| `prod/genesis-state.json` | Committed state from which event-log replay begins. |
| `prod/checkpoint.json` | Latest external checkpoint receipt. |

### `archive/` — historical projections

The archive contains deterministic, creation-month-partitioned JSONL exports of entries,
definitions, edges, labels, challenges, stakes, holdbacks, contributions, and recorded judgment
reasoning from the era that predates the event log.

`archive/manifest.json` identifies every shard, its SHA-256 digest, the export time, and excluded
fields. Archive shards are point-in-time projections: an older shard can change when the current
status, payout, release state, or reputation of an older row changes. The month in a filename is the
row's creation month, not a claim of immutable event time.

## Verify the record

Verify the live public record directly:

    git clone https://github.com/saidthefox/systema-verify.git
    cd systema-verify
    npm install
    ./bin/systema-verify https://systema.quartermachines.website/log

Or clone this repository beside the verifier and check the local mirror:

    ./bin/systema-verify ../systema-record/prod

The verifier checks artifact hashes, the record hash chain, ruleset replay, required signatures,
and the state commitment published through the Systema checkpoint contract on World Chain. Its
[README](https://github.com/saidthefox/systema-verify#readme) documents the result codes and trust
boundaries.

## Integrity and trust boundaries

- `prod/` is the replayable record covered by the external checkpoint.
- `archive/` is a historical projection and carries no independent checkpoint proof.
- The genesis snapshot is hash-checked and committed, but assertions about the pre-log era cannot
  be recreated solely from subsequent events.
- A mirror is not proof of completeness by itself; compare its sequence and digest with the latest
  available World Chain checkpoint.
- An accepted ontology claim is a recorded governance outcome, not external certification of its
  factual accuracy.

## Publication and privacy boundary

The exporter uses an explicit allowlist. Credentials, private keys, password hashes, raw World ID
nullifiers, and private control-plane data are excluded. The archive manifest records the permitted
tables and fields, excluded categories, and content hashes; publication stops if an unlisted or
hash-mismatched artifact appears.

If you discover sensitive information in this repository, do not quote or reproduce it in a public
issue. Follow [SECURITY.md](SECURITY.md).

## Data reuse

The public dataset is offered under the [CC0 dedication and historical-rights boundary](DATA-LICENSE.md).
Stable identifiers and source URLs are worth preserving for reproducibility even though CC0 does
not require attribution.

## Updates

This mirror is maintained automatically. Sealed segments are append-only; the current tail,
manifests, checkpoints, and historical projection shards can advance as the public record changes.
Pull requests that directly edit generated data cannot become part of the canonical record. See
[CONTRIBUTING.md](CONTRIBUTING.md) for the appropriate contribution routes.
