---
date: 2026-06-18
tags: [agg, GroupBy, pandas, 집계, 데이터분석]
aliases: [agg, aggregate, 복수집계]
---

# agg (복수 집계)

**정의**: [[GroupBy]] 결과에 **여러 집계함수를 동시에** 적용하는 메서드. `df.groupby(키)[값].agg(['mean','max'])`.

---

## 왜 필요한가

그룹별로 평균과 최댓값을 한 번에 보고 싶을 때, 집계함수를 따로 호출하지 않고 한 줄로 묶는다.

---

## 어떻게 작동하나

```python
df.groupby('check_type')['fix_hours'].agg(['mean','max'])
# →            mean    max
#   check_type
#   월간      2.98   11.56
#   일간      3.02   11.79
```

- 함수 이름 리스트(`['mean','max']`)를 주면 **각 함수가 한 열**이 된다.

---

## 자주 헷갈리는 것

**결과가 복수 컬럼이면 [[DataFrame]]**(단일 집계는 보통 [[Series]]). 강사 재강조 포인트 — "agg로 복수 집계하면 DataFrame이 된다".

---

## 더 알면 좋은 것

📌 컬럼별로 다른 집계도 가능: `df.groupby('g').agg({'a':'mean','b':'sum'})`. 이름 붙이기는 `agg(평균=('a','mean'))`(named aggregation).

---

## 관련 개념

- [[GroupBy]] — agg가 붙는 대상
- [[unstack]] — 다중그룹 agg 결과 펼치기
- [[집계함수]] — SQL 집계와 비교
