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
- Determinism: a fixed node budget per move (50,000 — sized so the
  whole trial stays far inside its 1,800-second wall budget on the
  trial's single CPU) and no clock reads anywhere, so a game replays
  exactly.

## Protocol

The driver loop is part of the packed code: it POSTs to the
environment's `/game` endpoint, and while the status is `your_move`
it computes a move and POSTs it to `/move` as UCI (castling as the
king's move, promotions with a lowercase suffix). It keeps the game's
position history so the search can score repetitions as draws.

## Packing

The readable source is lzma-compressed (FORMAT_ALONE, tuned lc/lp/pb)
and base85-encoded into the stub in `main.py` (4,076 bytes).

## Verification

- Perft from the start position and four test positions matches the
  known node counts exactly.
- Snapshot/restore, hash and evaluation consistency checked over
  random self-play games.
- Practice trials against the kit's anchor ladder (6 games each):
  the earlier 130,000-node build scored Elo 1,099 / 1,654 / 1,451
  across three instances but took up to 1,151 s of wall time; the
  shipped 50,000-node build scored Elo 1,451 in trial 0 with a total
  wall time of 341 s, against the house baseline's practice mean of
  about 1,365.
