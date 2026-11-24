# How this engine was built

Written from scratch for tiny-chess-01 in one session, then
compressed into the 4 KB entrypoint.

## Design

- Board: 0x88 array, pieces as signed ints (sign = colour).
- Move generation: pseudo-legal with an attack test on the king
  square; castling, en passant and promotions (q/r/b/n) included.
- Search: negamax alpha-beta, transposition table (depth, flag,
  score), quiescence search with delta pruning, late-move reductions,
  check extensions.
- Evaluation: material plus piece-square tables tapered by remaining
  material; scores are from the side-to-move's perspective, and the
  position hash is recomputed by a full board scan — which is what
  makes the code small enough.
- Determinism: a fixed node budget per move (50,000 — sized to keep
  the trial far inside its 1,800 s wall budget) and no clock reads
  anywhere, so a game replays exactly.

## Protocol

The packed code includes the driver loop: POST `/game`, and while
the status is `your_move`, compute a move and POST it to `/move` as
UCI (castling as the king's move, promotions lowercase). The position
history is kept so the search scores repetitions as draws.

## Packing

The readable source is lzma-compressed (FORMAT_ALONE, tuned lc/lp/pb)
and base85-encoded into the stub in `main.py` (4,076 bytes).

## Verification

- Perft matches the known node counts exactly; snapshot/restore,
  hash and evaluation consistency checked over random self-play games.
- Practice trials against the kit's anchor ladder (6 games each):
  the shipped 50,000-node build scored Elo 1,451 in trial 0 (341 s
  wall), against the house baseline's practice mean of about 1,365;
  an earlier 130,000-node build took up to 1,151 s — too close to
  the trial's wall budget.
