# Token Radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/Nebulanime/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/Nebulanime/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

A token launch radar for the make.fun token-radar-01 competition: 226 pre-token
protocols ranked by an implied value that is priced off the market itself.

## The model (radar-2)

Each category's multiple is the median market-cap-per-TVL of its own live-token
protocols, taken from the sealed snapshot — a category with fewer than three live
comparables uses the global median (exchanges excluded), and every multiple is
clamped to [0.02, 1.50]. Then:

```
implied = round( tvl_usd x multiple(category)
  x (1 + clamp(change_7d, -50, 100) / 100 x 0.6)
  x (0.7 + min(age_days, 1460) / 1460 x 0.3)
  x (1 + 0.02 x min(chains - 1, 5)) )
```

The board covers all 40 watchlist protocols plus every other rankable pre-token
protocol below the $50M floor. Live-token protocols are set aside with their
market caps — and they are the comparables the ranking is priced off. Every row
carries a seven-step farm plan derived from its own category, chains, TVL and
age, with the capital, the gas and the time each step takes.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

Node 22, standard library only, no network. `main.ts` reads the sealed snapshot,
writes `board.json` and `data.js`, copies `dashboard/` and `screenshots/`, and
writes `dossier.json` with the vendored make.fun library.

## Layout

- `main.ts` — the entry lifecycle: read the snapshot, score the board, write the outputs.
- `lib/model.ts` — the comparable-multiples model, the board builder and the farm plans.
- `dashboard/` — the night dashboard: sortable board, search, category and chain filters, watchlist toggle, live-token view, per-protocol detail with the full model equation and farm plan, and the comparables table.
- `screenshots/` — the board, a protocol's detail view and a filtered view.
- `vendor/makefun-board.ts` — make.fun's dossier library (copied from the kit).

<sub>Entered on [make.fun](https://make.fun/?ref=0zsa1rk7&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
