---
date: 2026-06-16
tags: [GRANT, REVOKE, DCL, 권한, MySQL, 데이터베이스]
aliases: [GRANT REVOKE, GRANT, REVOKE, 권한부여, 권한회수]
---

# GRANT / REVOKE

**정의**: [[DCL]]의 핵심 명령. **GRANT** = 권한 부여, **REVOKE** = 권한 회수. 서버형 DBMS([[MySQL]] 등)에서 "누가 무엇을 할 수 있는가"를 통제한다.

---

## 왜 필요한가

여러 사용자가 공유하는 DB에서 분석가에겐 조회만, 관리자에겐 수정까지 — 식으로 **권한을 차등 부여**해야 보안·무결성이 지켜진다.

---

## 어떻게 작동하나

```sql
CREATE USER 유저이름@localhost IDENTIFIED BY '비밀번호';   -- localhost=로컬만, 외부는 %
GRANT ALL PRIVILEGES ON 데이터베이스.테이블 TO 유저이름@localhost;
FLUSH PRIVILEGES;                                        -- 변경 즉시 적용
SHOW GRANTS FOR 유저이름@localhost;                       -- 권한 확인
REVOKE ALL ON 데이터베이스.테이블 FROM 유저이름@localhost; -- 회수
```

- `ALL` 대신 `SELECT`/`INSERT`/`DELETE` 등 개별 권한 지정 가능.

---

## 실제 예시

- 분석가 계정에 `GRANT SELECT ON shared_kickboard.* TO analyst@localhost;` → 조회만 허용, 수정·삭제 차단.

---

## 자주 헷갈리는 것

**SQLite엔 없다.** [[SQLite]]는 사용자/권한 개념 자체가 없어 `CREATE USER`·`GRANT`·`REVOKE`·`FLUSH PRIVILEGES`를 지원하지 않는다(파일 권한으로 통제). GRANT/REVOKE는 [[MySQL]]·PostgreSQL 같은 서버형 DBMS 전용.

---

## 더 알면 좋은 것

📌 `FLUSH PRIVILEGES`를 빼먹으면 권한 변경이 즉시 반영 안 될 수 있다.
📌 GRANT(권한) ↔ SELECT(데이터)는 다른 층위. 전자는 [[DCL]], 후자는 [[DML]].

---

## 관련 개념

- [[DCL]] — 상위 분류(데이터 제어어)
- [[MySQL]] — 권한 제어가 의미 있는 서버형 DBMS
- [[SQLite]] — 권한 개념 없음(비교)
