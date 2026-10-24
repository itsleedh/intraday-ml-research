# Results

[README](../../README.md) · [한국어](../results.md)

All amounts are account P&L in USD under simulated execution and costs. Each account started separately with $10,000. Results across samples, models, and conditions must not be combined into one investment track record.

## Rule exploration

The first 3-event study lost money; some single-entry rules were profitable in the next sample of 6 general and 3 halt events. However, repeated-swing rule B-DC lost money in all 40 general and 40 halt conditions in a subsequent study. Even with fees and additional slippage set to zero, it returned −$79.66 / −$188.05; spread costs and other effects remained.

The first 70-event exploration excluded B-DC. Results for its 60 general events are below. B-C entered once when ready with fixed exits; B-PB entered once after a pullback/rebound; B-HOLD entered once and followed common risk and closing rules.

| Rule | primary | stress |
|---|---:|---:|
| B-C | -125.62 | -157.94 |
| B-PB | -54.94 | -103.56 |
| B-HOLD | -367.64 | -48.71 |

B-PB lost less than B-C but remained unprofitable. On the separate 10 halt events, B-HOLD stress earned +$153.59, with +$307.87 contributed by one APMD event. The evidence did not show persistent profitability beyond small samples. This stage did not evaluate RL or repeated re-entry.

Evidence: S07–S13, S23, C04.

## RL internal check

The 12 models were evaluated on 12 general candidates over 5 trading days. The table shows policies allowing re-entry (full). Primary/stress denotes execution conditions; the numbers denote seeds.

| Model | primary / 11 | primary / 23 | primary / 47 | stress / 11 | stress / 23 | stress / 47 |
|---|---:|---:|---:|---:|---:|---:|
| MLP | -516.26 | -500.72 | -516.34 | -391.78 | -183.04 | -230.93 |
| RNN | -15.13 | -515.58 | -201.72 | +64.66 | -470.34 | -131.24 |
| GRU | -502.79 | -39.72 | -152.93 | -476.37 | -9.87 | -68.37 |
| LSTM | -510.70 | 0.00 | -501.87 | -298.18 | 0.00 | -103.11 |

Of the 24 paths, 1 was profitable, 21 lost money, and 2 did not trade. On the same block, no-trade B0 returned $0, B-C returned −$80.64 / −$21.33, and B-HOLD returned −$70.08 / −$25.77 under primary/stress.

Against the same weights with re-entry disabled, there was 1 improvement, 17 deteriorations, and 6 ties. Disabling re-entry also changes cash, slots, and opportunities in other candidates, so the difference is not a pure causal effect of individual trades. [All 24 paired comparisons (CSV)](../../results/internal-check.csv)

### Positive cases

- **RNN-11 stress full: +$64.66.** It liquidated 25 PSQL shares on the next trading day. This does not meet the same-day liquidation objective. Missing closing valuations during the halt leave August 28/31 daily P&L and drawdown across the gap unknown.
- **RNN-11 primary, re-entry disabled: +$5.39.** This used the same general internal-check block.
- **RNN-47 stress, re-entry disabled: +$315.58.** This was a separate halt diagnostic; 97.1% of positive contributions came from holding APMD for about 3 hours 55 minutes. The same model's full policy returned −$16.65.

These observations are retained, but do not establish an ML advantage across models, seeds, and periods. Evidence: S17–S19, C01–C03.

## Costs and learning opportunities

The following are descriptive sums across the 12 full-policy internal-check accounts. They decompose the same fills, rather than show retraining or replay with costs removed.

| Component | primary | stress |
|---|---:|---:|
| Midpoint-based directional P&L | -268.275 | -76.665 |
| Spread cost | 2,192.295 | 1,334.710 |
| Net P&L after all costs | -3,973.76 | -2,298.57 |

Spreads were costly, but directional P&L was also negative. The evidence does not establish that costs concealed an effective predictive signal.

Each training run processed 131,040 grid points but yielded only 261–903 policy samples and 10–30 optimizer updates. During the second pass, risk stops removed most opportunities to act after the first 1–2 days. Weights and optimizer state changed in every run, so learning did occur. Adequate training, or improved returns after changing constraints, was not established.

Evidence: S16, S20–S22.

## Completion and pause

After addressing latency interruptions and unresolved positions, final P&L was established for 162/162 main evaluation paths. The 50ms decision criterion remained FAIL because of recorded overruns.

Subsequent diagnostics stopped on 2026-09-29 after 226/270 paths were saved. The 44 incomplete paths comprise 17 fee-halving paths and 27 zero-slippage paths; they were not filled with zero P&L. The 54 paths using improved latency handling had the same final P&L as their original counterparts and still did not establish a real-time latency guarantee.

Evaluation-code changes are distinct from training a new policy. There was no adopted redesign, retraining, or evaluation on previously unexposed data. Evidence: S19–S22, C05–C07. See [Sources](provenance.md) for identifiers.
