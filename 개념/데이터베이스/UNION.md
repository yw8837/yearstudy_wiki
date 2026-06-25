---
date: 2026-06-10
tags: [UNION, 합집합, 집합연산자, SQL, 데이터베이스]
---

# UNION (합집합)

**정의**: 두 SELECT 결과를 합치고 **중복 행을 제거**하는 집합 연산자. 관계형 대수의 합집합(A∪B) 역할.

---

## 왜 필요한가

서로 다른 출처의 행을 **하나의 목록으로 합치되 중복은 한 번만** 보고 싶을 때 쓴다(예: 직원+고객 통합 명단).

---

## 어떻게 작동하나

```sql
SELECT FirstName, LastName, 'Employee' AS Type
FROM employees
UNION
SELECT FirstName, LastName, 'Customer'
FROM customers
ORDER BY FirstName, LastName;
```

- 두 결과를 합친 뒤 **중복을 제거**하며, 이를 위해 내부적으로 **정렬 과정이 발생**한다.
- 단, 최종 결과의 정렬 순서를 보장하려면 맨 끝에 `ORDER BY`를 명시해야 한다(자동 정렬에 의존 X).
- 출처 구분이 필요하면 상수 컬럼(`'Employee'`)을 추가한다.

---

## 실제 예시

- "구매 이력 있는 고객(Has Invoice) + 없는 고객(No Invoice)"을 한 목록으로 — 조건이 다른 두 SELECT를 UNION.

---

## 자주 헷갈리는 것

**UNION vs [[UNION ALL]]**: UNION은 중복 제거(+정렬 비용), UNION ALL은 중복 그대로(더 빠름). 중복이 없을 게 확실하면 UNION ALL이 효율적.

---

## 더 알면 좋은 것

📌 중복 제거 때문에 UNION은 큰 데이터에서 정렬 비용이 든다. 표준 SQL의 합집합 연산이라 대부분 DBMS(Oracle/SQL Server/MySQL/MariaDB/SQLite)가 지원한다.

---

## 관련 개념

- [[집합연산자]] — 상위 개념
- [[UNION ALL]] — 중복 제거 안 하는 버전
- [[INTERSECT]] · [[EXCEPT]] — 교집합 · 차집합
