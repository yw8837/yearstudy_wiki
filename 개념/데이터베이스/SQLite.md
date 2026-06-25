---
date: 2026-06-08
tags: [SQLite, 데이터베이스, DBMS, 실습환경, 데이터베이스]
---

# SQLite

**정의**: 별도 서버 없이 **파일 하나로 동작하는 가벼운 관계형 DBMS.** 설치 부담이 낮아 기초 SQL 학습에 적합하다.

---

## 왜 필요한가

비대면·다양한 PC 환경에서 Oracle·MySQL 같은 서버형 DB는 설치 난이도·충돌 위험이 크다. SQLite는 파일 기반이라 가볍게 붙여 바로 실습할 수 있어 수업 실습 DB로 채택됐다.

---

## 어떻게 작동하나

- `.db` 파일 하나가 곧 데이터베이스. VS Code + **SQLTools** + **SQLTools SQLite** 확장으로 연결한다.
- 커넥션: SQLTools 패널 → Add New Connection → SQLite → Database File 경로 → Test Connection("Successfully connected") → Connect.
- Windows는 연동 안정화를 위해 Node.js 설치(Add to PATH) 권장.

---

## 실제 예시

- 실습 DB: `chinook.db`(SELECT 연습), `book.db`(DML 연습·수업 중 생성), `classicmodels.db`(시험용).

---

## 자주 헷갈리는 것

**SQLite ≠ 모든 SQL**: ANY/ALL 미지원(→ MIN/MAX 대체), RIGHT JOIN은 3.39.0+에서만 동작 등 일부 문법 차이가 있다.

**실습 플랫폼과 다를 수 있음**: 엘리스는 SQLite가 아닌 다른 엔진(MariaDB 등)일 수 있어 세미콜론·LIMIT 등 문법 차이가 날 수 있다.

---

## 더 알면 좋은 것

📌 **경로 표기**: Windows는 역슬래시 `\`, Mac은 슬래시 `/`.

📌 **동시 연결 주의**: 여러 DB를 동시에 연결하면 혼동 → 하나만 연결 유지(나머지 Disconnect) 권장.

---

## 관련 개념

- [[데이터베이스]] — 상위 개념
- [[SQL]] — 사용 언어
- [[VSCode]] — 실습 에디터
