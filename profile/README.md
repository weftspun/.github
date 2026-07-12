# weftspun

**A FOSS, content-addressable, semantic-ID recommender for user-generated worlds.**

weftspun turns raw creative content — meshes, images, scene graphs, body poses — into
recommendations for a churning catalog of user-generated Godot assets (avatars, maps, props).
It is **allocentric and item-centric**: every asset lives in one shared, viewpoint-invariant
world-frame, a session is a sequence of item→item transitions, and new assets are recommendable
the moment they exist because their identity is derived from their *content*, not an opaque ID.

---

## The name — content spun into code-thread, woven as weft

The whole pipeline is a loom:

- **Spun** — the encoders take raw content (a mesh, a render, scene text, a body pose = raw *fiber*)
  and **spin it into thread**: a residual-FSQ quantizer turns continuous embeddings into discrete
  **semantic-ID codes** — a countable, weavable yarn.
- **Weft** — in weaving, the **warp** is the fixed lengthwise threads (here: the shared allocentric
  item catalog / world-frame), and the **weft** is the thread woven *across* it to make the pattern.
  A **session** — a user's basket of items — *is* a weft thread drawn through the catalog, and the
  generative model **weaves the next weft pick** (next-item generation, item→item).
- **Fabric** — the emergent recommendations, woven from spun code-thread.

*"Content spun into code-thread, woven as weft."* Encode → quantize → generate, end to end.
Family-coherent with the [`taskweft`](https://github.com/V-Sekai-fire) planner (same weft lineage).

---

## How it's built — Elixir · Burrito · hexagonal

Every repo is a small **hexagonal** cluster (Alistair Cockburn's ports & adapters):

- **`core/`** — zero-dependency domain logic (tests in `core/spec/` against fixtures)
- **`ports/`** — narrow `*_source` (input / driving) and `*_sink` (output / driven) interfaces,
  wired to peers via `ports/sibling-repos.txt`
- **`adapters/`** — implementations against real systems

Each cluster is an **Elixir** program shipped as a self-contained binary via **Burrito**. When a
cluster needs the ML stack it embeds it — **Pythonx** for Python/torch, **Fine** for native
**Lean / C / C++** — meeting at language-agnostic **`.sigs`** port boundaries.

Compose by connecting one component's **sink** to the next component's **source**.

---

## The clusters

| Repo | Role | Binding |
|---|---|---|
| **essential-tuple-lake** | ETNF feature store — UUIDv5 identity, relation integrity, parquet/DuckDB | pure Elixir |
| **corpus-relation-ingest** | datasets → ETNF relations | pure Elixir |
| **semantic-code-oracle** | the recommender domain — FSQ, tokenizer, decoder, metrics | Pythonx → native Nx |
| **unified-modal-embedder** | text + image, one shared space | Pythonx |
| **voxel-slat-encoder** | mesh → structured SLAT tokens | Pythonx / Fine |
| **headless-viser-render** | GPU-free three.js renders | Pythonx |
| **pose-anny-phenotype** | keypoints → canonical body retarget | Pythonx + Fine → `sinew-mocap` |

---

## Principles

- **FOSS only** — Apache-2.0 / MIT code, CC-BY / CC0 data. No non-commercial or gated dependencies.
- **Content-addressable identity** — a re-imported or rescaled duplicate resolves to the *same* key.
- **No cold start** — content-derived codes make brand-new assets recommendable immediately.
- **Decisions are logged** — every choice is an MADR.
