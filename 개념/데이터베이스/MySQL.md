---
date: 2026-06-16
tags: [MySQL, RDBMS, 데이터베이스, 서버형DB, DCL, 인덱스]
aliases: [MySQL, 마이에스큐엘]
---

# MySQL

**정의**: 가장 널리 쓰이는 **서버형 관계형 DBMS**([[관계형데이터베이스|RDBMS]]). 사용자/권한·여러 데이터베이스·ENUM/DATETIME 등을 지원. 슬라이드02 DB 구현의 기준 DBMS.

---

## 왜 필요한가

[[SQLite]]는 파일 1개 = DB 1개의 가벼운 단일 사용자 DB라 **공유·동시성·권한**에 한계가 있다. 조직 단위로 공유·접근 제어가 필요하면 MySQL 같은 서버형 DBMS를 쓴다.

---

## 어떻게 작동하나

서버를 실행하고 클라이언트로 접속해 SQL을 보낸다.

| | 윈도우 | 맥 |
|:---|:---|:---|
| 설치 | `choco install mysql` | `brew install mysql` |
| 서버 실행 | `net start mysql` | `mysql.server start` |
| 접속 | `mysql -u root -p` | `mysql -u root` |
| 종료 | `net stop mysql` | `mysql.server stop` |

- 하나의 서버 안에 여러 DB: `CREATE DATABASE` → `USE db` → 테이블 조작.

---

## 실제 예시

- 공유킥보드 DB: `CREATE DATABASE shared_kickboard; USE shared_kickboard;` 후 customer/borrow/kickboard/brand 테이블 생성.

---

## 자주 헷갈리는 것

**MySQL vs [[SQLite]].** MySQL은 [[DCL]](GRANT/REVOKE)·`CREATE/USE DATABASE`·`ENUM`·`SHOW INDEX`를 지원하지만 SQLite엔 없거나 문법이 다르다. 이번 회차 실습 코드는 SQLite, 슬라이드 구현은 MySQL.

---

## 더 알면 좋은 것

📌 `AUTO_INCREMENT`(MySQL) ↔ `AUTOINCREMENT`(SQLite), `ENUM`(MySQL) ↔ CHECK 대체(SQLite) 등 자료형·문법 차이에 주의.

---

## 관련 개념

- [[관계형데이터베이스]] — MySQL의 분류
- [[SQLite]] — 비교 대상(파일형)
- [[DCL]] · [[GRANT REVOKE]] — MySQL 권한 제어
- [[인덱스Index]] — MySQL 인덱스
