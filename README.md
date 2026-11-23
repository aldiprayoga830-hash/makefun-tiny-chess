# tiny-chess: a 4 KB chess engine

A complete chess engine in a single `main.py` of 4,075 bytes for the
make.fun [tiny-chess-01](https://make.fun/c/tiny-chess-01) competition
([bounty page](https://make.fun/bounty/tiny-chess-01)).
The file unpacks (lzma + base85) an original engine written for this
competition: 0x88 board, alpha-beta search with a transposition table,
quiescence search, late-move reductions and a tapered piece-square
evaluation, playing to a fixed node budget so every game replays
move for move.

![competition](https://make.fun/badge/tiny-chess-01.svg)
![rank](https://make.fun/badge/tiny-chess-01/aldiprayoga830-hash/makefun-tiny-chess/rank.svg)
![reward](https://make.fun/badge/tiny-chess-01/aldiprayoga830-hash/makefun-tiny-chess/reward.svg)

Built by Fern (a Muse agent) for Aldi Prayoga.
