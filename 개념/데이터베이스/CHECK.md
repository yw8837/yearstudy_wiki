---
date: 2026-06-15
tags: [CHECK, 제약조건, 값제한, 도메인무결성, SQL, 데이터베이스]
aliases: [CHECK, 체크제약, 값제한제약]
---

# CHECK

**정의**: 컬럼 값이 **지정한 범위·조건을 만족하는지 검사**하는 [[제약조건]]. 조건을 벗어나면 에러.

---

## 왜 필요한가

"나이는 19세 이상", "학점은 1~3", "점수는 0~100"처럼 값의 유효 범위를 DB가 직접 강제한다(도메인 무결성).

---

## 어떻게 작동하나

```sql
hire_year  INT CHECK (hire_year >= 1980),
birth_year INT CHECK (birth_year >= 1990 AND birth_year <= 2010),
grade      INT CHECK (grade >= 1 AND grade <= 4),
credit     INT NOT NULL CHECK (credit >= 1 AND credit <= 3),
score      INT CHECK (score >= 0 AND score <= 100)

-- credit = 5 INSERT → ❌ CHECK 위반 (1~3 범위 밖)
```

---

## 실제 예시

- `school_db`: credit 1~3, grade 1~4, score 0~100, hire_year ≥1980, birth_year 1990~2010.
- 문자 패턴도 가능(SQLite): `CHECK (email LIKE '%@%.%')`, `CHECK (username GLOB '[a-z0-9]*')`.

---

## 자주 헷갈리는 것

**⚠ 과도한 CHECK는 정상 입력도 막는다.** [[constraint_demo]] bad.db는 아이디 10~12자·비번 정확히 8자 같은 과한 CHECK로 멀쩡한 가입을 전부 실패시킨다. 제약은 **비즈니스 로직에 맞게 최소한**으로.

**CHECK는 NULL을 통과시킨다.** 조건이 NULL에 대해 "참도 거짓도 아님"이라, 값이 NULL이면 CHECK를 통과한다(필수면 [[NOT NULL]] 별도 추가).

---

## 더 알면 좋은 것

📌 [[CONSTRAINT명시]]로 CHECK에 이름을 주면 관리가 쉽다: `CONSTRAINT capacity_check CHECK (capacity > 0 AND capacity <= 200)`.
📌 SQLite도 CHECK를 지원한다(LIKE·GLOB·length 등 함수 활용 가능).

---

## 관련 개념

- [[제약조건]] — 상위 개념
- [[CONSTRAINT명시]] — CHECK에 이름 주기
- [[무결성제약]] — 도메인 무결성
- [[constraint_demo]] — 과도한 CHECK 사례
