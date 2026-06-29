---
date: 2026-06-18
tags: [pandas, DataFrame, 데이터가공, 데이터분석]
aliases: [pandas, 판다스, Pandas]
---

# pandas (판다스)

**정의**: [[NumPy]] 위에 만들어진 **표(table) 자료구조** 라이브러리. 열마다 이름·타입이 있는 [[DataFrame]]·[[Series]]로 데이터를 적재·필터·집계·결합한다. "엑셀의 파이썬 버전".

---

## 왜 필요한가

[[NumPy]]는 숫자 배열의 뼈대지만 **열 이름·혼합 타입·결측 처리**가 불편하다. pandas는 행 index와 이름 있는 열을 얹어 **CSV/DB → 가공 → 분석**을 자연스럽게 만든다. [[SQL]]로 추출한 데이터를 받아 분석하는 현실 워크플로의 중심.

---

## 어떻게 작동하나

```python
import pandas as pd
df = pd.read_csv("data.csv")     # CSV → DataFrame
df.shape; df.info(); df.describe()        # 점검
df.loc[df['cpu'] > 40, :]                 # 필터
df.groupby('loc')['val'].mean()           # 집계
```

- 핵심 자료구조 = [[DataFrame]](2차원)·[[Series]](1차원). 내부는 [[NumPy]] [[ndarray]] 기반이라 [[벡터화]]·[[axis]] 개념이 그대로 이어진다.

---

## 자주 헷갈리는 것

**pandas ≠ SQL을 대체.** 가공 논리는 [[SQL]]과 유사하지만, 운영 DB는 SQL로 필요한 데이터만 뽑고 **pandas는 그 뒤 분석·가공**을 맡는 게 현실적 분업.

---

## 더 알면 좋은 것

📌 [[NumPy]]를 먼저 이해해야 pandas가 쉽다. 마스킹([[Boolean인덱싱]])·축 집계([[axis]])·[[벡터화]]는 numpy에서 배운 그대로 pandas에서 반복된다.

---

## 관련 개념

- [[DataFrame]] · [[Series]] — pandas 핵심 자료구조
- [[NumPy]] — pandas의 토대
- [[SQL]] — 같은 가공 논리, 다른 문법
