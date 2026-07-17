# PROJECT_BRIEF.md — Signal Static

Design context carried over from planning sessions. This is the source of truth for WHAT the game is. CLAUDE.md is the source of truth for HOW we work.

## One-liner
Signal Static is an idle game where the player scans a dark galaxy for mysterious signals, then chooses how to claim what they find — by force, science, or diplomacy — and it feels like quiet discovery with creeping stakes.

## Premise & stakes
Earth is dying (an Earth Integrity meter ticks down, visible at all times). The player scans the galactic neighbourhood for signals of potential home worlds — minerals, Earth-like atmospheres. Materials (mined from asteroids) build probes and later ships. Planets must be surveyed and may need terraforming or clearing by force. Making enemies breeds retaliation.

**Win condition:** hold a fully controlled, human-compatible planet for X days.
**After winning:** endless mode unlocks — "Stop this happening again. Send us among the stars." Goal shifts to spreading humanity (colonies founded becomes the endless metric).

## Design pillars
1. **The signal comes first.** Every node begins as a mystery — a waveform, not a target. Curiosity drives; conquest is a response to what you learn.
2. **Three tools, one galaxy.** Military, tech, diplomacy are different answers to the same situations, not separate modes.
3. **Absence is gameplay.** The galaxy moves while you're gone. Returning feels like opening mail, never cleaning up a mess.

## The core gamble (signature mechanic)
Signals arrive with a waveform hint BEFORE the player commits a probe:
- JAGGED = likely hostile
- RHYTHMIC = likely structured/civilization
- FLAT = likely noise/empty
Investing in Signal Decryption reveals more before launch. Chasing a signal you haven't decrypted can mean stumbling into a hostile faction. Decryption is risk management, not a stat stick.

## Core loop
Scan → Travel (probe, idle timer) → Survey → Approach (force / science / diplomacy, different costs+timers+side effects) → Exploit (mine / settle / terraform) → Defend → back to Scan.

Load-bearing rule: **no failure may close all three approaches.** Recovery is always a decision, not just waiting.

## Resources (v1 targets)
- ENERGY (cyan) — universal currency; mining + settled nodes
- ALLOY (amber) — construction; asteroids; builds probes
- INTEL (violet) — surveying/decoding; buys decryption + reveals
- (later) INFLUENCE — diplomacy currency; decays if you use force

## Upgrade trees
1. Probe research: speed, reveal radius, detection range (idle "researching" timers)
2. Signal decryption & analysis: reveals waveform class/detail pre-launch
3. (later) Military wing, terraforming, faction diplomacy/linguistics

## Parked for post-v1 (PARKING_LOT seeds)
Faction management/diplomacy/linguistics ("the art of first contact"), manned flights as distinct system, farming, multi-resource asteroid chains, retaliation sieges on "the new earth," prestige galaxies.

## Visual direction
- Looks like a real screenshot, not concept art. Target: `docs/mockup-landscape.html` (16:9, dark, Share Tech Mono, colour lives in the text)
- Art layer (nebulae, dust, glows) stays at low opacity BEHIND the fog; gameplay layer (blips, tags, dotted lines) stays crisp on top. These never mix.
- Everything procedural: gradients, circles, lines. No sprites.

## Segment ladder (the plan)
1. ✅ Galaxy exists — seeded generation (mulberry32), node data model, SVG render, click-to-inspect. DONE — see index.html. Exercises may still be in progress.
2. First light — fog states enforced (hidden nodes truly hidden), probe entity, travel along a path, reveal radius flips nodes to detected/revealed
3. The tick — game loop (setInterval or requestAnimationFrame), Energy income from mined nodes, first upgrades purchasable
4. The choice — signals spawn with waveform hints; survey reveals node identity; approach buttons (force/science/diplomacy) with different costs, timers, odds
5. The tide — static/interference pressure reclaims undefended frontier nodes; defend + retake
6. It remembers — save/load (localStorage, JSON), offline progress (timestamp math, formula-based not tick-simulated)
7. The dress — restyle to match mockup; Earth Integrity bar; event log; juice pass

Milestone rule: every segment ends with the game runnable and playable.

## Tech decisions (locked)
- Vanilla HTML/CSS/JS, single-page, no build tools
- SVG rendering (inspectable, CSS-animatable, fine at this node count)
- Seeded RNG everywhere (seed 88412 is the dev seed)
- localStorage saves; formula-based offline progress
- Host: GitHub Pages (public repo); later itch.io; Steam later via wrapper (Melvor precedent)
