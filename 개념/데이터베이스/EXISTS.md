---
date: 2026-06-09
tags: [EXISTS, NOT_EXISTS, 상관서브쿼리, NULL, SQL, 데이터베이스]
---

# EXISTS / NOT EXISTS

**정의**: 서브쿼리 결과가 **존재하는지 여부**만 판단하는 상관 서브쿼리. `EXISTS`(있으면 참) / `NOT EXISTS`(없으면 참).

---

## 왜 필요한가

"트랙이 있는 앨범", "한 번도 구매 안 된 트랙"처럼 **관련 행의 존재/부재**로 거를 때 쓴다. 특히 `NOT IN`이 NULL에 취약한 문제를 안전하게 대체한다.

---

## 어떻게 작동하나

```sql
-- 트랙이 하나라도 있는 앨범
SELECT Title FROM albums a
WHERE EXISTS (SELECT 1 FROM tracks t WHERE t.AlbumId = a.AlbumId);

-- 구매되지 않은 트랙 (NULL 안전)
SELECT Name FROM tracks t
WHERE NOT EXISTS (SELECT 1 FROM invoice_items ii WHERE ii.TrackId = t.TrackId);
```

- 바깥 행마다 서브쿼리를 평가해 행이 하나라도 있으면 EXISTS = 참.
- `SELECT 1`처럼 무엇을 고르든 상관없다(존재 여부만 봄).

---

## 실제 예시

- "담당 고객이 1명 이상인 직원" 등 존재 기반 필터.

---

## 자주 헷갈리는 것

**NOT IN vs NOT EXISTS**: 서브쿼리 결과에 NULL이 섞이면 `NOT IN`은 아무것도 반환 안 할 수 있다. `NOT EXISTS`는 **NULL에 안전** → 강사·실습 파일 모두 권장.

---

## 더 알면 좋은 것

📌 EXISTS는 "있으면 즉시 참"이라 행을 다 안 세도 되어 대량 데이터에서 유리할 수 있다.

---

## 관련 개념

- [[서브쿼리]] — 상위 개념
- [[NULL필터링]] — NULL 안전성
- [[스칼라서브쿼리]] — 다른 상관 서브쿼리
