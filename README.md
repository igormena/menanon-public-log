# menanon-public-log

Public audit trail of institutional accumulation signals detected by Menanon.

**Git commit timestamp = immutable proof of first detection.**

## What this is

Menanon (https://menanon.com) detects shifts in institutional conviction by tracking SEC 13F filings across a curated set of 12 funds (Berkshire, Pershing, Lone Pine, Viking, Citadel, Renaissance, Two Sigma, ARK, Coatue, Bridgewater, BlackRock, State Street).

This repository is the **public, append-only audit log** of every accumulation signal we detect. Each day, a snapshot is committed. The git commit timestamp serves as cryptographic proof of when we first identified each signal — before it was reported in financial media.

## Structure

- `current.json` — Latest full snapshot (overwritten each run)
- `daily/YYYY-MM-DD.json` — Daily diff of signals (append-only)
- `quarterly/YYYYQN.json` — Closed quarter snapshots

## Schema

Each signal contains:

- `ticker` — equity ticker
- `filing_quarter` — quarter the 13F data refers to
- `magnitude` — weighted log-scale measure of conviction change
- `breadth` — count of funds in net inflow
- `direction` — +1 accumulation, -1 distribution, 0 neutral
- `confirmed` — true if 2+ funds converge or 1+ concentrated/activist fund initiates
- `disagreement` — true if both inflow and outflow funds are meaningful
- `persistence_quarters` — consecutive quarters with same direction
- `inflow_funds_count` / `outflow_funds_count`
- `net_flow_usd`
- Category breakdown: `concentrated_inflow`, `quant_inflow`, `passive_inflow` (and outflow equivalents)

## Methodology

Signals derive from quarterly 13F-HR filings. Magnitude is computed as a category-weighted sum of position changes using log-scale to prevent outlier funds from dominating. Quant funds with isolated single-fund moves are penalized to reduce noise.

Full product context: https://menanon.com

## Update frequency

This log is regenerated daily at 03:00 UTC after the SEC 13F collector run completes.

## License

MIT — see LICENSE.
