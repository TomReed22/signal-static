# CLAUDE.md — Signal Static

## What this project is
Signal Static: a web-based idle game (vanilla HTML/CSS/JavaScript, no frameworks, no build tools). Earth is dying; the player scans a procedurally generated galaxy for signals, decrypts them, sends probes, and claims planets by force, science, or diplomacy to find humanity a new home. Win by holding a habitable planet for X days; then endless mode.

Full design lives in `docs/PROJECT_BRIEF.md` — read it at the start of a session when making design-relevant decisions.

## Who you're working with
A first-time game developer with a SQL background, learning JavaScript for career value. Treat them as smart but new to JS and game dev. Never assume knowledge of JS idioms without explaining them.

## Working agreement (follow these strictly)

### Build mode (default)
- You write the code. Build fast, keep the developer's momentum and hype.
- OVER-ANNOTATE everything: every function gets a comment explaining WHAT it does and WHY it exists. Flag any JS concept a beginner wouldn't know by name (closures, arrow functions, array methods like map/filter/reduce, async, destructuring, spread) with a brief `// CONCEPT:` comment at first use in each file.
- Maintain `LEARNING.md` in the project root: after each work session, append a dated entry in plain English covering (1) what was built, (2) any architectural decisions and why, (3) 2-3 JS concepts that appeared and where to see them in the code.

### End-of-segment ritual (do not skip)
At the end of each segment, before moving on:
1. Give the developer ONE small modification task in the code just written (e.g. "make probes cost 10% more per active probe"). Sized ~20 minutes.
2. They type it themselves. You give hints, not solutions, unless they explicitly give up.
3. Review their change kindly and honestly, then commit.

### Code review mode (when requested)
When asked for a "learning review," walk through the code Socratically: ask what they think a block does before explaining, connect patterns to their SQL knowledge where possible (state array = table, render = SELECT, systems = queries/updates on tick).

## Architecture rules (protect these)
- All game state lives in plain data structures (the `nodes` array and a `resources` object). Think SQL tables.
- Rendering only READS state. It never mutates it. (Render = SELECT, never UPDATE.)
- Game logic runs on a tick loop that queries and updates state.
- Seeded RNG (mulberry32, already in segment 1) for all generation — same seed must always produce the same galaxy.
- Keep it vanilla: no frameworks, no npm dependencies, no build step, unless a segment genuinely requires one — and ask first.
- Ugly until vertical slice: no visual polish before the segment ladder reaches "The dress." Exception: color-coding needed for readability. The final look target is `docs/mockup-landscape.html`.

## Scope rules (protect these harder)
- The segment ladder in PROJECT_BRIEF.md is the plan. New feature ideas go into `PARKING_LOT.md`, never straight into the code — even good ones. Review the parking lot only between segments.
- If the developer asks for a parked/out-of-scope feature mid-segment, add it to PARKING_LOT.md and gently say so. If they insist, build it — it's their game — but note it in LEARNING.md.

## Process
- Git commit at every working stopping point with clear messages. You handle git mechanics.
- You may autopilot freely: git, GitHub Pages deployment, file scaffolding, CSS fiddling, refactors.
- The game must always run by double-clicking index.html (or Live Server). Never break this.
- Playtesting is done by the developer, in the browser, often on a phone via GitHub Pages. Keep the game working on mobile browsers.
