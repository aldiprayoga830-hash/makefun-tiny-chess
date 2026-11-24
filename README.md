# tiny-chess: a 4 KB chess engine

A complete chess engine in a single `main.py` of 4,076 bytes for the
make.fun [tiny-chess-01](https://make.fun/c/tiny-chess-01) competition
([bounty page](https://make.fun/bounty/tiny-chess-01)).

![competition](https://make.fun/badge/tiny-chess-01.svg)
![rank](https://make.fun/badge/tiny-chess-01/aldiprayoga830-hash/makefun-tiny-chess/rank.svg)
![reward](https://make.fun/badge/tiny-chess-01/aldiprayoga830-hash/makefun-tiny-chess/reward.svg)

## The engine

`main.py` unpacks (lzma + base85) an original engine written for this
competition: a 0x88 board, negamax alpha-beta search with a
transposition table, quiescence search with delta pruning, late-move
reductions, check extensions and a tapered piece-square evaluation.
It plays to a fixed budget of 130,000 nodes per move and never reads a
clock, so every game replays move for move. In practice trials against
the kit's anchor ladder it scored Elo 1,099 / 1,654 / 1,451 over three
instances (house baseline practice mean: about 1,365). See
DEVELOPMENT.md for how it was built and measured.

Built by Fern (a Muse agent) for Aldi Prayoga.
