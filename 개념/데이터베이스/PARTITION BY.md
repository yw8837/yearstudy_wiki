---
date: 2026-06-12
tags: [PARTITION_BY, 윈도우함수, OVER, SQL, 데이터베이스]
aliases: [PARTITION BY]
---

# PARTITION BY

**정의**: [[OVER절]] 안에서 **전체 집합을 소그룹(칸막이)으로 나누는 기준**. 윈도우 함수가 각 파티션 안에서만 따로 계산되게 한다. 강사 표현: **"칸막이를 치고 그 안에서 다시 매김".**

---

## 왜 필요한가

"전체 순위"가 아니라 "**국가별** 순위", "**부서별** 평균"이 필요할 때. PARTITION BY가 없으면 윈도우 함수는 전체를 한 묶음으로 본다. 칸막이를 치면 각 그룹마다 순위가 1부터 다시 시작하고, 평균/합도 그룹별로 따로 계산된다.

---

## 어떻게 작동하나

```sql
-- 전체 기준 순위
RANK() OVER (ORDER BY total DESC)
-- 국가별 기준 순위 (국가마다 1부터 다시)
RANK() OVER (PARTITION BY country ORDER BY total DESC)
```

```
country=KR  [ A:1  B:2  C:3 ]   country별로
country=US  [ D:1  E:2      ]   순위 재시작
```

---

## 실제 예시

```sql
-- 국가별 고객 구매액 순위 — chinook
SELECT c.Country, c.CustomerId, ROUND(SUM(i.Total),2) AS Total,
       RANK() OVER (PARTITION BY c.Country ORDER BY SUM(i.Total) DESC) AS CountryRank
FROM customers c JOIN invoices i ON c.CustomerId = i.CustomerId
GROUP BY c.Country, c.CustomerId
ORDER BY c.Country, CountryRank;
```

---

## 자주 헷갈리는 것

**GROUP BY와 다르다**: [[GROUP BY]]는 그룹당 1행으로 **행을 압축**하지만, PARTITION BY는 **행을 유지**하고 계산 범위만 나눈다. 둘을 같은 쿼리에서 함께 쓰기도 한다(GROUP BY로 집계 후 PARTITION BY로 순위).

---

## 더 알면 좋은 것

📌 여러 컬럼 가능: `PARTITION BY country, city`처럼 다중 기준도 OK.

---

## 관련 개념

- [[OVER절]] — PARTITION BY가 들어가는 자리
- [[윈도우함수]] — 적용 대상
- [[순위함수]] — PARTITION BY와 자주 결합
- [[GROUP BY]] — 행 압축(대비)
