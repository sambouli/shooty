# SHOOTY — test report

Run: 2026-10-31 00:28 UTC · node v22.22.1

```

== math & helpers ==
  ✓ clamp bounds
  ✓ lerp endpoints & midpoint
  ✓ dist2 pythagorean
  ✓ circleHit touching & gap
  ✓ normAngle wraps
  ✓ approachAngle steps and snaps
  ✓ formatTime
  ✓ mulberry32 deterministic & in [0,1)

== difficulty curve ==
  ✓ waveNumber progression
  ✓ spawnInterval decreases with waves, floor at 0.16
  ✓ speed/hp multipliers bounded
  ✓ maxEnemies capped
  ✓ xpToNext strictly increasing
  ✓ availableTypes unlocks by wave
  ✓ pickEnemyType only returns available types

== player / combat ==
  ✓ newPlayerStats sane defaults
  ✓ firePlayer spawns shots along aim
  ✓ crit never exceeds 3x dmg multiplier sanity
  ✓ bullet kills enemy -> score, kill, xp orb
  ✓ pierce lets bullet hit 2 enemies but not the same twice
  ✓ contact damage respects invuln & dash i-frames
  ✓ death costs a life, last life ends game
  ✓ gainXp levels up and queues pendingLevels
  ✓ step transitions playing->levelup when pending
  ✓ step is a no-op outside playing
  ✓ dash moves player and sets cooldown
  ✓ regen heals up to max only

== pickups ==
  ✓ xp orb magnetizes and is collected
  ✓ hp pickup heals but never overheals
  ✓ nuke pickup clears nearby enemies
  ✓ expired pickups vanish uncollected

== upgrades ==
  ✓ every upgrade applies without throwing & stats stay finite
  ✓ offerUpgrades respects max levels and count
  ✓ upgrade table integrity: unique ids, valid rarities

== save/load (localStorage faked) ==
  ✓ loadSave defaults on empty storage
  ✓ save round-trips through storage
  ✓ corrupt JSON falls back to defaults, no throw
  ✓ partial/garbage save coerces to numbers
  ✓ storage quota failure returns false, no throw

== spawning & world ==
  ✓ enemies spawn off-screen around player over time
  ✓ enemy cap respected under long sim
  ✓ particle pool bounded (<900)

== headless bot survival sim (perf & stability) ==
    sim: score=2423 kills=91 lvl=1 peakE=5 wall=149ms state=playing
  ✓ 90s bot run: survives, scores, levels up, no NaN
  ✓ deterministic: same seed -> identical outcome
  ✓ runaway difficulty eventually kills an idle player

== html integrity ==
  ✓ single file: no external resource refs
  ✓ required DOM ids present
  ✓ balanced braces/parens in script body

———— 48 passed, 0 failed ————
exit code: 0
```
