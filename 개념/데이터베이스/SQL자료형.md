---
date: 2026-06-15
tags: [SQL자료형, 데이터타입, VARCHAR, INT, FLOAT, DATETIME, SQL, 데이터베이스]
aliases: [SQL자료형, 데이터타입, 자료형SQL, 데이터형식]
---

# SQL 자료형 (데이터 타입)

**정의**: 컬럼에 저장할 값의 **종류(문자·숫자·날짜 등)를 지정**하는 형식. CREATE 시 컬럼마다 정한다. **DBMS마다 차이**가 있다.

---

## 왜 필요한가

자료형을 정해야 DB가 값의 형식을 검증하고(도메인 무결성), 저장 공간·연산 방식을 결정할 수 있다. 예: 날짜를 `DATETIME`으로 두면 날짜 연산이 가능.

---

## 어떻게 작동하나

| 타입 | 의미 | 비고 |
|:---|:---|:---|
| `VARCHAR(n)` | 가변 길이 문자 | n = 최대 길이(바이트) |
| `INT` | 정수 | 4바이트 |
| `FLOAT` | 부동소수점 | 4바이트 |
| `DATETIME` | 날짜+시간 | `YYYY-MM-DD HH:MM:SS` |

```sql
email     VARCHAR(50) NOT NULL UNIQUE,
hire_year INT CHECK (hire_year >= 1980),
enroll_date DATE NOT NULL
```

---

## 실제 예시

- `school_db`의 `student.name VARCHAR(20)`, `birth_year INT`, `enrollment.enroll_date DATE`.

---

## 자주 헷갈리는 것

**길이는 넉넉히, 그러나 의미 있게.** 강사: VARCHAR 길이는 다국적 이름·확장성을 고려해 정한다. 너무 짧으면 정상 입력도 잘린다.

**타입 불일치 주의.** 화면 입력(문자열) → 앱 처리 → DB 저장(날짜형) 사이에서 형 변환이 잘못되면 정보가 유실될 수 있다.

**SQLite는 타입이 느슨하다.** SQLite는 동적 타입(타입 친화성, type affinity)이라 `VARCHAR(10)`에 더 긴 문자열도 들어간다. 길이 제한 강제는 DBMS마다 다르다.

---

## 더 알면 좋은 것

📌 DBMS마다 타입명이 다름(MySQL `DATETIME`, SQLite `TEXT`/`INTEGER`/`REAL`). 문법 의심되면 공식 문서 먼저 확인.
📌 자료형은 [[제약조건]] 중 도메인 무결성의 1차 방어선. → [[무결성제약]]

---

## 관련 개념

- [[제약조건]] — 값을 더 좁히는 규칙
- [[관계형데이터베이스]] — 컬럼·타입의 토대
- [[SQLite]] — 동적 타입 특성
