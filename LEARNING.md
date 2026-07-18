# LEARNING.md — Signal Static

Plain-English dev log. One dated entry per work session: what got built, why, and 2-3 JS concepts that showed up and where to find them.

---

## 2026-07-17 — Project setup

**What was built:**
- Fixed a file-naming accident: everything on disk had a doubled extension (`CLAUDE.md.md`, `index.html.html`, etc.) — renamed to the correct names so the game and GitHub Pages actually work.
- Initialised git, made the first commit (Segment 1 code + design docs).
- Installed GitHub CLI locally, reconciled with an earlier stub repo you'd made, and got the public repo + GitHub Pages **live**: https://tomreed22.github.io/signal-static/
- Created this file and `PARKING_LOT.md`.

**Decisions:**
- Local `.claude/settings.local.json` is gitignored — it's machine-specific tool config, not project content.
- Git identity set locally to this repo only (not global), since this machine had none configured.
- A `signal-static` repo already existed from an earlier manual attempt (a placeholder README + the pre-exercise `index.html`). Our local repo was a strict superset, so we made it the source of truth with a force-push rather than merging junk history.

**Gotchas worth remembering:**
- **GitHub Pages "source path" matters.** The old repo had Pages set to serve from `/docs`, but our game's `index.html` is at the repo root — so the site 404'd. Fixed by pointing Pages at `/` (root). `docs/` only holds design files and the mockup, not the playable game.
- **Changing the Pages source path doesn't auto-rebuild.** After switching `/docs` → `/`, the *config* updated but the *published site* was still the old build, so it kept 404ing. Had to trigger a fresh build explicitly. Lesson: after changing Pages settings, force a rebuild (or push a commit) and re-check the live URL, don't trust the "built" status alone.

**Concepts seen so far (from Segment 1, `index.html`):**
- **Seeded RNG (`makeRng`)** — a *closure*: `makeRng(seed)` returns a function that "remembers" `seed` between calls. This is why calling `makeRng(89745)` twice gives you two independent generators that don't interfere with each other.
- **Array methods / `pick()`** — `arr[Math.floor(rng() * arr.length)]` is a manual random-index pick; later segments will introduce `map`/`filter`/`reduce` as the data model grows.
- **State vs. rendering split** — `nodes` (the array) is the "table"; `render()` only reads it. This is the load-bearing architecture rule for the whole project (see CLAUDE.md).

---

## 2026-07-17 — Segment 1 exercise: new node types + name generator

**What was built:** the Segment 1 ritual exercise, and then some. Added two new node types (`wreckage`, `comet`) to `generateGalaxy()`'s type pool and `COLORS`, then rebuilt `makeName()` from a single naming pattern into a 7-pattern generator (`KEPLER-418`, `HUBBLE 418`, `GAIA-418A`, `NEXUS-IV`, etc.), backed by a much bigger `PREFIXES` list, a `SUFFIXES` string, and a `ROMAN` array.

**Concepts that showed up:**
- **`switch`/`case` with a `default`** — `makeName()` picks one of 7 patterns via `randInt(0, 6)`; cases `0`–`5` are explicit, `default` catches the 7th value. Each `case` `return`s immediately, so there's no fall-through to worry about.
- **Strings index like arrays** — `pick(SUFFIXES)` works even though `SUFFIXES` is a string (`"ABCDEFGHIJKLMNOPQRSTUVWXYZ"`), not an array, because `pick()` just does `arr[Math.floor(rng() * arr.length)]`, and JS strings support both `.length` and bracket-indexing the same way arrays do.
- **Template literals chained back-to-back** — `` `${prefix}-${randInt(1, 999)}${pick(SUFFIXES)}` `` interpolates twice with no separator between the last two, which is what makes `GAIA-418A` read as one token instead of `GAIA-418 A`.

**Design note:** first two colors picked for `asteroid`/`comet` (`#D177C1` / `#A777D1`) were too close in hue to tell apart at node scale — nudged to `#C77FA0` (warm rose) / `#7C8CE0` (cool indigo) so they still read as the same "debris" family but are distinguishable at a glance. Worth eyeballing new colors against the existing palette before committing, since color is doing a lot of the game's signaling work later.
