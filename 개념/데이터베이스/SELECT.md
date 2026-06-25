---
date: 2026-06-08
tags: [SELECT, FROM, 조회, SQL, 데이터베이스]
---

# SELECT / FROM

**정의**: 테이블에서 데이터를 **조회**하는 SQL 명령. `SELECT 컬럼 FROM 테이블;` = 명령(SELECT) + 검색할 컬럼 + 대상 테이블(FROM).

---

## 왜 필요한가

CRUD의 Read를 담당하는 가장 기본이자 가장 많이 쓰는 명령. 이후 [[WHERE]]·[[ORDER BY]]·[[GROUP BY]]·[[JOIN]]·[[서브쿼리]]가 모두 SELECT 위에 얹힌다.

---

## 어떻게 작동하나

```sql
SELECT Title FROM albums;          -- 특정 컬럼만
SELECT * FROM artists;             -- * = 모든 컬럼
SELECT Name, UnitPrice FROM tracks LIMIT 10;  -- 여러 컬럼 + 상위 10건
```

- `*`는 모든 컬럼. 단 실무에선 [[LIMIT]]·필요 컬럼으로 좁혀 쓰는 게 권장된다.

---

## 실제 예시

- `SELECT Name, UnitPrice FROM tracks ORDER BY UnitPrice DESC LIMIT 10;`

---

## 자주 헷갈리는 것

**SELECT * 남용**: 조회는 되지만 컬럼 과다·성능·가독성 문제. 실무에선 "조인 성공 확인용"으로만 쓰고 필요한 컬럼으로 좁힌다.

**컬럼 순서**: SELECT에 적은 순서대로 결과 컬럼이 나온다.

---

## 더 알면 좋은 것

📌 **실행 순서(개념)**: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT. 작성 순서와 실제 처리 순서가 다르다.

---

## 관련 개념

- [[WHERE]] — 행 조건
- [[ORDER BY]] — 정렬
- [[LIMIT]] — 행 수 제한
- [[DISTINCT]] — 중복 제거
