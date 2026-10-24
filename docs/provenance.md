# 출처와 검증

[README](../README.ko.md) · [English](en/provenance.md)

연구 상태는 2026-09-29 기록 기준입니다. 2026-10-04 전체 흐름을 검토했고, 2026-10-24 한국어·영어 공개본의 수치·출처·파일 경계를 대조했습니다. 공개 준비 중 새 학습·시장자료 수집·주문은 없었습니다.

## 출처

[출처 목록 CSV](../results/sources.csv)에 원본 30개의 ID·연구 저장소 상대 경로·SHA-256을 담았습니다.

- S01–S06: 초기 질문, 설계 변경, 자료 확보
- S07–S13 및 S23: 규칙 탐색과 첫 70건
- S14–S19: RL 설계, 학습, 평가 보완
- S20–S22: 후반 진단, 중단, 미채택 수정안
- C01–C07: 수치·완료 상태를 대조한 CSV·JSON

원본은 공개 묶음에 포함하지 않았습니다. 경로는 식별자이며 공개 링크가 아닙니다. 해시는 같은 파일인지 확인하는 값으로, 독립 검증이나 원본 접근을 보장하지 않습니다.

## 집계 자료

[internal-check.csv](../results/internal-check.csv)는 C01의 일반 내부 확인 24쌍에서 선택한 집계입니다. 금액은 USD입니다.

| 필드 | 의미 |
|---|---|
| model, seed, scenario | 모델·학습 seed·실행 조건 |
| full_pnl_usd | 재진입 허용 계좌 손익 |
| no_reentry_pnl_usd | 같은 가중치의 재진입 금지 계좌 손익 |
| reentry_account_delta_usd | 허용 손익 − 금지 손익 |

24행을 원본과 대조하고 두 언어의 결과 표·금액 차이·부호 개수를 검산했습니다. 162경로 완료와 후속 226/270 상태도 저장된 결과로 확인했습니다. 원시 자료부터 학습·평가를 다시 실행한 검증은 아닙니다.

## 검산

저장소 루트에서 Python 표준 라이브러리로 실행합니다.

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

기대 개수는 손익에서 **positive 1 / negative 21 / zero 2**, 차이에서 **positive 1 / negative 17 / zero 6**입니다. 공개 파일의 크기와 SHA-256은 저장소 루트의 manifest.json에 기록합니다.
