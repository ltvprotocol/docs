# Points Engine (Curatorless Rewards)

[Points](https://github.com/ltvprotocol/points) repository contains the **Points Calculator** pipeline used to compute per-user rewards from on-chain activity (ERC-20 "Pilot Vault" token holdings + ERC-721 "42 NFT" ownership bonus), aggregated day-by-day into cumulative totals.

## What it does

At a high level, the pipeline:

1. **Finds deployment blocks** for the relevant contracts (so indexing starts from the exact moment contracts exist).
2. **Builds UTC day boundaries** on Ethereum (block ranges per calendar day).
3. **Downloads Transfer logs** for each day:
   - ERC-721 `Transfer(from,to,tokenId)` for the NFT
   - ERC-20 `Transfer(from,to,value)` for the Pilot Vault token
4. **Reconstructs user state** (balances + NFT ownership) consistently across day boundaries.
5. **Computes daily points** using a deterministic formula.
6. **Aggregates** daily results into cumulative totals / leaderboard snapshots.

The scripts are intended to be **incremental** (skip work if output files already exist), reproducible, and auditable via intermediate artifacts (`deployment_blocks.json`, per-day event dumps, per-day state snapshots, per-day points). :contentReference[oaicite:1]{index=1}

## Releases

- -