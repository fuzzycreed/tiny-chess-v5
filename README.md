# tiny-chess-v5

![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)
![rank](https://make.fun/badge/tiny-chess-01/fuzzycreed/tiny-chess-v5/rank.svg)
![reward](https://make.fun/badge/tiny-chess-01/fuzzycreed/tiny-chess-v5/reward.svg)

A chess engine in a single 3,205-byte Python file, entered in the
[make.fun tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01) (size cap 4,096 bytes).

## The engine

- **Board:** a Sunfish-style 120-character mailbox string. The side to move is always the
  upper-case side; the board is flipped after every ply. Move legality is exact (an attack test on
  the king after each move), so mate and stalemate are detected properly.
- **Search:** negamax with principal variation search and iterative deepening, stopped on a fixed
  budget of 60,000 nodes per move. There is no clock and no randomness, so the same position
  always produces the same move.
- **Pruning and ordering:** transposition table keyed on (board, en passant) with its move tried
  before move generation, null-move pruning (R = 2 or 3 by depth), reverse futility pruning,
  futility pruning at depth 1-2, internal iterative reduction, late move reductions, captures
  ordered most valuable victim / least valuable attacker, killer and history ordering, a check
  extension, and no pruning while in check. Positions already seen in
  the game score 0.
- **Quiescence:** captures and promotions, with delta pruning at 150.
- **Evaluation:** material plus piece-square tables built from formulas at the root of every
  search and then updated incrementally: a game-phase blend between opening and endgame tables,
  passed-pawn bonuses, king-shield pawns, rooks on open files, the bishop pair, and a mop-up term
  (king distance) when ahead in the endgame.
- **Root:** only moves from the env's `legal_moves` list are played, and never an under-promotion.
- **Packing:** `main.py` is the readable source `src.py` (7,626 bytes) packed with lzma by
  `makefun compress` into 3,205 bytes. Standard library only; it reads no other file.

## Practice scores

Six-game practice trials of `main.py`, graded locally with the kit environment and scorer:

| | trial-0 | trial-1 | trial-2 | mean |
|---|---|---|---|---|
| Elo | 1654 | 2112 | 1809 | **1858** |
| W-D-L | 5-0-1 | 6-0-0 | 5-1-0 | |

Trial-0 was graded twice more and all runs were move-for-move identical. Six-game Elo is coarse
(2112 is the cap for winning every game), so treat these as rough figures.

**Second pass.** After the first version was graded, one ordering refinement was added: among
captures, the cheaper attacker is tried first. The scores above are from this final version. They
equal the first version's (1654 / 2112 / 1809): the win-draw-loss record of every game is the same,
but some games changed length (trial-0 wins got shorter overall, trial-2 wins longer), so the
change is neutral within what 18 games can show.

## Files

- `main.py`: the entry, as graded.
- The readable source `src.py` stays in the repository folder uncommitted; it is uploaded beside
  the entry by `makefun bundle`.

## Licence

MIT, see `LICENSE`.
