# Methodology

[README](../../README.md) · [한국어](../methodology.md)

## Research evolution

This was an exploratory sequence of changes to questions, implementation, and policies using shared development data, rather than a single confirmatory experiment.

| Date | Change |
|---|---|
| September 24 | Started with identifying repeated swings after a surge; later that day developed toward retaining an RL policy and analyzing its profits and failures |
| September 25 | Removed mandatory pullback/rebound confirmation; designed RL to wait, enter, hold, or exit from detection onward |
| September 27 | Surveyed minute bars across 40 development sessions, collected detailed samples, and replayed rules on a few events |
| Earlier September 28 | Examined execution and cost sensitivity, then compared single-entry rules on the first 70 events; excluded repeated-swing rule B-DC |
| Later September 28 | Trained PPO without fixed pullback, take-profit, stop-loss, or 15-minute holding rules; explicitly evaluated re-entry's contribution |
| September 29 | Diagnosed costs, learning opportunities, and latency, then stopped; the new training design was not adopted |

The initial supervised-learning, LLM, and alternative RL candidates, and synthetic implementations, were not all empirically compared on market data. The current broad ML question overlaps with the historical repeated-trading question, but their scope differs.

## Executed RL setup

The candidate filter required a price of at least $2, a gain of +5% over 5 minutes or +8% over 15 minutes, and a dollar-volume proxy of at least $1 million in that window. The proxy was `Σ(low × volume)`; it does not establish actual traded dollar volume or sufficient liquidity.

| Component | Executed setting |
|---|---|
| Capital and slots | $10,000 per block; up to $1,000 per new-entry order; 1 trading slot |
| Actions | Every 5 seconds: wait, buy a candidate, or sell the held stock; no shorting, leverage, or adding to positions |
| Recording | 3 candidate slots for general samples, 1 for halt diagnostics; recording continues after exit |
| Input | 120 frames covering the previous 10 minutes, plus account and order state |
| Models | MLP, RNN, GRU, LSTM × seeds 11, 23, 47 |
| Training | Masked PPO; 2 passes through a 14-trading-day block |
| Reward | `10,000 × change in net asset value / initial capital`; costs included, no trade-count bonus |
| Risk | 5% account drawdown, 10% position loss; forced liquidation starts 5 minutes before close |
| Order limits | At most 20 buy submissions per candidate; 5-second wait after the exit report before re-entry |

Risk stops persisted to the block's end, including subsequent dates. Partial fills, unfilled IOC orders, and halts meant that loss limits and same-day liquidation were not guaranteed. Remaining positions continued on the same ledger; missing carry data left a path incomplete.

All models received the same window, and recurrent memory reset at each decision. This was not a comparison of PPO policies with continuously persistent recurrent memory. PPO settings were γ=1, GAE λ=.99, rollout 1,024, minibatch 64, 2 epochs, learning rate .0003, and clip .2.

## Data split and evaluation

The researcher had already seen the first 70 events during rule exploration. Normalization used only general training inputs. All dates below are in 2026.

| Role | Candidates | Account period | Trading days |
|---|---:|---|---:|
| Training | 36 | 07-30~08-18 | 14 |
| Development comparison | 12 | 08-19~08-25 | 5 |
| Internal check, excluded from training | 12 | 08-26~09-01 | 5 |
| Separate halt diagnostic | 10 | 07-30~09-01 | 24 |

Evaluation comprised 72 paths from 12 models × 3 evaluation blocks × 2 execution conditions, 72 paths using the same weights with re-entry disabled, and 18 baseline paths: 162 in total. No best seed or checkpoint was selected.

Assumed order-arrival delays were 250ms/2,000ms for primary/stress; costs, displayed liquidity, and report delays also changed. Each comparison policy replayed its own account. The 50ms decision criterion was assessed separately from economic completion.

## Status of later changes

Latency handling was improved for evaluating existing models. Daily training episodes, reward rescaling, and proposals to reduce buy submissions from 20 to 2 and increase the wait from 5 to 60 seconds were not adopted or used in new training. Observed loss of learning opportunities is distinct from evidence that changing constraints improves profitability.

Evidence: S01–S06, S11–S16, S20–S22 in [Sources](provenance.md).
