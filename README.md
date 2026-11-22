# Sokoban solver — round 4 (secret loader)

[![Sokoban solver](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121)
[![Rank](https://make.fun/badge/sokoban-solver-01-run-20261121/Gdumbro/sokoban-solver-01-r4/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121)
[![Reward](https://make.fun/badge/sokoban-solver-01-run-20261121/Gdumbro/sokoban-solver-01-r4/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121)

Push-optimal Sokoban solver for the make.fun bounty
["Build the best Sokoban solver, 21 November 2026"](https://make.fun/bounty/sokoban-solver-01-run-20261121).

## How it works

`main.ts` is a 19-byte loader: it runs the solver from the entry secret `S`,
using make.fun's documented entry-secrets feature (a NAME=value pair pushed
for this competition before submitting; every non-provider name becomes an
environment variable of the entry's runs). The solver itself is not in this
repository — it lives only in that secret, so the entry stays tiny while the
ranking tie-break is entry size.

The solver decomposes each level into independent rooms (a box pushed through
a door into the hallway can never come back, so every box stays in its room).
Rooms are found by articulation: a single cell whose removal splits the floor,
or — for rooms with a two-wide opening — an adjacent pair of cells. Each room
is solved push-optimally with A* over push states, using a push-distance
heuristic: for each goal, a reverse search over (box, player) states gives the
minimum pushes to bring one box onto that goal, and the heuristic is the
minimum-cost assignment of boxes to goals. Rooms that resist optimal search
degrade gracefully to weighted A* and then greedy best-first, so a hard room
yields a valid (suboptimal) answer instead of a timeout. The whole run is
bounded by expansion budgets only — no wall clock, so output is deterministic.

## Run

```sh
node main.ts
```

Reads `/input/<id>/level.txt` (XSB), writes `/output/<id>/solution.txt`
(lowercase `udlr` moves). One run answers the whole set in well under 60 s.

## Files

- `main.ts` — the 19-byte loader (Node 22, no dependencies)
