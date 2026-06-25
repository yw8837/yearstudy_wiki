---
date: 2026-06-12
tags: [ROLLUP, CUBE, GROUPING_SETS, 그룹함수, 차원집계, 소계, UNION, SQLite, SQL, 데이터베이스]
aliases: [ROLLUP, CUBE, GROUPING SETS, 그룹함수, 차원집계]
---

# ROLLUP / CUBE / GROUPING SETS (그룹 함수)

**정의**: [[GROUP BY]] 집계에 **소계·총계 등 집계 레벨을 추가**하는 그룹 함수. 그룹별 통계와 함께 부분합·전체합을 한 번에 산출한다.

---

## 왜 필요한가

"장르·미디어타입별 트랙수" 위에 "장르별 소계"와 "전체 합계"를 함께 보고 싶을 때. 각 레벨을 따로 쿼리해 UNION할 수도 있지만, Oracle 등은 전용 함수로 한 줄에 처리한다.

---

## 어떻게 작동하나

| 함수 | 생성 레벨 (A=장르, B=미디어타입) | 레벨 수 |
|:---|:---|:---:|
| **ROLLUP(A,B)** | (A,B) + (A) + 전체 | 3 |
| **CUBE(A,B)** | (A,B) + (A) + (B) + 전체 (모든 조합) | 4 |
| **GROUPING SETS(A,B)** | (A) + (B) | 2 |

```sql
-- Oracle 문법
GROUP BY ROLLUP(D.NAME, J.NAME)
GROUP BY CUBE(D.NAME, J.NAME)
GROUP BY GROUPING SETS(D.NAME, J.NAME)
```

---

## 실제 예시 (SQLite는 UNION 대체)

```sql
-- ROLLUP 대체: 조합 + 장르소계 + 전체 — chinook
SELECT g.Name AS Genre, mt.Name AS MediaType, COUNT(t.TrackId) AS Cnt
FROM tracks t JOIN genres g ON t.GenreId=g.GenreId
              JOIN media_types mt ON t.MediaTypeId=mt.MediaTypeId
GROUP BY g.Name, mt.Name
UNION ALL
SELECT g.Name, NULL, COUNT(t.TrackId)
FROM tracks t JOIN genres g ON t.GenreId=g.GenreId GROUP BY g.Name   -- 장르 소계
UNION ALL
SELECT NULL, NULL, COUNT(TrackId) FROM tracks                        -- 전체
ORDER BY Genre, MediaType;
```

---

## 자주 헷갈리는 것

**⚠ SQLite 미지원**: ROLLUP/CUBE/GROUPING SETS 모두 SQLite엔 없다 → ROLLUP=`UNION ALL`, CUBE=`UNION`, GROUPING SETS=`UNION ALL`로 대체(코드가 길어짐). 소계 행은 해당 컬럼을 `NULL`로 SELECT.

**CUBE = ROLLUP + 반대쪽 소계**: CUBE는 ROLLUP 결과에 (B)별 소계까지 더한 것. 그래서 4레벨.

---

## 더 알면 좋은 것

📌 그룹함수 실습은 Elice 플랫폼(Oracle/MariaDB 환경) 예제 참고 권장(라이브13 §4.2). SQLite 실습은 UNION 패턴으로.
📌 [[UNION]](중복 제거)과 [[UNION ALL]](중복 포함·빠름)의 선택이 결과 정확도에 영향.

---

## 관련 개념

- [[GROUP BY]] — 기반 집계
- [[집계함수]] · [[HAVING]]
- [[UNION]] · [[UNION ALL]] — SQLite 대체 수단
- [[SQLite]] — 미지원 제약
