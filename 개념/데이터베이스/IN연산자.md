---
date: 2026-06-11
tags: [IN, NOT_IN, 다중행서브쿼리, 서브쿼리, NULL, SQL, 데이터베이스]
---

# IN / NOT IN (다중행 연산자)

**정의**: 값이 **목록(또는 서브쿼리 결과) 중 하나와 일치**하는지 검사하는 연산자. `IN`(하나라도 일치하면 참) / `NOT IN`(모두 불일치면 참). 다중행 [[서브쿼리]]와 짝.

---

## 왜 필요한가

"품질팀 또는 영업팀 소속", "판매된 적 있는 트랙"처럼 **여러 후보 값 집합**과 비교할 때 쓴다. `OR`를 여러 번 쓰는 대신 간결하게, 그리고 그 집합을 서브쿼리로 동적으로 구할 수 있다.

---

## 어떻게 작동하나

```sql
-- 값 목록과 비교
SELECT NAME FROM EMPLOYEE WHERE DEPARTMENT_ID IN (1, 2, 3);

-- 서브쿼리 결과와 비교 (비연관)
SELECT t.Name FROM tracks t
WHERE t.TrackId IN (SELECT ii.TrackId FROM invoice_items ii);   -- 판매된 트랙

-- NOT IN : 판매 안 된 트랙
SELECT Name FROM tracks
WHERE TrackId NOT IN (SELECT TrackId FROM invoice_items);
```

---

## 실제 예시

- 음료 카테고리 상품이 포함된 주문(중첩 IN): `WHERE order_id IN (SELECT order_id FROM order_items WHERE product_id IN (SELECT product_id FROM products WHERE category_id=1))`.

---

## 자주 헷갈리는 것

**`NOT IN` + NULL 함정**: 서브쿼리 결과에 NULL이 하나라도 섞이면 `NOT IN`은 **아무 행도 반환하지 않을 수** 있다(NULL과의 비교가 UNKNOWN). 안전하게는 [[EXISTS|NOT EXISTS]]로 대체하거나 서브쿼리에서 `WHERE 키 IS NOT NULL`. → [[NULL필터링]]

**IN vs EXISTS**: 보통 IN은 비연관(서브쿼리 한 번 실행), EXISTS는 연관(행마다). 결과는 같은 경우가 많다(문제7=EXISTS, 문제8=IN 동일 결과).

---

## 더 알면 좋은 것

📌 다중컬럼과 결합 가능: `(col1, col2) IN (서브쿼리)` → [[다중컬럼서브쿼리]].

---

## 관련 개념

- [[서브쿼리]] — 상위 개념
- [[EXISTS]] — NULL 안전한 대안
- [[다중컬럼서브쿼리]] — 튜플 IN
- [[NULL필터링]] — NOT IN 함정
