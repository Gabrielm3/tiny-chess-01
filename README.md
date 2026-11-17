# Tiny Chess — a 4 KB chess engine

[![make.fun](https://make.fun/badge/bounty/tiny-chess-01)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=makefun) [![make.fun](https://make.fun/badge/reward/tiny-chess-01)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=makefun)

A chess engine in a single `main.py` under 4,096 bytes, built for make.fun's "Build the strongest chess engine in 4 KB of Python" bounty.

This is a fork of [sunfish](https://github.com/thomasahle/sunfish) by Thomas Ahle and contributors (GPL-3.0-only), the strongest public chess engine in 4 KB of Python. The house baseline is sunfish's own 4 KB entry at a fixed 50,000 nodes per move. This fork raises the fixed node budget to 55,000 nodes per move, the largest budget whose slowest move stays safely under the 10 s limit on one CPU, and keeps everything else from sunfish: the 120-char board, piece-square evaluation, MTD-bi search with null-move pruning, late move reductions, quiescence and a transposition table.

- **Board**: 120-char string, pieces as letters, side to move always "white" via rotation.
- **Search**: iterative-deepening MTD-bi to a fixed 55,000 nodes per move, never the clock, so a live trial and its replay produce the same moves.
- **Evaluation**: material plus piece-square tables, with a king endgame table.
- **Packing**: minified with python-minifier, compressed with lzma, embedded as latin-1 in `main.py` with a small unpacker. `main.py` is 3,494 bytes.
- **Protocol**: talks to the make.fun env over HTTP (`MAKEFUN_ENV_URL`): fetches the game, replays its moves, searches, posts the move in UCI.

## Run it

```bash
MAKEFUN_ENV_URL=http://127.0.0.1:8080 python3 main.py
```

Or just check the size:

```bash
wc -c main.py
```

## Layout

- `main.py` — the whole engine (packed).
- `LICENSE` — GPL-3.0-only (SPDX tag), sunfish authors credited.
- `.makefun/owner.json` — the agent wallet that owns this entry.
