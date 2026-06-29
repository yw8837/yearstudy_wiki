---
date: 2026-06-18
tags: [unstack, GroupBy, MultiIndex, pandas, 데이터분석]
aliases: [unstack, 언스택, 피벗펼치기]
---

# unstack (다중 인덱스 펼치기)

**정의**: [[GroupBy]] 다중그룹 결과의 **MultiIndex를 표(피벗) 형태로 펼치는** [[pandas]] 메서드. 안쪽 인덱스 레벨을 **열로 올린다**.

---

## 왜 필요한가

`groupby(['region','type'])` 결과는 인덱스가 2겹(MultiIndex)이라 세로로 길고 조회가 불편하다. unstack으로 한 축을 열로 펼치면 **표처럼 한눈에** 보이고 [[loc]] 조회가 쉬워진다.

---

## 어떻게 작동하나

```python
g = df.groupby(['region','facility_type'])['score'].mean()
g.unstack()      # facility_type을 열로 펼침 → region×type 표
g.unstack().loc['수도권', '체육시설']   # 특정 조합 조회
```

```
MultiIndex (세로로 김)          unstack 후 (표)
region  type                    type    체육  복지
수도권  체육   3.0       →       region
수도권  복지   4.0               수도권  3.0  4.0
영남    체육   2.5               영남    2.5  3.5
```

---

## 자주 헷갈리는 것

**unstack은 인덱스→열, stack은 열→인덱스**(반대 방향). 펼칠 레벨은 `unstack(level=)`로 지정.

---

## 더 알면 좋은 것

📌 `groupby(...).unstack()` ≈ `pivot_table`. 둘 다 그룹 집계를 표로 만든다. [[pivot_table]]은 한 번에, unstack은 groupby 뒤 후처리.

---

## 관련 개념

- [[GroupBy]] — 다중그룹 MultiIndex 생성
- [[pivot_table]] — 한 번에 표 만들기
- [[loc]] — 펼친 표에서 조합 조회
