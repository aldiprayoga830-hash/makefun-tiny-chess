# How this engine was built

The engine was written from scratch for tiny-chess-01 in one long
working session, then compressed into the 4 KB entrypoint.

## Design

- Board: 0x88 array, pieces as signed ints (sign = colour).
- Move generation: pseudo-legal with an attack test on the king
  square; castling, en passant and promotions (q/r/b/n) included.
- Search: negamax alpha-beta with a transposition table (depth, flag,
  score), quiescence search over captures and evasions with delta
  pruning, late-move reductions, and check extensions.
- Evaluation: material plus piece-square tables tapered between
  middlegame and endgame by remaining material; scores are kept from
  the side-to-move's perspective and the position hash is recomputed
  by a full board scan, which is what makes the code small enough.
- Determinism: a fixed node budget per move (130,000) and no clock
  reads anywhere, so a game replays exactly.

## Packing

The readable source is compressed with lzma (FORMAT_ALONE, tuned
lc/lp/pb) and base85-encoded into the stub in `main.py`, which is
4,076 bytes as committed — under the 4,096-byte cap. The whole
repository snapshot stays under the 8,192-byte cap.

## Verification

- Perft from the start position and four test positions matches the
  known node counts exactly.
- Snapshot/restore, hash and evaluation consistency checked over
  random self-play games.
- Practice trials against the kit's anchor ladder (6 games each):
  Elo 1,099 / 1,654 / 1,451 across three instances, against the house
  baseline's practice mean of about 1,365.
