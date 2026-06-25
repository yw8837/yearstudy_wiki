---
date: 2026-06-09
tags: [INNER_JOIN, JOIN, 교집합, SQL, 데이터베이스]
---

# INNER JOIN

**정의**: 두 테이블에서 **매칭되는 키가 있는 행(교집합)만** 결합해 조회하는 [[JOIN]]. 매칭 안 되는 행은 결과에서 제외된다.

---

## 왜 필요한가

"대여 기록이 있는 유저", "아티스트가 있는 앨범"처럼 **양쪽에 다 존재하는** 데이터만 보고 싶을 때 쓴다. 가장 기본적인 조인.

---

## 어떻게 작동하나

```sql
SELECT *
FROM rental INNER JOIN user
ON user.id = rental.user_id;   -- ON으로 연결 조건
```

```sql
-- 컬럼명 Name 중복 → 별칭으로 구분
SELECT tracks.Name AS TrackName, genres.Name AS GenreName
FROM tracks INNER JOIN genres ON tracks.GenreId = genres.GenreId;
```

`ON A.key = B.key` 양쪽 키가 일치하는 행만 합쳐진다.

---

## 실제 예시

- 3개 조인: `tracks INNER JOIN albums ON ... INNER JOIN artists ON ...` (Album 경유).

---

## 자주 헷갈리는 것

**매칭 안 되는 행 제외**: 대여 기록 없는 유저는 INNER JOIN 결과에 안 나온다. 그걸 보려면 [[LEFT JOIN]].

**컬럼명 충돌**: 같은 컬럼명(`Name`)은 `table.column` 또는 별칭(AS)으로 구분(ambiguous 오류 방지).

---

## 더 알면 좋은 것

📌 조인 순서는 바꿔도 되지만, 직접 연결 가능한 키가 있어야 한다. 없으면 중간 테이블 경유.

---

## 관련 개념

- [[JOIN]] — 상위 개념
- [[LEFT JOIN]] — 한쪽 전부 + NULL
- [[NULL필터링]] — 매칭 실패 행 찾기
