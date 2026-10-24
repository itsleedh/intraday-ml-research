# Sources and verification

[README](../../README.md) · [한국어](../provenance.md)

Research status is based on the 2026-09-29 records. The research history was reviewed on 2026-10-04; figures, sources, and publication boundaries for both languages were checked on 2026-10-24. Publication preparation involved no new training, market-data collection, or orders.

## Sources

The [source index CSV](../../results/sources.csv) records the IDs, research-repository-relative paths, and SHA-256 hashes of 30 originals.

- S01–S06: initial questions, design changes, and data collection
- S07–S13 and S23: rule exploration and the first 70 events
- S14–S19: RL design, training, and evaluation completion
- S20–S22: later diagnostics, pause, and unadopted redesign
- C01–C07: CSV/JSON evidence for figures and completion status

Originals are not included in this release. Paths are identifiers, not public links. Hashes identify matching files; they do not establish independent verification or provide access to originals.

## Aggregate data

[internal-check.csv](../../results/internal-check.csv) contains selected aggregates for the 24 general internal-check pairs in C01. Amounts are in USD.

| Field | Meaning |
|---|---|
| model, seed, scenario | Model, training seed, and execution condition |
| full_pnl_usd | Account P&L with re-entry allowed |
| no_reentry_pnl_usd | Account P&L using the same weights with re-entry disabled |
| reentry_account_delta_usd | Allowed P&L minus disabled P&L |

All 24 rows were reconciled to the original. Result tables in both languages, monetary differences, and sign counts were checked. The 162 completed evaluation paths and subsequent 226/270 diagnostic status were also checked against stored results. Training and evaluation were not rerun from raw data.

## Arithmetic check

Run from the repository root using the Python standard library.

```python
import csv
from collections import Counter
from decimal import Decimal as D

with open("results/internal-check.csv", newline="") as f:
    rows = list(csv.DictReader(f))
assert len(rows) == len({(r["model"], r["seed"], r["scenario"]) for r in rows}) == 24
for r in rows:
    assert D(r["full_pnl_usd"]) - D(r["no_reentry_pnl_usd"]) == D(r["reentry_account_delta_usd"])
for field in ("full_pnl_usd", "reentry_account_delta_usd"):
    print(field, Counter("positive" if D(r[field]) > 0 else
                         "negative" if D(r[field]) < 0 else "zero" for r in rows))
```

Expected counts: P&L **positive 1 / negative 21 / zero 2**; differences **positive 1 / negative 17 / zero 6**. The root manifest.json records the size and SHA-256 of each published file.
