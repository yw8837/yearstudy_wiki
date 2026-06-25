---
date: 2026-06-09
tags: [NULL, IS_NULL, IS_NOT_NULL, JOIN, SQL, 데이터베이스]
---

# NULL 필터링 (IS NULL / IS NOT NULL)

**정의**: 값이 비어 있는(NULL) 행을 거르는 조건. `IS NULL`(비어 있음) / `IS NOT NULL`(값 있음).

---

## 왜 필요한가

NULL은 "값이 없음"이라 `=`·`!=` 같은 비교로는 걸러지지 않는다. [[LEFT JOIN]] 후 "관계가 없는 행"(매칭 실패)을 찾는 데 핵심이다.

---

## 어떻게 작동하나

```sql
-- 앨범 없는 아티스트 (LEFT JOIN 후 오른쪽이 NULL)
SELECT artists.Name
FROM artists LEFT JOIN albums ON artists.ArtistId = albums.ArtistId
WHERE albums.AlbumId IS NULL;

-- 반대: 앨범이 있는 아티스트
... WHERE albums.AlbumId IS NOT NULL;
```

---

## 실제 예시

- "구매되지 않은 트랙", "담당 직원이 없는 고객" 등 "관계가 비어 있는" 데이터 탐색.

---

## 자주 헷갈리는 것

**`= NULL`은 동작 안 함**: 반드시 `IS NULL` / `IS NOT NULL`을 써야 한다.

**`NOT IN`의 NULL 함정**: 서브쿼리 결과에 NULL이 섞이면 `NOT IN`이 빈 결과를 낼 수 있다 → [[EXISTS]]의 `NOT EXISTS`가 안전.

---

## 더 알면 좋은 것

📌 COUNT(컬럼)은 NULL을 세지 않는다 → [[집계함수]]. NULL은 집계·조건 전반에 영향.

---

## 관련 개념

- [[LEFT JOIN]] — NULL이 생기는 대표 상황
- [[EXISTS]] — NULL 안전한 존재 검사
- [[WHERE]] — 조건 절
