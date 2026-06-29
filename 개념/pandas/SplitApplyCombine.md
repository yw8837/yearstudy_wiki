---
date: 2026-06-18
tags: [SplitApplyCombine, GroupBy, pandas, 집계, 데이터분석]
aliases: [SplitApplyCombine, Split-Apply-Combine, 분할적용결합]
---

# Split-Apply-Combine

**정의**: [[GroupBy]]가 작동하는 **3단계 패러다임**. 전체 데이터를 **Split**(그룹으로 분할) → **Apply**(각 그룹에 연산 적용) → **Combine**(결과 결합).

---

## 왜 필요한가

"그룹별 집계"를 머릿속에 그리는 정신 모델이다. 이 흐름을 알면 [[GroupBy]]·[[agg]]·[[pivot_table]] 결과가 왜 그렇게 나오는지 이해된다.

---

## 어떻게 작동하나

```
[전체 데이터]
   │ Split   (지역 A / B / C 로 나눔)
   ▼
[A][B][C]
   │ Apply   (각 그룹에 mean 적용)
   ▼
A:3.0  B:5.0  C:4.0
   │ Combine (하나의 결과로 합침)
   ▼
[지역별 평균 Series]
```

- pandas: `df.groupby('region')['val'].mean()` 한 줄이 이 3단계를 수행.

---

## 자주 헷갈리는 것

**Apply 단계의 결과 형태가 Combine 결과를 결정.** 단일 집계→[[Series]], 복수 집계([[agg]])→[[DataFrame]], 다중 그룹→MultiIndex.

---

## 더 알면 좋은 것

📌 Hadley Wickham이 정리한 개념(R의 plyr). SQL [[GROUP BY]]·[[NumPy]] [[axis]] 집계도 본질은 같은 분할-적용-결합.

---

## 관련 개념

- [[GroupBy]] — 이 패턴을 구현하는 메서드
- [[agg]] — Apply 단계 복수 집계
- [[GROUP BY]] — SQL의 같은 패러다임
