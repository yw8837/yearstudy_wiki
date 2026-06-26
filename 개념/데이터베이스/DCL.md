---
date: 2026-06-15
tags: [DCL, 데이터제어어, GRANT, REVOKE, 권한, SQL, 데이터베이스]
aliases: [DCL, 데이터제어어, Data Control Language]
---

# DCL (데이터 제어어)

**정의**: [[SQL]]의 3분류 중 하나로, 데이터에 대한 **접근 권한을 부여·회수**하고 관리하는 명령. Data Control Language.

---

## 왜 필요한가

여러 사용자가 공유하는 DB에서 "누가 무엇을 할 수 있는가"를 통제해야 한다(보안·무결성). 그 권한을 다루는 게 DCL이다.

---

## 어떻게 작동하나

| 명령 | 역할 |
|:---|:---|
| `GRANT` | 권한 부여(예: SELECT 허용) |
| `REVOKE` | 권한 회수 |

```sql
GRANT SELECT ON student TO analyst;   -- analyst에게 조회 권한
REVOKE SELECT ON student FROM analyst; -- 회수
```

---

## 실제 예시

- 분석가 계정엔 조회(SELECT)만 주고, 수정·삭제는 막아 데이터를 보호.

---

## 자주 헷갈리는 것

**DCL vs [[DML]]**: DCL은 "권한"을, DML은 "데이터"를 다룬다. `GRANT`(권한) ↔ `SELECT`(데이터).

**SQLite엔 권한 개념이 거의 없다**: SQLite는 파일 기반 단일 사용자 DB라 GRANT/REVOKE 같은 권한 제어가 사실상 없다(파일 권한으로 통제). DCL은 MySQL·PostgreSQL 같은 서버형 DBMS에서 의미가 크다.

---

## 더 알면 좋은 것

📌 트랜잭션 제어(`COMMIT`/`ROLLBACK`)를 TCL로 따로 분류하기도 하지만, 이번 수업은 DDL/DML/DCL 3분류 기준.

---

## 관련 개념

- [[DDL]] — 데이터 정의어
- [[DML]] — 데이터 조작어
- [[데이터베이스]] — 공유·권한이 필요한 이유
