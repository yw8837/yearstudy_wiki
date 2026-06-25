---
date: 2026-06-08
tags: [SQL, 데이터베이스, SELECT, WHERE, DML, 정리, 요약, 치트시트]
---

# SQL 시작하기 정리

DB/SQL 기초부터 SELECT·DML까지 **섹션별로 한눈에**. 자세한 설명은 개념 페이지 링크에서.

> 강의 출처: [[0608-SQL시작하기]] · 다음: [[SQL-함수와서브쿼리]]

---

## 1. DB·SQL·CRUD → [[데이터베이스]] · [[SQL]] · [[CRUD]]

- **데이터베이스** = 공유·통합 관리되는 데이터의 모음. DB 종류는 많아도 **기본 사용법은 공통.**
- **SQL** = Structured Query Language, DB 접근·조작 표준 언어.
- **CRUD** = Create/Read/Update/Delete — DB 조작 4대 기초. 어려움은 CRUD가 아니라 **테이블 조합·조건 설계**에서 온다.
- **SQLite**를 쓰는 이유: 가볍고 설치 부담 낮음(기초 학습용). 단 엘리스 플랫폼은 다른 엔진(MariaDB 등)일 수 있음.

## 2. 관계형 DB 구조 → [[관계형데이터베이스]]

- **관계형 DB** = 하나 이상의 테이블로 구성, 서로 연결된 데이터.
- **테이블 = 컬럼(세로) + 레코드(가로)**. 컬럼=필드(field) 혼용.

## 3. SELECT 기초 → [[SELECT]]

```sql
SELECT 컬럼 FROM 테이블;       -- * = 모든 컬럼
SELECT * FROM tracks LIMIT 10; -- 상위 n건
SELECT DISTINCT Country FROM customers;  -- 중복 제거(여러 컬럼=조합 기준)
```

| 명령 | 역할 | 예 |
|:---|:---|:---|
| [[SELECT]] · [[LIMIT]] | 조회 / 행 수 제한 | `SELECT Name FROM tracks LIMIT 10` |
| [[DISTINCT]] | 중복 제거 | `SELECT DISTINCT Country FROM customers` |
| [[WHERE]] | 조건 | `WHERE Country = 'USA'` |
| [[ORDER BY]] | 정렬 ASC/DESC | `ORDER BY Total DESC` |
| [[LIKE]] | 패턴 `%` | `WHERE Title LIKE 'Rock%'` |

## 4. 조건·연산자 → [[WHERE]] · [[연산자SQL]]

- 비교: `>` `<` `>=` `<=` `=` `!=` (문자열은 `'따옴표'`)
- 복합: `AND`/`&&`, `OR`/`||`, `NOT`/`!`
- 기타: `BETWEEN A AND B`(포함), `IN`(목록), `NOT IN`
- 패턴: [[LIKE]] — `'A%'`(시작) `'%A'`(끝) `'%A%'`(포함)

```sql
SELECT * FROM customers WHERE Country IN ('USA','Canada','Brazil');
SELECT * FROM invoices WHERE Total BETWEEN 5 AND 10;
```

## 5. DML — INSERT/UPDATE/DELETE → [[DML]]

> chinook 직접 조작 X → 별도 **book.db** 생성 후 연습. `DROP TABLE IF EXISTS`로 재실행 가능.

```sql
INSERT INTO book VALUES (14, 'Set Me Free', 'Young Adult', 17.46, 5, 19);  -- 컬럼 생략 시 정의 순서
UPDATE book SET stock = stock + 5 WHERE category = 'Poetry';
DELETE FROM book WHERE rating = 1;
```

- [[INSERT]]: 컬럼 생략 시 정의 순서대로. 문자열 내 `'`는 `''`로 이스케이프.
- [[UPDATE]]: `SET 컬럼=값`, 기존 값 연산 가능. **WHERE 누락 = 전체 수정.**
- [[DELETE]]: `DELETE FROM 테이블 WHERE 조건`. **WHERE 누락 = 전체 삭제.**
- [[기본키]]: `INTEGER PRIMARY KEY` 등 테이블 생성 시 지정.

---

## 관련

- [[SQL]] · [[CRUD]] · [[데이터베이스]] — 개념 상세
- [[SELECT]] · [[WHERE]] · [[DML]] — 명령어 상세
- [[SQL-함수와서브쿼리]] — 다음 날 집계·조인·서브쿼리
- [[0608-SQL시작하기]] — 원본 강의
