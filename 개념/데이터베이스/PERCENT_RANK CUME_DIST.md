---
date: 2026-06-12
tags: [PERCENT_RANK, CUME_DIST, 비율함수, 백분위, 윈도우함수, SQL, 데이터베이스]
aliases: [PERCENT_RANK, CUME_DIST]
---

# PERCENT_RANK / CUME_DIST

**정의**: 행의 **상대적 백분위 위치**를 0~1 값으로 반환하는 [[비율함수]]. PERCENT_RANK=순위 백분율, CUME_DIST=누적 분포(이하 건의 비율).

---

## 왜 필요한가

"상위 몇 %"를 정수 순위가 아니라 **연속 비율**로 표현해야 할 때(데이터 크기가 달라도 비교 가능). 상위 10% 고객 추출, 백분위 컷오프 등에 쓴다.

---

## 어떻게 작동하나

| 함수 | 계산 의미 | 범위 |
|:---|:---|:---|
| `PERCENT_RANK()` | (순위−1)/(전체−1) — 상대 순위 | `[0,1]`, **최고=0**, 최저=1 |
| `CUME_DIST()` | 현재 행보다 작거나 같은 건의 비율 | `(0,1]` |

```sql
PERCENT_RANK() OVER (ORDER BY SALARY DESC)
ROUND(CUME_DIST() OVER (ORDER BY SALARY DESC), 4)
```

---

## 실제 예시

```sql
-- 고객 백분위 — chinook
SELECT c.CustomerId, ROUND(SUM(i.Total),2) AS TotalPurchase,
       ROUND(PERCENT_RANK() OVER (ORDER BY SUM(i.Total) DESC), 4) AS PctRank,
       ROUND(CUME_DIST()    OVER (ORDER BY SUM(i.Total) DESC), 4) AS CumeDist
FROM customers c JOIN invoices i ON c.CustomerId = i.CustomerId
GROUP BY c.CustomerId ORDER BY TotalPurchase DESC LIMIT 10;
```

---

## 자주 헷갈리는 것

**PERCENT_RANK 최고=0**: 1등이 0이라 직관과 반대. "상위 10%"는 `PERCENT_RANK <= 0.1`.

**경계 차이**: PERCENT_RANK는 0 포함 `[0,1]`, CUME_DIST는 0 제외 `(0,1]`.

---

## 더 알면 좋은 것

📌 구간(등급)으로 나누고 싶으면 [[NTILE]]가 더 직관적.

---

## 관련 개념

- [[비율함수]] — 상위 개념
- [[NTILE]] — 정수 분위 그룹
- [[순위함수]] — 정수 순위
