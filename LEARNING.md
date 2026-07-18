# LEARNING.md — Signal Static

Plain-English dev log. One dated entry per work session: what got built, why, and 2-3 JS concepts that showed up and where to find them.

---

## 2026-07-18 — Project setup

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

## 2026-07-18 — Segment 1 exercise: new node types + name generator

**What was built:** the Segment 1 ritual exercise, and then some. Added two new node types (`wreckage`, `comet`) to `generateGalaxy()`'s type pool and `COLORS`, then rebuilt `makeName()` from a single naming pattern into a 7-pattern generator (`KEPLER-418`, `HUBBLE 418`, `GAIA-418A`, `NEXUS-IV`, etc.), backed by a much bigger `PREFIXES` list, a `SUFFIXES` string, and a `ROMAN` array.

**Concepts that showed up:**
- **`switch`/`case` with a `default`** — `makeName()` picks one of 7 patterns via `randInt(0, 6)`; cases `0`–`5` are explicit, `default` catches the 7th value. Each `case` `return`s immediately, so there's no fall-through to worry about.
- **Strings index like arrays** — `pick(SUFFIXES)` works even though `SUFFIXES` is a string (`"ABCDEFGHIJKLMNOPQRSTUVWXYZ"`), not an array, because `pick()` just does `arr[Math.floor(rng() * arr.length)]`, and JS strings support both `.length` and bracket-indexing the same way arrays do.
- **Template literals chained back-to-back** — `` `${prefix}-${randInt(1, 999)}${pick(SUFFIXES)}` `` interpolates twice with no separator between the last two, which is what makes `GAIA-418A` read as one token instead of `GAIA-418 A`.

**Design note:** first two colors picked for `asteroid`/`comet` (`#D177C1` / `#A777D1`) were too close in hue to tell apart at node scale — nudged to `#C77FA0` (warm rose) / `#7C8CE0` (cool indigo) so they still read as the same "debris" family but are distinguishable at a glance. Worth eyeballing new colors against the existing palette before committing, since color is doing a lot of the game's signaling work later.

---

## 2026-07-18 — Centre-home, guaranteed spacing, and a zoom/pan camera

**What was built:**
- **Home is now placed, not promoted.** The old code promoted whichever generated node landed *nearest* the centre — which in a sparse galaxy could still be well off-centre. Now `generateGalaxy()` places SOL at the exact centre and uses `.filter()` to clear out any node too close to it first, so it never overlaps a neighbour. (See section 4.)
- **Nodes can no longer overlap.** Added a spacing guarantee: before placing a candidate node, we skip it if any already-placed node is within `MIN_DIST`. This turns "nodes rarely overlap" into "nodes never overlap".
- **Grander galaxy + a camera.** Enlarged the map coordinate space to 3200×1800 so the galaxy feels roomy, and added scroll-to-zoom / drag-to-pan by moving the SVG `viewBox` (section 7).
- You also independently added a `moon` node type and finished **Exercise 3** (distance-from-home in `inspect()`) — both working.

**Concepts that showed up:**
- **`.some()`** (section 4, spacing check) — returns `true` if the test passes for *at least one* item in an array. We use it to ask "is any existing node too close?". Its siblings: `.filter()` (keep matching items → the home-overlap clear-out) and `.find()` (first matching item → your Exercise 3 home lookup). Three array methods, same shape, different questions — all your SQL `WHERE` instincts.
- **The camera / viewBox trick** (section 7) — the galaxy data never moves; we only change *which rectangle of the map fills the screen* (`viewBox`). Zoom = shrink that rectangle, pan = slide it. This keeps the sacred rule intact: rendering (and now the camera) only ever *reads* state, never mutates node data.
- **Events + a drag-vs-click flag** — `addEventListener("wheel"/"mousedown"/"mousemove")` wires functions to user input. A `panMoved` flag distinguishes a real drag from a click, so panning across a node doesn't accidentally "inspect" it.

**Deviation from the plan (on the record):** scroll-to-zoom/pan is a small fork away from the *locked* visual target (`docs/mockup-landscape.html`), which is a fixed 16:9 "screenshot" stage, not a zoomable map. Built it now because the developer wanted it and visuals are placeholder until Segment 7 ("the dress"); when we do the real visual pass we'll need to decide whether the zoomable camera stays or the fixed-stage look wins. Logged per CLAUDE.md's scope rule. Mobile pinch-zoom/drag isn't implemented yet (desktop wheel/drag only) — tap-to-inspect still works on phones. See `PARKING_LOT.md`.

---

## 2026-07-18 — Realistic star systems (hierarchy in the data model)

**What was built:** generation now models real structure instead of a flat scatter. Stars are placed at spaced random positions; each star gets planets in orbit; each planet gets 0–2 moons in tighter orbit. Loose bodies (asteroids/comets/wreckage) still drift at random. SOL is a full home system at the centre with EARTH as its innermost planet. Faint orbit rings are drawn so you can *see* what orbits what. (All in section 4 generation + section 5 render.)

**The one big idea — a foreign key (`parentId`):**
- The `nodes` table gained a `parentId` column: a planet's `parentId` is its star's `id`; a moon's is its planet's `id`; stars and loose bodies have `parentId: null`. This is a **self-referencing foreign key** — the same shape as an `employees` table with a `manager_id` pointing at another employee row. It's what turns a flat list into a hierarchy, and later segments (probe travel, claiming) can walk it.
- Also added a `size` column so bodies render at different radii (stars biggest → moons smallest).

**JS concepts that showed up:**
- **Spread `...body`** (section 4, the `add()` helper) — `{ id: id++, parentId: null, ...body }` copies every field from `body` into a new object. Because `...body` comes *after* `parentId: null`, a body that supplies its own `parentId` overrides the default; one that doesn't stays `null`. Spread = "copy all the fields of that object into this one."
- **`.find()` for the parent lookup** (section 5, ring pass) — `nodes.find(p => p.id === n.parentId)` fetches a body's parent row by id. Same method you used in Exercise 3; here it walks the foreign key. (SQL: a self-join on `parentId = id`.)
- **A tiny bit of trig for orbits** — `x = star.x + Math.cos(angle) * orbit`, `y = star.y + Math.sin(angle) * orbit` places a body at a random `angle` and a set `orbit` distance around its parent. cos handles the horizontal part, sin the vertical; together they walk a circle. You don't need to derive it — just know "cos/sin + a radius = a point on a circle."

**Design note (logged, we chose to build it):** this is a meaningful data-model change that will ripple into Segment 2 (probe travel) and Segment 4 (the choice) — we'll have to decide whether the player scans/claims an individual body or a whole star system. Building the placement now keeps that decision open; it actually strengthens the "a star is the distant signal; surveying reveals its planets" fantasy. Possible future exercise: extend `inspect()` to also show `orbits: <parent name>` using the same `.find()` on `parentId`.
