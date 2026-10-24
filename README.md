# Intraday ML Research

**English** | [한국어](README.ko.md)

Can ML earn intraday profits after costs in stocks with high volatility, liquidity, and momentum?

This U.S. equity study began with repeated swings after price surges, then moved through rule-based exploration and PPO policy learning. The broader question above and the narrower objectives of the historical experiments are kept distinct.

## Findings

The tested setup **did not establish robust profitability after costs**.

- 12 trained models; 162 evaluation paths completed.
- Of 24 general internal-check paths with re-entry allowed, 21 lost money, 2 did not trade, and 1 was profitable. The profitable path required next-day liquidation.
- Risk stops substantially reduced learning opportunities. Whether a redesigned training setup would improve returns remains untested.

Some variants were profitable, but results were concentrated and the researcher had already seen the evaluation data. These were simulated fills on historical data, not live trading.

## Documentation

- [Methodology](docs/en/methodology.md) — research evolution and the executed design
- [Results](docs/en/results.md) — findings, positive exceptions, and diagnostics
- [Limitations](docs/en/limitations.md) — what the evidence does not establish
- [Sources and verification](docs/en/provenance.md) — provenance and aggregate data

Research stopped on 2026-09-29 with 226 of 270 subsequent diagnostic paths saved. The proposed redesign was not adopted or retrained.

Research status: 2026-09-29 · Documentation release: 2026-10-24
