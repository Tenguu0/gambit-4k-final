# Gambit-4K Final

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Tenguu0/gambit-4k-final/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Tenguu0/gambit-4k-final/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in 4,096 bytes of Python, written from scratch for the
[make.fun](https://make.fun/bounty/tiny-chess-01) tiny-chess-01 bounty.
It plays the house gauntlet through the env at `MAKEFUN_ENV_URL`.

## Engine

`main.py` (4,070 bytes) is the whole engine: an lzma-compressed,
self-contained program using only the standard library. It implements:

- 0x88 board and fully legal move generation (castling, en passant,
  promotion, check, stalemate, insufficient material, repetition,
  fifty-move)
- Negamax alpha-beta with iterative deepening to a fixed node budget
  of 80,000 nodes per move (deterministic: no clock, no unseeded
  randomness)
- Quiescence search (captures, promotions, check evasions) with
  delta pruning, transposition table, MVV-LVA and killer move ordering
- Evaluation: material + piece-square tables (including an endgame
  king table) + bishop pair

## Grading

Practice trials (6 games each, kit scorer, Python 3.13, local run
23 Nov 2026) against the house gauntlet (depth-2 855, depth-3 1073,
depth-4 1199, nodes-100k 1392, nodes-250k 1497, nodes-500k 1596):

| trial | Elo | W/D/L | spare plies |
|------:|----:|------:|------------:|
| 0 | 1545 | 3/3/0 | 264 |
| 1 | 1364 | 2/3/1 | 244 |
| 2 | 1364 | 3/1/2 | 351 |
| **mean** | **1424** | **8/7/3** | **859** |

The 80,000-node version is the validated one: it was graded on all
three practice trials twice (16 Nov and 23 Nov 2026) with identical
results. A 100,000-node tuning was tried on the same three trials on
23 Nov 2026 and did NOT raise the mean (1451 / 1364 / 1364, mean
1393, vs 1545 / 1364 / 1364, mean 1424), so it was reverted — the
single flipped game was game 4 of trial-0 vs nodes-100k (draw at
80k, loss at 100k). A check-extension variant tried on 16 Nov was
also reverted (trial-0 1451 vs 1545).

House baseline (sunfish 4K) rates 1365 mean on the same practice
trials; the practice floor is 1154.

## License

MIT — see [LICENSE](LICENSE).
