# weftspun

**A recommendation engine for user-made 3D worlds** — it suggests the avatars, maps, and props a
creator is likely to want next.

Most recommenders need lots of click history before they can suggest anything, so brand-new
creations get ignored. weftspun instead reads an item's actual **content** — its 3D shape, textures,
images, and text — so a freshly-made asset can be recommended the moment it exists.

It's built as small, independent **Elixir** programs, each shipped as a single self-contained binary
that hands its results to the next — easy to run, swap, or scale on their own.

*The name:* content is **spun** into a compact code, and those codes are **woven** into suggestions —
like threads on a loom.

Open source (Apache-2.0 / MIT) · built for V-Sekai.
