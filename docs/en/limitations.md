# Limitations

[README](../../README.md) · [한국어](../limitations.md)

## Scope of the evidence

- **Reused development data:** The researcher had seen the first 70 events in earlier rule exploration. Exclusion from training does not make the internal check an unexposed external evaluation.
- **Limited universe:** Current instrument classifications and collection/detection filters were used. Representation of the historical stock universe, or of volatility, liquidity, and momentum as distinct characteristics, was not established.
- **Narrow training scope:** The experiment tested one PPO setup, four encoders, and three seeds. The initial supervised-learning, alternative RL, LLM, walk-forward, and CPCV plans were not all executed on market data.
- **Limited learning opportunities:** Risk stops propagated to later dates. Training completion does not establish convergence, and there is no evidence that changing constraints would make the policies profitable.
- **Dependent outcomes:** Seeds, models, and replay paths on the same dates and candidates are not independent market samples.
- **Post-hoc and incomplete diagnostics:** Causes were examined after seeing results, and 44 of 270 subsequent paths remain incomplete. This is distinct from testing new hypotheses.

## Execution and operation

Historical quotes do not guarantee that actual orders would fill. Historical receipt/correction timing, market impact from the strategy's own orders, and queue position were not fully reconstructed. Order, report, and halt-notification delays and IOC liquidity were assumptions.

Primary/stress changed several conditions together. A smaller loss does not isolate an improvement in one factor; decomposing costs on the same fills also differs from retraining at lower costs.

The only profitable full-policy internal-check path included next-day liquidation. Missing halt-period valuations were not replaced with zero daily P&L or observed drawdown. The 50ms criterion had overruns, and not all operational overhead was measured. No live or paper orders were placed.

## Publication, reproducibility, and rights

The public CSV supports arithmetic checks of the 24 paired differences and sign counts. Raw market data, complete fill logs, weights, and the runtime environment are excluded, so the complete training and execution replay cannot be independently reproduced from this release. Source hashes do not provide access to originals or third-party verification.

No reuse license has yet been assigned to the documents or aggregate results. Provider raw data is not published; publishing aggregates does not itself establish compliance with the provider's redistribution terms.
