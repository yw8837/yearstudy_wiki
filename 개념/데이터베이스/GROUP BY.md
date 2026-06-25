---
date: 2026-06-09
tags: [GROUP_BY, 그룹화, 집계, SQL, 데이터베이스]
---

# GROUP BY (그룹 집계)

**정의**: 기준 컬럼이 **같은 데이터끼리 그룹화**해 집계함수를 적용하는 절. `SELECT 기준컬럼, 집계함수 FROM 테이블 GROUP BY 기준컬럼;`

---

## 왜 필요한가

"국가별 매출", "장르별 평균 재생시간"처럼 **그룹 단위 통계**를 내려면 GROUP BY가 필요하다. [[집계함수]]와 거의 항상 함께 쓴다.

---

## 어떻게 작동하나

```sql
SELECT user_id, COUNT(*) FROM rental GROUP BY user_id;   -- 유저별 건수
SELECT BillingCountry, SUM(Total) FROM invoices GROUP BY BillingCountry;
SELECT Country, COUNT(*) FROM customers GROUP BY Country ORDER BY COUNT(*) DESC;
```

기준 컬럼이 같은 행들이 한 그룹이 되고, 그 그룹마다 집계함수가 한 값을 만든다.

---

## 실제 예시

- 장르별 평균 재생시간: `SELECT GenreId, AVG(Milliseconds) FROM tracks GROUP BY GenreId;`

---

## 자주 헷갈리는 것

**GROUP BY 대상 컬럼은 SELECT에도 포함**(정석). SQLite는 예외적으로 동작하기도 하지만, 결과 의미가 불명확해질 수 있어 정석 문법을 지킨다(강사 강조).

**그룹 후 조건은 HAVING**: 그룹 결과를 거르려면 WHERE가 아니라 [[HAVING]].

---

## 더 알면 좋은 것

📌 실행 순서: WHERE(행 필터) → GROUP BY(그룹화) → HAVING(그룹 필터) → SELECT → ORDER BY.

---

## 관련 개념

- [[집계함수]] — 함께 쓰는 함수
- [[HAVING]] — 그룹 결과 필터
- [[WHERE]] — 그룹 전 행 필터
