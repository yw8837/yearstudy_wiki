---
date: 2026-06-10
tags: [INTERSECT, 교집합, 집합연산자, DBMS차이, SQL, 데이터베이스]
---

# INTERSECT (교집합)

**정의**: 두 SELECT 결과에서 **양쪽에 모두 존재하는 행**만 추출하는 집합 연산자. 중복은 제거. 관계형 대수의 교집합(A∩B) 역할.

---

## 왜 필요한가

"두 조건을 **동시에** 만족하는 대상"을 구할 때 쓴다. 예: 2009년에도 사고 2010년에도 산 고객(둘 다 구매).

---

## 어떻게 작동하나

```sql
-- 2009·2010 모두 구매한 고객 ID
SELECT CustomerId FROM invoices WHERE strftime('%Y', InvoiceDate) = '2009'
INTERSECT
SELECT CustomerId FROM invoices WHERE strftime('%Y', InvoiceDate) = '2010';
```

- 교집합 결과(CustomerId 집합)를 인라인 뷰로 만들어 customers와 [[JOIN]]하면 이름까지 출력할 수 있다("분할 → 합치기").

---

## 실제 예시

- 청구서가 있는 고객(customers ∩ invoices), 두 해 모두 구매한 충성 고객.

---

## 자주 헷갈리는 것

**INTERSECT vs INNER JOIN**: 둘 다 "겹치는 것"을 구하지만, INTERSECT는 **컬럼 전체가 일치**하는 행을 집합으로 비교하고, JOIN은 **키로 행을 연결**한다. 이름 등 추가 컬럼이 필요하면 INTERSECT 결과에 JOIN을 덧붙인다.

---

## 더 알면 좋은 것

📌 **DBMS 차이**: SQLite·Oracle·MariaDB는 지원하지만 **MySQL은 INTERSECT 미지원** → `INNER JOIN`이나 `IN` 서브쿼리로 대체해야 한다.

---

## 관련 개념

- [[집합연산자]] — 상위 개념
- [[EXCEPT]] — 차집합(반대 개념)
- [[UNION]] — 합집합
- [[JOIN]] — 교집합 결과에 이름 붙이기
