---
date: 2026-06-18
tags: [value_counts, pandas, 집계, 데이터분석]
aliases: [value_counts, 값빈도]
---

# value_counts (값 빈도)

**정의**: [[Series]]의 **각 값이 몇 번 나오는지** 빈도를 내림차순으로 세는 [[pandas]] 메서드. `df['col'].value_counts()`.

---

## 왜 필요한가

범주형 변수의 분포(어떤 지역/카테고리가 많은가)를 빠르게 본다. **최빈값**을 뽑아 필터 기준으로 쓰기도 한다.

---

## 어떻게 작동하나

```python
df['region'].value_counts()              # 값별 개수(내림차순)
top = df['region'].value_counts().index[0]   # .index[0] = 최빈값
df.loc[df['region'] == top, :]               # 최빈값 기반 필터
```

- 결과는 값을 index로, 빈도를 value로 갖는 [[Series]]. `.index[0]`이 가장 많은 값.

---

## 자주 헷갈리는 것

**기본은 결측(NaN) 제외.** 결측 개수까지 보려면 `dropna=False`. 비율로 보려면 `normalize=True`.

---

## 더 알면 좋은 것

📌 [[GroupBy]] `count`와 비슷하지만 value_counts는 **한 열의 값 분포** 전용. 그룹별 다른 열 집계는 groupby.

---

## 관련 개념

- [[Series]] — value_counts 결과 형태
- [[GroupBy]] — 그룹별 집계(다른 열)
- [[Boolean인덱싱]] — 최빈값 기반 필터
