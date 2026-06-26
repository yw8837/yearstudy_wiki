---
date: 2026-06-15
tags: [DDL, 데이터정의어, CREATE, ALTER, DROP, SQL, 데이터베이스]
aliases: [DDL, 데이터정의어, Data Definition Language]
---

# DDL (데이터 정의어)

**정의**: [[SQL]]의 3분류 중 하나로, 테이블 등 **데이터의 구조(스키마)를 정의·변경·삭제**하는 명령. Data Definition Language.

---

## 왜 필요한가

데이터를 넣기 전에 "어떤 표를, 어떤 컬럼·자료형·제약으로 만들 것인가"를 먼저 정의해야 한다. 그 구조를 다루는 게 DDL이다.

---

## 어떻게 작동하나

| 명령 | 역할 |
|:---|:---|
| `CREATE` | 테이블·DB 생성 |
| [[ALTER TABLE\|ALTER]] | 구조 변경(컬럼 추가·수정·삭제, 이름 변경) |
| [[DROP TABLE\|DROP]] | 테이블·DB 삭제 |

```sql
CREATE TABLE department (
    dept_id   VARCHAR(10) PRIMARY KEY,
    dept_name VARCHAR(50) NOT NULL
);
```

---

## 실제 예시

- 실습 `school_db`의 5테이블(department/professor/student/course/enrollment)을 `CREATE TABLE`로 정의.

---

## 자주 헷갈리는 것

**DDL vs [[DML]]**: DDL은 "표 자체(구조)"를 다루고, DML은 "표 안의 데이터(행)"를 다룬다. `CREATE TABLE`(DDL) ↔ `INSERT`(DML).

**DDL은 자동 커밋**: 많은 DBMS에서 DDL은 실행 즉시 반영(롤백 어려움). 구조 변경은 신중히.

---

## 더 알면 좋은 것

📌 SQLite는 `ALTER`로 컬럼 수정/제약 변경을 직접 못 한다 → 새 테이블 생성→복사→RENAME 우회. → [[ALTER TABLE]]

---

## 관련 개념

- [[DML]] — 데이터 조작어
- [[DCL]] — 데이터 제어어
- [[ALTER TABLE]] · [[DROP TABLE]] — 구조 변경·삭제
- [[제약조건]] — CREATE 시 함께 지정
