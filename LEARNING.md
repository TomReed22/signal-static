# LEARNING.md — Signal Static

Plain-English dev log. One dated entry per work session: what got built, why, and 2-3 JS concepts that showed up and where to find them.

---

## 2026-07-17 — Project setup

**What was built:**
- Fixed a file-naming accident: everything on disk had a doubled extension (`CLAUDE.md.md`, `index.html.html`, etc.) — renamed to the correct names so the game and GitHub Pages actually work.
- Initialised git, made the first commit (Segment 1 code + design docs).
- Set up GitHub CLI and a public repo + GitHub Pages for phone playtesting.
- Created this file and `PARKING_LOT.md`.

**Decisions:**
- Local `.claude/settings.local.json` is gitignored — it's machine-specific tool config, not project content.
- Git identity set locally to this repo only (not global), since this machine had none configured.

**Concepts seen so far (from Segment 1, `index.html`):**
- **Seeded RNG (`makeRng`)** — a *closure*: `makeRng(seed)` returns a function that "remembers" `seed` between calls. This is why calling `makeRng(89745)` twice gives you two independent generators that don't interfere with each other.
- **Array methods / `pick()`** — `arr[Math.floor(rng() * arr.length)]` is a manual random-index pick; later segments will introduce `map`/`filter`/`reduce` as the data model grows.
- **State vs. rendering split** — `nodes` (the array) is the "table"; `render()` only reads it. This is the load-bearing architecture rule for the whole project (see CLAUDE.md).
