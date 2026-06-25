---
date: 2026-06-11
tags: [VIEW, 뷰, 가상테이블, SQLite, SQL, 데이터베이스]
---

# VIEW (뷰)

**정의**: 다른 테이블에서 파생된 **논리적 가상 테이블.** 물리적으로 데이터를 저장하지 않고, 조회 시 DBMS가 **뷰 정의(SELECT)대로 질의를 재작성**해 수행한다.

---

## 왜 필요한가

자주 쓰는 복잡한 [[JOIN]]·집계 쿼리를 매번 다시 쓰는 대신 **이름 붙여 저장**해 `SELECT * FROM 뷰명`으로 간단히 쓴다. ① 편리성(복잡 쿼리 재사용) ② 독립성(원천 테이블 구조가 바뀌어도 응용은 그대로) ③ 보안성(노출 컬럼/행 제한).

---

## 어떻게 작동하나

```sql
-- 표준 SQL
CREATE [OR REPLACE] VIEW view_name AS
SELECT column1, column2 FROM table_name WHERE condition;

-- SQLite (OR REPLACE 미지원 → DROP 먼저)
DROP VIEW IF EXISTS view_track_full;
CREATE VIEW view_track_full AS
SELECT t.TrackId, t.Name AS TrackName,
       al.Title AS AlbumTitle, ar.Name AS ArtistName, g.Name AS GenreName
FROM tracks t
INNER JOIN albums  al ON t.AlbumId   = al.AlbumId
INNER JOIN artists ar ON al.ArtistId = ar.ArtistId
INNER JOIN genres  g  ON t.GenreId   = g.GenreId;

SELECT * FROM view_track_full LIMIT 10;   -- 일반 테이블처럼 조회
```

---

## 실제 예시

- `view_track_full`: 트랙+앨범+아티스트+장르를 미리 조인해둔 뷰.
- **뷰 위에 뷰**: `view_customer_by_country`가 또 다른 뷰 `view_customer_public`을 FROM에 사용(특징 중 하나).

---

## 자주 헷갈리는 것

**데이터 저장 X**: 뷰는 결과를 저장하지 않는다. 원천 테이블이 바뀌면 뷰 조회 결과도 자동으로 바뀐다(항상 최신).

**SQLite 차이**: `CREATE OR REPLACE VIEW` 미지원 → `DROP VIEW IF EXISTS 뷰명;` 후 `CREATE VIEW`. 뷰 정의는 변경 불가라 어차피 삭제 후 재생성해야 한다.

**갱신 제약**: 뷰를 통한 INSERT/UPDATE는 제약이 많다(원천 PK 포함 필요 등). 조인 포함 뷰의 갱신 가능 여부는 DBMS마다 다름.

---

## 더 알면 좋은 것

📌 원천 테이블/뷰가 삭제되면 그것을 기반으로 한 뷰도 함께 삭제된다.

---

## 관련 개념

- [[JOIN]] · [[서브쿼리]] — 뷰 정의의 재료
- [[SQLite]] — OR REPLACE 미지원 환경
- [[Python으로DB연결]] — 뷰도 파이썬에서 동일하게 조회
