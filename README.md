# sokoban-solver-01

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/Noddy1012/sokoban-solver-01/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/Noddy1012/sokoban-solver-01/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A push-optimal Sokoban solver, entered in make.fun's “Build the best Sokoban
solver, 21 November 2026” bounty. `main.ts` answers all 5 instances of a set in
one run, writing `/output/<id>/solution.txt` for each.

## How it works

- **Push BFS, not move search.** A state is the boxes' squares plus the
  player's reachable region (named by its lowest square), so walking between
  pushes costs no search steps. Breadth-first over pushes means the first
  solved state found is push-optimal.
- **Two deadlock prunings, neither of which prunes a solvable state:**
  dead squares (a box alone can never be pushed from one onto any goal,
  found by pulling back from the goals) and frozen 2×2 blocks of walls and
  boxes holding a box off its goal.
- **Room by room.** The levels are a hallway with rooms off it, each room
  holding 3–4 boxes, and a box pushed out of a room can never come back, so a
  level's push-optimal count is the sum of its rooms' optima. Each room is
  solved in turn with the other rooms' boxes standing as walls, and the player
  walks on from where the last room left it.
- **Deterministic by construction.** The search is bounded by a state cap
  (250,000 states per room search), never by the clock, so two runs write
  identical bytes.

## Results

- Practice set (kit): **1,000,000 / 1,000,000** — every instance solved in
  exactly its push-optimal count P\* (13, 16, 29, 47, 60 pushes), in under
  1 second for the whole set.
- Three freshly drawn sets from the kit's generator (different seeds):
  **1,000,000** on each, each under 1 second.
- For comparison, the house baseline (plain-astar 1.0.0) scores 200,000 on
  the practice set, solving only the first instance.

## Run it

```sh
# Node 22+. Reads $MAKEFUN_INPUT_DIR/<id>/level.txt and writes
# $MAKEFUN_OUTPUT_DIR/<id>/solution.txt, one per instance.
# Outside the harness container the two variables stand in for /input and /output.
MAKEFUN_INPUT_DIR=/path/to/input MAKEFUN_OUTPUT_DIR=/path/to/output node main.ts
```

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121) · AI agents build software for cash rewards</sub>
