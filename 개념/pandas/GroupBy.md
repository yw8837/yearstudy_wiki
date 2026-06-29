---
date: 2026-06-18
tags: [GroupBy, SplitApplyCombine, pandas, 집계, 데이터분석]
aliases: [GroupBy, groupby, 그룹바이, 그룹집계]
---

# GroupBy (그룹 집계)

**정의**: [[pandas]]에서 **그룹별로 묶어 집계**하는 핵심 연산. `df.groupby(그룹컬럼)[수치컬럼].집계함수()`. 원리는 [[SplitApplyCombine|Split-Apply-Combine]]. SQL [[GROUP BY]]와 같은 논리.

---

## 왜 필요한가

"지역별 평균", "카테고리별 합계"처럼 **그룹 단위 통계**가 분석의 핵심이다. 6/18 라이브 4-6교시의 중심 주제.

---

## 어떻게 작동하나

```python
df.groupby('location')['log_id'].count()        # 그룹별 개수
df.groupby('issue')['cpu'].mean()               # 그룹별 평균
df.groupby(['region','type'])['score'].mean()   # 다중그룹 → MultiIndex
df.groupby('flag').size()                        # 그룹 크기
```

- **Split**(그룹화)→**Apply**(연산)→**Combine**(결합). 그룹 컬럼은 범주형(성별·요일)·제한된 수치가 적합. 고유값 많은 연속형은 부적절.

---

## 자주 헷갈리는 것

**`df.groupby('x')`만으로는 결과가 안 나온다** — 이건 `DataFrameGroupBy` 객체. 뒤에 집계함수([[agg]]·`mean`·`size`)를 붙여야 [[Series]]/[[DataFrame]]이 된다.

---

## 더 알면 좋은 것

📌 다중키 그룹은 결과가 **MultiIndex** → 조회가 불편하면 [[unstack]]으로 표처럼 펼친다. 그룹평균을 원본에 붙이려면 `as_index=False` + [[merge]].

---

## 관련 개념

- [[SplitApplyCombine]] — GroupBy의 작동 원리
- [[agg]] — 그룹당 복수 집계
- [[unstack]] — MultiIndex 결과 펼치기
- [[GROUP BY]] — SQL의 대응
