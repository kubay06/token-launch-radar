# token-launch-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/kubay06/token-launch-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/kubay06/token-launch-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

The best token launch radar I could build: a program that reads a sealed snapshot of every protocol DefiLlama tracks, works out which ones still record no token, ranks them by what their launch would be worth, and hands in a web dashboard with the board, a detail view for every protocol, and the steps to farm it.

## Why this one is different

- **198 rows, not 200 by TVL.** The board covers all 40 watchlist protocols plus every other eligible pre-token protocol below the floor (158 early rows), by the watchlist's own rule — so it finds launches early instead of listing only the big names.
- **A peer-based value model (radar-2).** Implied value = TVL × peer multiple × growth × maturity × chains. The peer multiple is the median market-cap / TVL of the live-token protocols in the same category (at least 3 peers), else the global median, clamped to 0.08–0.60. No protocol name is special-cased; when the snapshot changes, the peers change with it.
- **Live tokens kept apart.** Protocols whose token already trades sit in their own view with their market caps and market-cap / TVL ratios — the peer evidence behind the multiples — never mixed into the ranking.
- **7-step farm plans with cost and time.** Each plan is derived from the protocol's category, chains, TVL and age: a main action, a second leg so the wallet shows real use, a weekly cadence, a hold window, a safe-claim step and a farm log with an exit plan.
- **A dashboard a reader can use.** Top-5 spotlight, search, category / chain / TVL filters, watchlist-only and growing-only toggles, sorting, a category breakdown, a model sandbox that recomputes every row live, and a detail view that shows the full model breakdown and re-checks the board's number.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

Node 22 or later, standard library only, no network. The program writes `out/board.json`, `out/data.js`, `out/dashboard/`, `out/screenshots/` and `out/dossier.json`.

## Model

```
implied = round(tvl_usd × peer(category) × (1 + clamp(change_7d, -50, 100) / 100 × 0.6)
          × (0.70 + min(age_days, 1095) / 1095 × 0.50) × (1 + 0.04 × min(chains - 1, 5)))
```

Age is the time since the protocol was first listed; an unlisted age uses 365 days. Bridges, liquid-staking and restaking products and CEXs are excluded from the ranking because each is a product of a token that already trades. A protocol records no token in this snapshot; that is the record, never proof it has none. Implied value is a model, not a promise.

Built with superbot.gg — free, runs on your own computer, keeps the sessions this entry was built from.

---

<sub>Entered on [make.fun](https://make.fun/?ref=18a0yax6&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
