# WORKBOOK.md — Signal Static (v2, current)

The living development plan. PROJECT_BRIEF.md holds the game design; this holds the process — phases, worksheets, and exit gates. Items marked ✏️ still need the developer's answer. Claude Code: consult this when scope or process questions arise; the parking lot rule and milestone gates here are binding.

---

## Phase 0 — Vision & Pillars ✅ (mostly done)

**One-liner:** Signal Static is an idle game where the player scans a dark galaxy for signals of a new home for dying Earth, choosing to claim what they find by force, science, or diplomacy — and it feels like quiet discovery with creeping stakes.

**Pillars:**
1. The signal comes first — curiosity drives, conquest responds.
2. Three tools, one galaxy — military/tech/diplomacy are answers, not modes.
3. Absence is gameplay — returning feels like opening mail, never cleanup.

**Anti-vision:** not a combat game (battles = timers and odds), not a 4X (no unit micro), not a horror game.

**Structure:** finite winnable campaign (find and hold New Earth for X days) → victory moment (pause, stats, colony ship lands) → optional endless mode: "Stop this happening again. Send us among the stars." Endless metric: colonies founded.

✏️ **Still open:** personal goals ranking (decided informally: learn JavaScript for career + finish something — both rank high, which is why the build-fast/annotate-heavy/exercise-ritual workflow exists). Tone: does any VotV-style unease survive via anomalies? Does the player ever learn what the static/interference is?

---

## Phase 1 — Research ✅ (ongoing, low priority)
Deconstruction targets: Melvor Idle, Spaceplan, Kittens Game, VotV (let's play), Helldivers 2 war map, one Civ run with a single victory type. Keep notes on: what pulls you back after closing, and where boredom hits.

**Differentiation:** "The only idler where *how* you expand matters as much as how fast — every node is a small mystery with three solutions."

---

## Phase 2 — Design ✅ (captured in PROJECT_BRIEF.md)
Load-bearing rules already locked:
- No failure may close all three approaches.
- Every resource needs a source, a sink, and a brake.
- Waveform hint (JAGGED/RHYTHMIC/FLAT) arrives BEFORE probe commitment — the signature gamble.

---

## Phase 3 — Scope (current law)

**v0.1 MVP (the segment ladder, segments 1–6):** small galaxy (~15 nodes), fog, probe travel, waveform hints, survey (3 node types: deposit/civilization/hazard), minimum-viable approaches (2–3 buttons per node with different costs/timers/side-effects; Force fast+expensive+raises pressure, Diplomacy slow+cheap+civs only, Science medium+grants Intel), Energy + Alloy + Intel, static/interference tide, mining + ~5 upgrades, save/load + offline progress, Earth Integrity countdown, win condition.

**v1.0 (post-MVP):** doctrine tracks earned by behavior (using Force levels Military), Influence currency with decay-on-force tension ✏️ (keep/soften/cut — undecided), node traits & anomalies, signal decoding spend, terraforming timers, event log flavor, endless mode.

**Parked (PARKING_LOT.md):** faction diplomacy/linguistics, manned flights, farming, multi-resource chains, retaliation sieges, prestige galaxies, rival memory.

**Definition of Done:** a feature is done when it works with placeholder visuals, survives save/load, is understandable to a stranger, and has been used across one full offline/return cycle.

---

## Phase 4 — Pre-production ⚠️ (one debt outstanding)
Tech decisions locked: vanilla JS, SVG, seeded RNG, localStorage, formula-based offline progress, GitHub Pages → itch.io → Steam via wrapper.

✏️ **Outstanding debt — the economy spreadsheet.** Before Segment 3 (the tick), build the sheet: rows = game-minutes, columns = Energy/Alloy/income/upgrade costs/static pressure; simulate 8 hours. Answer: first upgrade < 2 min? Any dead 20-min stretch in the first 2 hours? Cost growth ratio vs income (start: cost = base × 1.15^n)? Claude Code may generate the starter sheet; the developer tunes it.

✏️ **Optional but valuable:** the index-card paper test of approach balance (does one approach always dominate?). Can be replaced by testing in-engine at Segment 4 — but test it deliberately either way.

---

## Phase 5 — Production (the segment ladder)
1. ✅ Galaxy exists — generation, data model, render, inspect
2. First light — fog enforced, probe travels, reveal radius
3. The tick — game loop, income, first upgrades (**requires the economy sheet**)
4. The choice ⭐ — signals, waveforms, approach buttons (the fantasy moment — if this isn't fun with placeholder numbers, redesign before continuing)
5. The tide — interference pressure, losing/retaking nodes
6. It remembers — save/load, offline progress
7. The dress — restyle to mockup, Earth Integrity bar, event log, juice

**Rules:** every segment ends runnable. End-of-segment exercise ritual is mandatory (CLAUDE.md). Ideas → PARKING_LOT.md. If a segment slips badly, cut features rather than extend twice.

---

## Phase 6 — Practices (standing)
Git every session · parking lot reviewed only between segments · ugly until segment 7 · LEARNING.md maintained · track rough hours per segment (for estimating game #2).

**Risk register:** (1) JS learning curve stalls momentum → build-fast rule + exercises keep it moving; (2) scope creep → parking lot law; (3) motivation dip → schedule a friend playtest after Segment 4 as a forcing function.

---

## Phase 7 — Playtesting (after Segment 6)
3–5 testers, unaided, developer stays silent. Ask after: "what were you trying to do?", "when did you consider stopping?", "what did you want that you couldn't have?", "would you open it tomorrow?"

**THE idler test:** tester closes the game, returns next day. Did they? What did returning feel like? This single test outranks all other feedback.

Fix top 3 confusion points, retest, then enter Phase 8.

---

## Phase 8 — Polish (timebox: 2–3 weeks, features frozen)
Juice shortlist: fog reveal fade + soft ping · node production pulses · smooth number ticking · probe trail · glow on owned nodes · event log typewriter · interference crackle at frontier · 2–3 UI sounds (freesound.org) · colorblind-safe owner colors (critical — color is the language).
Art rule: atmosphere layer (nebulae/dust/glow) stays low-opacity BEHIND the fog; gameplay glyphs stay crisp on top. Never mix.
First-5-minutes script: dark screen → Sol alone → Earth Integrity ticking → first signal → "LAUNCH PROBE" is the only button → first light. Zero explanation needed.

---

## Phase 9 — Release
itch.io page: one-liner, 3–5 screenshots, 30–60s GIF of the fog-reveal opening. WebGL/browser build tested. Free. Post to r/incremental_games (read rules first) + communities you already inhabit. Feedback channel open. Known-issues list published.
✏️ Success criterion (write BEFORE launch): ______________________

---

## Phase 10 — Retrospective (2 weeks post-launch)
What took 3× the estimate? Which phase saved you / felt useless? Three changes for game #2. Review PARKING_LOT.md — circle only what players actually asked for. Decide: v1.1 or next game.

---

## Cheat sheet
1. The vision decides feature arguments, not the mood of the day.
2. Every segment ends playable.
3. New ideas go to the parking lot.
4. Ugly until segment 7.
5. Behind schedule → cut scope, never extend twice.
