---
date: 2026-06-08
tags: [ORDER_BY, 정렬, ASC, DESC, SQL, 데이터베이스]
---

# ORDER BY (정렬)

**정의**: 검색 결과를 **정렬하여 출력**하는 명령어. `ASC`(오름차순) / `DESC`(내림차순).

---

## 왜 필요한가

"비싼 순", "최신 순", "알파벳 순" 등 의미 있는 순서로 보려면 정렬이 필요하다. [[LIMIT]]과 합치면 "상위 N건"이 된다.

---

## 어떻게 작동하나

```sql
SELECT Name, UnitPrice FROM tracks ORDER BY UnitPrice DESC LIMIT 10;  -- 비싼 순
SELECT * FROM invoices ORDER BY Total DESC LIMIT 5;                   -- 결제 큰 5건
SELECT Title FROM albums ORDER BY Title ASC LIMIT 10;                 -- 알파벳 오름차순
```

- `ASC` = 오름차순(작은 값부터, 생략 시 기본).
- `DESC` = 내림차순(큰 값부터).

---

## 실제 예시

- 국가별 고객 수를 많은 순으로: `GROUP BY Country ORDER BY COUNT(*) DESC`.

---

## 자주 헷갈리는 것

**`DESC` 두 의미 주의**: 여기 `DESC`(내림차순)는 [[SQL]]의 `DESC 테이블명`(구조 보기)과 **완전히 다른 기능**이다.

**여러 기준**: `ORDER BY A DESC, B ASC`처럼 콤마로 다중 정렬 가능.

---

## 더 알면 좋은 것

📌 정렬은 실행 순서상 거의 마지막(SELECT 이후, LIMIT 직전)에 처리된다.

---

## 관련 개념

- [[SELECT]] — 함께 쓰는 명령
- [[LIMIT]] — 정렬 후 상위 N건
