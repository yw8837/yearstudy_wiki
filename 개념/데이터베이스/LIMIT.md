---
date: 2026-06-08
tags: [LIMIT, 조회, SQL, 데이터베이스]
---

# LIMIT

**정의**: 출력하는 데이터의 **개수를 제한**하는 명령. 데이터가 많을 때 상위 n개만 확인할 때 쓴다.

---

## 왜 필요한가

수만 행을 한 번에 띄우면 느리고 보기 어렵다. LIMIT로 상위 일부만 빠르게 확인한다(특히 [[ORDER BY]]와 함께 "상위 N건").

---

## 어떻게 작동하나

```sql
SELECT * FROM book LIMIT 5;     -- 앞 5개
SELECT * FROM book LIMIT 1, 5;  -- 2번째 데이터부터 5개 (시작 인덱스 0)
```

- `LIMIT n` = 앞 n개.
- `LIMIT 시작, 개수` = 시작 인덱스(0부터)부터 개수만큼.

---

## 실제 예시

- `SELECT Name, UnitPrice FROM tracks ORDER BY UnitPrice DESC LIMIT 10;` (가장 비싼 10곡)

---

## 자주 헷갈리는 것

**인덱스는 0부터**: `LIMIT 1, 5`는 "2번째" 데이터부터다(1번째 X).

**DBMS 차이**: Oracle 등은 LIMIT 문법이 다르다(ROWNUM·FETCH 등).

---

## 더 알면 좋은 것

📌 정렬 없이 LIMIT만 쓰면 "아무 N개"라 결과 순서가 보장되지 않는다. 보통 [[ORDER BY]]와 함께.

---

## 관련 개념

- [[SELECT]] — 함께 쓰는 조회 명령
- [[ORDER BY]] — 정렬 후 상위 N건
