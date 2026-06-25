---
date: 2026-06-12
tags: [RANK, DENSE_RANK, ROW_NUMBER, 순위함수, 윈도우함수, SQL, 데이터베이스]
aliases: [RANK, DENSE_RANK, ROW_NUMBER]
---

# RANK / DENSE_RANK / ROW_NUMBER

**정의**: 세 가지 [[순위함수]]. 정렬 기준으로 순위를 매기되 **동률 처리 방식만 다르다.** RANK=건너뜀, DENSE_RANK=안 건너뜀, ROW_NUMBER=고유번호.

---

## 왜 필요한가

같은 데이터라도 "공동 순위를 어떻게 표시할지"가 분석마다 다르다. 동점자 다음을 한 칸 비울지(RANK), 안 비울지(DENSE_RANK), 아예 동점을 무시하고 줄세울지(ROW_NUMBER)를 골라 쓴다.

---

## 어떻게 작동하나

급여 내림차순 예시(ELICE 10000, JAMES 8000, JESSICA 6000, JANNET 6000, STEVE 4000):

| 이름 | 급여 | RANK | DENSE_RANK | ROW_NUMBER |
|:---|---:|:---:|:---:|:---:|
| ELICE | 10000 | 1 | 1 | 1 |
| JAMES | 8000 | 2 | 2 | 2 |
| JESSICA | 6000 | 3 | 3 | 3 |
| JANNET | 6000 | **3** | **3** | **4** |
| STEVE | 4000 | **5** | **4** | 5 |

- RANK: 공동 3등 둘 → 다음은 **5등**(4 건너뜀).
- DENSE_RANK: 공동 3등 둘 → 다음은 **4등**(안 건너뜀).
- ROW_NUMBER: 동률 6000도 3·4로 **강제 구분**.

```sql
SELECT NAME, SALARY,
       RANK()       OVER (ORDER BY SALARY DESC) AS Rank,
       DENSE_RANK() OVER (ORDER BY SALARY DESC) AS DenseRank,
       ROW_NUMBER() OVER (ORDER BY SALARY DESC) AS RowNumber
FROM EMPLOYEE;
```

---

## 실제 예시

```sql
-- 고객별 총구매액 3종 순위 비교 — chinook
SELECT c.CustomerId, ROUND(SUM(i.Total),2) AS TotalPurchase,
       RANK()       OVER (ORDER BY SUM(i.Total) DESC) AS Rank,
       DENSE_RANK() OVER (ORDER BY SUM(i.Total) DESC) AS DenseRank,
       ROW_NUMBER() OVER (ORDER BY SUM(i.Total) DESC) AS RowNumber
FROM customers c JOIN invoices i ON c.CustomerId = i.CustomerId
GROUP BY c.CustomerId ORDER BY TotalPurchase DESC LIMIT 10;
```

---

## 자주 헷갈리는 것

**ROW_NUMBER는 동률이어도 매번 다른 번호** → 동률 처리 기준이 모호하면 실행마다 순서가 달라질 수 있어, 정렬 기준을 명확히(필요하면 tie-break 컬럼 추가).

---

## 더 알면 좋은 것

📌 [[GROUP BY]] 집계 결과 위에 순위 컬럼만 추가하는 패턴이 전형적(라이브13 §5.1).

---

## 관련 개념

- [[순위함수]] — 상위 개념
- [[PARTITION BY]] — 그룹별 순위
- [[NTILE]] — N등분 그룹 번호
