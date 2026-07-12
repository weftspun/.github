# weftspun build plan

The org's cluster build-out as a [taskweft](https://github.com/V-Sekai-fire) HTN plan.

- **`weftspun-build.domain.jsonld`** — the HTN domain: one action per repo (scaffold + test + ship an
  Elixir/Burrito hexagonal cluster), with dependencies following the ETNF wiring.
- **`weftspun-build.plan.json`** — the cached solve (ordered plan).

**State:** `essential-tuple-lake` ✅ (the store, everyone depends on it). Next: `corpus-relation-ingest`,
the encoders (`unified-modal-embedder`, `headless-viser-render`, `voxel-slat-encoder` — no cross-deps),
then `pose-anny-phenotype` (needs renders) and `semantic-code-oracle` (needs the features).

```
lake ─┬─ ingest ───────────────┬─ oracle
      ├─ embedder ─────────────┤
      ├─ slat ─────────────────┘
      └─ render ── phenotype
```
