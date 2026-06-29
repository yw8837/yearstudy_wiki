---
date: 2026-06-18
tags: [pivot_table, pandas, 집계, 데이터분석]
aliases: [pivot_table, 피벗테이블]
---

# pivot_table (피벗 테이블)

**정의**: 엑셀 피벗처럼 **행·열 기준으로 값을 집계해 표로** 만드는 [[pandas]] 메서드. `df.pivot_table(values=, index=, columns=, aggfunc=)`.

---

## 왜 필요한가

"카테고리(행) × 월(열)별 평균"처럼 2축 교차 집계를 한 번에 표로 만든다. [[GroupBy]]+[[unstack]]을 한 호출로 줄인 것.

---

## 어떻게 작동하나

```python
df.pivot_table(values='avg_daily_usage', index='category', aggfunc='mean')
df.pivot_table(values='v', index='region', columns='type', aggfunc='mean')
```

- `index`=행 기준, `columns`=열 기준, `values`=집계 대상, `aggfunc`=집계함수(기본 mean).

---

## 자주 헷갈리는 것

**`pivot_table`(집계 O, 중복 허용) vs `pivot`(집계 X, 중복 시 에러).** 같은 (행,열)에 값이 여러 개면 pivot_table로 집계해야 한다.

---

## 더 알면 좋은 것

📌 `df.groupby(['region','type'])['v'].mean().unstack()` ≈ `pivot_table(index='region', columns='type', values='v')`. 결과는 같고 표현만 다르다.

---

## 관련 개념

- [[GroupBy]] · [[unstack]] — 같은 결과의 다른 경로
- [[agg]] — aggfunc로 집계 지정
- [[DataFrame]] — 피벗 대상
