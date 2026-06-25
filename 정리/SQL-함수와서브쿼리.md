---
date: 2026-06-09
tags: [SQL, 집계함수, GROUP_BY, HAVING, JOIN, 서브쿼리, NULL, 정리, 요약, 치트시트]
---

# SQL 함수와 서브쿼리 정리

집계함수·GROUP BY·JOIN·서브쿼리를 **섹션별로 한눈에**. 자세한 설명은 개념 페이지 링크에서.

> 강의 출처: [[0609-SQL함수와서브쿼리]] · 이전: [[SQL-시작하기]]
>
> ⚠️ 아래 chinook 실습 정답은 학습용 **재구성(⚠)** 이다 — 원본 실습 파일엔 정답 쿼리가 비어 있어 슬라이드/판서/스키마 기반으로 표준 정답을 채웠다.

---

## 1. 집계 함수 → [[집계함수]]

| 함수 | 역할 | 비고 |
|:---|:---|:---|
| COUNT | 개수 | **NULL 제외.** `COUNT(*)` vs `COUNT(컬럼)` |
| SUM / AVG | 합계 / 평균 | |
| MAX / MIN | 최대 / 최소 | **숫자·문자 모두 가능** |

```sql
SELECT COUNT(*) FROM book;
SELECT AVG(korean), AVG(math) FROM grade;
```
- [[LIMIT]] 보강: 시작 인덱스 0 → `LIMIT 1, 5`는 2번째부터 5개.

## 2. GROUP BY / HAVING → [[GROUP BY]] · [[HAVING]]

- **GROUP BY** = 기준 컬럼별 그룹 집계. **기준 컬럼은 SELECT에도 포함**(정석).
- **HAVING** = 그룹 결과 필터(GROUP BY 이후, 단독 불가).
- WHERE(행 필터) **vs** HAVING(그룹 필터).

```sql
SELECT BillingCountry, SUM(Total) FROM invoices
GROUP BY BillingCountry HAVING SUM(Total) > 100;
```

## 3. JOIN → [[JOIN]]

| 종류 | 결과 | 비고 |
|:---|:---|:---|
| [[INNER JOIN]] | 교집합(매칭 키만) | `ON A.key=B.key` |
| [[LEFT JOIN]] | 왼쪽 전부 + 매칭 NULL | 권장 |
| RIGHT JOIN | 오른쪽 전부 | SQLite 3.39+ · LEFT로 대체 권장 |

- 정규화로 나눈 테이블을 다시 합치는 게 JOIN.
- 컬럼명 겹치면 `table.column`(ambiguous 방지), 동일 컬럼명은 **별칭(AS)**.
- **[[NULL필터링]]**: "관계 없는 행"은 `IS NULL` / `IS NOT NULL`로.

```sql
-- 앨범 없는 아티스트
SELECT artists.Name
FROM artists LEFT JOIN albums ON artists.ArtistId = albums.ArtistId
WHERE albums.AlbumId IS NULL;
```

## 4. 서브쿼리 → [[서브쿼리]]

> 핵심 학습법: **"분할 → 각각 실행 → 합치기".**
> 규칙: 괄호 필수 · ORDER BY 불가 · 연산자 오른쪽 · SELECT문만.

| 분류 | 설명 | 연산자 |
|:---|:---|:---|
| 단일행 | 1행 반환 | `= <> > >= < <=` |
| 다중행 | 2행+ 반환 | `IN` / `ANY` / `ALL` |
| [[스칼라서브쿼리]] | SELECT절, 1행, JOIN과 동일 결과 | |
| FROM절(파생) | 별칭 필수 | |
| [[EXISTS]] | 존재 여부, NULL 안전 | `EXISTS` / `NOT EXISTS` |

- ⚠️ **SQLite는 ANY/ALL 미지원 → MIN/MAX 대체** (ALL→MAX, ANY→MIN).
- `NOT IN`은 NULL에 취약 → **NOT EXISTS가 안전.**

```sql
SELECT Name FROM tracks
WHERE Milliseconds > (SELECT AVG(Milliseconds) FROM tracks);

SELECT Name FROM tracks t
WHERE NOT EXISTS (SELECT 1 FROM invoice_items ii WHERE ii.TrackId = t.TrackId);
```

---

## 관련

- [[집계함수]] · [[GROUP BY]] · [[HAVING]] — 그룹 집계 상세
- [[JOIN]] · [[INNER JOIN]] · [[LEFT JOIN]] · [[NULL필터링]] — 다수 테이블
- [[서브쿼리]] · [[스칼라서브쿼리]] · [[EXISTS]] — 쿼리 속 쿼리
- [[SQL-시작하기]] — 이전 날 SELECT/DML
- [[0609-SQL함수와서브쿼리]] — 원본 강의
