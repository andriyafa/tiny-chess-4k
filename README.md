# tiny-chess-01 — entry (v13)

A chess engine in 4,096 bytes of Python, for the make.fun tiny-chess-01
bounty ($500, 20 places, deadline 25 Nov 2026 00:00 WIB).

`main.py` (3,387 bytes) is the whole engine: 0x88 board, own move
generation, negamax alpha-beta with a transposition table, quiescence
search, killer/history move ordering, piece-square eval, and a check
extension (+1 ply when the side to move is in check). Fixed node budget
of 100,000 nodes/move, deterministic (no clock, no randomness). It talks
to the trial env over HTTP JSON (`MAKEFUN_ENV_URL`): `POST /game {}`,
`POST /move {"game": n, "move": "<uci>"}` until `/game` answers
`{"status": "done", ...}`. Python 3.13, standard library only, no model
calls.

Local test results (kit's own server + scorer, practice trial-0):
v13 run17, 14 Nov 2026 — **Elo 1,173** (1 won / 3 drawn / 4 lost, 2.5/8
pts), clearing the 1,154 practice floor by 19. Perft 6/6 pass, mate-in-1
found, think max 5.04 s (limit 10 s/move). Full log in the build
workspace; only this repo is submitted.

## Run locally

```bash
python3 -P -s main.py   # expects MAKEFUN_ENV_URL to be set by the harness
```

## Grade locally (needs Docker + the kit)

From inside the unpacked dev kit:

```bash
node cli/makefun.mjs grade-local --kit . --bot <this repo> --driver docker
```
