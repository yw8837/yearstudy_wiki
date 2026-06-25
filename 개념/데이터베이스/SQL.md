---
date: 2026-06-08
tags: [SQL, 쿼리, 데이터베이스, CRUD, 데이터베이스]
---

# SQL (Structured Query Language)

**정의**: 데이터베이스에 접근하고 조작하기 위한 **표준 언어.** = 데이터베이스를 제어하는 방법.

---

## 왜 필요한가

데이터를 직접 파일로 뒤지는 대신, "무엇을 찾아라/넣어라/바꿔라"를 **선언적으로** 명령하면 DBMS가 알아서 처리한다. 검색·분석·서비스 개발의 공통 언어다.

---

## 어떻게 작동하나

쿼리(query)는 **명령 + 대상** 구조로 이루어진다.

```sql
SELECT Title FROM albums;   -- 명령(SELECT) + 대상 컬럼/테이블
DESC Employees;             -- 명령(DESC) + 대상(테이블 구조 보기)
```

SQL이 할 수 있는 것: 데이터 검색·삽입·수정·삭제, 데이터베이스 생성, 테이블 생성 등. 큰 줄기는 [[CRUD]].

---

## 실제 예시

- 조회: `SELECT * FROM customers WHERE Country = 'USA';`
- 삽입: `INSERT INTO book VALUES (...);`
- 수정/삭제: [[UPDATE]] / [[DELETE]]

---

## 자주 헷갈리는 것

**DBMS별 문법 차이**: 기본 개념은 같지만 LIMIT, 별칭, 세미콜론, ANY/ALL 등 세부 문법은 DBMS마다 다르다.

**`DESC`의 두 의미**: 테이블 구조 보기(`DESC 테이블`)와 정렬 내림차순([[ORDER BY]]의 `DESC`)은 전혀 다른 기능.

---

## 더 알면 좋은 것

📌 **분류**: 데이터 조작([[DML]] — INSERT/UPDATE/DELETE), 데이터 정의(DDL — CREATE/DROP), 조회(SELECT) 등으로 나뉜다.

---

## 관련 개념

- [[데이터베이스]] · [[관계형데이터베이스]] — 대상
- [[CRUD]] — 4대 기초 동작
- [[SELECT]] · [[DML]] — 대표 명령
