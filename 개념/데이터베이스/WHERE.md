---
date: 2026-06-08
tags: [WHERE, 조건, 연산자, SQL, 데이터베이스]
---

# WHERE

**정의**: 검색하고자 하는 데이터의 **조건을 설정**하는 절. 조건을 만족하는 행만 결과로 남긴다.

---

## 왜 필요한가

전체 데이터가 아니라 "USA 고객", "10 이상 결제" 같은 부분만 보려면 행을 거르는 조건이 필요하다. 조회의 핵심.

---

## 어떻게 작동하나

`SELECT ... FROM 테이블 WHERE 조건;` 형태. [[연산자SQL]]을 조합해 조건을 만든다.

```sql
SELECT * FROM customers WHERE Country = 'USA';        -- 문자열은 '따옴표'
SELECT * FROM tracks WHERE UnitPrice > 0.99 LIMIT 10; -- 비교
SELECT * FROM invoices WHERE Total >= 10;
```

비교 연산자: `>` `<` `>=` `<=` `=`(같다) `!=`(같지 않다).

---

## 실제 예시

- `WHERE Country = 'USA' AND State = 'CA'` (복합 조건)
- `WHERE Country IN ('USA','Canada','Brazil')` (목록)

---

## 자주 헷갈리는 것

**문자열 따옴표**: 문자 값은 `'USA'`처럼 작은따옴표로 감싼다.

**WHERE vs HAVING**: WHERE는 그룹 전 **행 필터**, [[HAVING]]은 그룹 후 **그룹 필터**.

**NULL은 `=`로 못 거른다**: `IS NULL` / `IS NOT NULL`을 써야 한다 → [[NULL필터링]].

---

## 더 알면 좋은 것

📌 [[UPDATE]]·[[DELETE]]에서 **WHERE를 빼면 전체 행이 수정/삭제**된다 — 매우 위험.

---

## 관련 개념

- [[연산자SQL]] — 조건에 쓰는 연산자
- [[LIKE]] — 패턴 조건
- [[SELECT]] — 함께 쓰는 명령
- [[NULL필터링]] — NULL 조건
