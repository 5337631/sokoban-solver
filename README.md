# Sokoban Solver

[![make.fun](https://img.shields.io/badge/make.fun-sokoban--solver--01-blue)](https://make.fun)
[![owner](https://img.shields.io/badge/owner-5337631-lightgrey)](https://github.com/5337631/sokoban-solver)
[![referral](https://img.shields.io/badge/referral-jp8xc64w-green)](https://make.fun)

A push-optimal Sokoban solver for make.fun's `sokoban-solver-01` competition.
It runs as `node /bot/main.ts` (Node 22, no dependencies, no network) and
answers a set of 5 levels in one run, writing `/output/<id>/solution.txt`
for each.

## How it works

A level is a hallway with 1–4 rooms off it, and a box pushed out of a room's
door can never be pushed back, so each room is an independent problem and
the level optimum is the sum of the rooms' optima. Each room is solved with
a breadth-first search over *pushes* — a state is the sorted box squares
plus the player's reachable region — so the first solved state found is
push-optimal for that room. Two deadlocks are pruned: dead squares (no
push sequence from them can ever reach a goal, found by pulling back from
the goals) and frozen 2×2 blocks of walls and boxes holding a box off its
goal. The per-room push sequences are stitched together with shortest
walks, and the player walks on from where the last room left it.

Search is budgeted by work (a fixed state cap per room), never by the
clock, so two runs on one set write byte-identical answers.

## Practice set

All 5 practice levels solved push-optimally (13 / 16 / 29 / 47 / 60 pushes),
score 1,000,000 via `grade-local`.

## License

MIT — see [LICENSE](LICENSE).
