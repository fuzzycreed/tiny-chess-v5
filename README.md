# tiny-chess-v5

![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)
![rank](https://make.fun/badge/tiny-chess-01/fuzzycreed/tiny-chess-v5/rank.svg)
![reward](https://make.fun/badge/tiny-chess-01/fuzzycreed/tiny-chess-v5/reward.svg)

A chess engine in a single 2,385-byte Python file, entered in the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01) (size cap 4,096 bytes).

## The engine

- **Search:** negamax with principal variation search (PVS) and iterative deepening, stopped on a
  fixed budget of 60,000 nodes per move. There is no clock and no randomness, so the same position
  always produces the same move.
- **Quiescence:** captures are searched past the nominal depth so the evaluation is only taken in
  quiet positions.
- **Transposition table:** positions already searched are reused for cutoffs and move ordering.
- **Evaluation:** material plus piece-square values computed from formulas (centralisation, pawn
  advancement, passed pawns, open files, king safety, game phase) instead of stored tables.
- **Packing:** `main.py` is the readable source `src.py` (4,505 bytes) packed with lzma by
  `makefun compress` into 2,385 bytes. Standard library only (`json`, `os`, `urllib.request`,
  `lzma`); it reads no other file.

## Practice scores

Six-game practice trials, graded locally with the kit environment and scorer:

| | trial-0 | trial-1 | trial-2 | mean |
|---|---|---|---|---|
| tiny-chess-v5 | 1654 | 1451 | 1654 | **1586** |

Six-game Elo is coarse, so treat these as rough figures.

## Files

- `main.py`: the entry, as graded.
- The readable source `src.py` is over the 4,096-byte per-file cap, so it is not committed here; it
  is uploaded beside the entry by `makefun bundle`.

## Licence

MIT, see `LICENSE`.
