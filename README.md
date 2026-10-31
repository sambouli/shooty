# SHOOTY — neon swarm survivors

A hyper-polished, single-file arcade twin-stick shooter / rogue-lite survivor.
**One `index.html`. No build step, no dependencies, no external assets** — every
visual is a canvas primitive and every sound is synthesized live with the Web Audio API.

Hold the line against an endless neon swarm, vacuum up XP shards, and pick
roguelite upgrades between waves. How long can you last?

## Play

Open `index.html` in any modern browser — that's it.

```bash
# option A: just open the file
xdg-open index.html          # Linux
open index.html              # macOS

# option B: serve it (needed for some mobile browsers / localStorage quirks)
python3 -m http.server 8091
# then visit http://<your-ip>:8091/
```

To play from your phone, serve it from your machine (`python3 -m http.server 8091`)
and open `http://<machine-ip>:8091/` on the same Wi-Fi.

## Controls

| Action   | Keyboard / Mouse      | Touch               |
|----------|-----------------------|---------------------|
| Move     | `WASD` / arrows       | Left virtual stick  |
| Aim      | Mouse                 | Right virtual stick |
| Fire     | Hold click / `Space`  | Push aim stick out  |
| Dash     | `Shift`               | ⚡ button            |
| Pause    | `P` / `Esc`           | ▮▮ button           |
| Mute     | `M`                   | —                   |

No aim input = auto-aim at the nearest threat. Dash has i-frames — use it.

## What's in the box

- **State machine**: menu → playing ⇄ paused → levelup → gameover, fixed-timestep sim decoupled from rAF rendering
- **5 enemy archetypes** (grunt, runner, tank, shooter, bomber) unlocked by wave, each with distinct AI (kiting shooters, detonating bombers)
- **15 upgrade cards** across common/rare/epic rarities, level-capped, offered 3-at-a-time on level-up
- **Dynamic difficulty**: spawn rate, HP, speed, and enemy cap all scale off elapsed time with hard bounds
- **Juice**: particle system (pooled, capped), screen shake, hit-flash vignette, glow rendering, combo multiplier, off-screen threat arrows
- **Procedural audio**: shoot/hit/boom/hurt/pickup/level-up/dash/nuke/game-over — all Web Audio synthesis, zero files
- **Persistence**: best score, best wave, total runs and kills in `localStorage`, corrupt-save-proof
- **Cross-platform**: pointer-event dual virtual sticks, safe-area insets, DPR-aware canvas, tab-switch guard

## Testing

The pure game logic (`CORE`) lives in a DOM-free IIFE inside `index.html` that
exports via `module.exports` under Node. The test suite extracts the `<script>`
block and runs it headless — the tests exercise the *shipping code*, not a copy.

```bash
node tests/test.js
```

48 tests covering:

- math & helpers (collision, angle wrapping, seeded RNG determinism)
- difficulty curve (monotonicity + hard caps)
- combat (bullet kill/pierce/crit, i-frames, life loss, dash cooldown, regen)
- pickups (magnet physics, heal caps, nuke radius, expiry)
- upgrades (table integrity, max-level gating, stat sanity after full investment)
- save/load (round-trip, corrupt JSON, quota failure — faked localStorage)
- world (off-screen spawn radius, enemy cap, particle pool bound)
- a 90-second headless bot survival simulation (perf, stability, NaN sweep)
- HTML integrity (zero external refs, required DOM ids, balanced delimiters)

Exit code `0` = green. The last run's report is committed at `TEST-REPORT.md`.

## Repo layout

```
index.html      — the entire game (HTML + CSS + JS, ~46 KB)
tests/test.js   — headless test suite (Node, no deps)
TEST-REPORT.md  — output of the latest test run
README.md
```

## Notes

- Works offline; the page never touches the network.
- High-DPI aware (capped at 2× for mobile GPU sanity).
- Particle/bullet/enemy pools are hard-capped so long runs stay at 60 fps.
