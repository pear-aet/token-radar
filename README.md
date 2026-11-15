# Token Radar — pre-token protocols ranked by implied value

A token launch radar for make.fun bounty `token-radar-01`. It reads the sealed
DefiLlama snapshot, scores every pre-token protocol with a transparent model,
and writes a ranked board, a static dashboard, and a dossier the judge reads.

## Model (radar-2)

Implied value follows only from the snapshot's own figures:

```
implied = round( tvl_usd x 0.25
  x (1 + clamp(change_7d, -50, 150) / 100 x 0.6)
  x (0.6 + min(age_days, 1460) / 1460 x 0.4)
  x category
  x (1 + 0.04 x min(chains, 5)) )
```

- **category**: fee-earning protocols (DEX, perps, lending, CDP, yield) score 1.3;
  pipes and custodians (bridges, liquid staking, curators) score 0.8; the rest 1.0.
- **age**: time since the protocol was first listed, the best record of how long
  it has run with no token. No protocol name is ever special-cased.

## Board

226 rows: all 40 watchlist protocols plus every rankable pre-token protocol
below the $50M floor (186 early rows). Each row carries an 8-step farm plan
with concrete costs and times, derived from the protocol's category, chains,
TVL and age.

## Dashboard

A static page (no network, no framework): a sortable, filterable board, a
per-protocol detail view with the model's own inputs, a live-token view, and
a model sandbox — move the base multiple or growth weight and the board
re-scores and re-sorts live.

## Run

```
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

Requires Node 22+. The program is deterministic: two runs write identical bytes.
