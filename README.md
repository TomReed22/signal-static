# Signal Static

A browser-based idle game. Earth is dying. You scan a procedurally generated galaxy for mysterious signals, decrypt them, send probes, and claim planets by **force, science, or diplomacy** to find humanity a new home.

> Every node begins as a mystery — a waveform, not a target. Curiosity drives; conquest is a response to what you learn.

**Play it:** https://tomreed22.github.io/signal-static/

Vanilla HTML/CSS/JavaScript — no frameworks, no build step. Just open `index.html`.

## Status

In active development, built one segment at a time:

1. ✅ **Galaxy exists** — seeded generation, node data model, SVG render, click-to-inspect
2. ⬜ **First light** — fog of war, probes that travel, reveal radius
3. ⬜ **The tick** — game loop, resource income, first upgrades
4. ⬜ **The choice** — signals with waveform hints; force / science / diplomacy
5. ⬜ **The tide** — interference reclaims undefended frontier nodes
6. ⬜ **It remembers** — save/load, offline progress
7. ⬜ **The dress** — the real visual pass

See [`docs/PROJECT_BRIEF.md`](docs/PROJECT_BRIEF.md) for the full design.
