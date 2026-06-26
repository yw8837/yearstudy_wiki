---
date: 2026-06-15
tags: [NOT_NULL, 제약조건, 필수입력, NULL, SQL, 데이터베이스]
aliases: [NOT NULL, 낫널, 필수입력제약]
---

# NOT NULL

**정의**: 컬럼에 **NULL(빈 값)을 금지**하는 [[제약조건]]. 값을 반드시 입력해야 한다 = "필수 입력".

---

## 왜 필요한가

이름·가격처럼 비어 있으면 안 되는 값을 강제할 때. NOT NULL이 없으면 누락된 채로 행이 들어가 분석·서비스에서 문제가 된다.

---

## 어떻게 작동하나

```sql
CREATE TABLE department (
    dept_id   VARCHAR(10) PRIMARY KEY,
    dept_name VARCHAR(50) NOT NULL   -- 학과명은 필수
);

-- dept_name 없이 INSERT → 에러
INSERT INTO department (dept_id) VALUES ('CSE');   -- ❌ NOT NULL 위반
```

---

## 실제 예시

- `school_db`: `name`, `dept_name`, `title`, `enroll_date` 등에 NOT NULL.

---

## 자주 헷갈리는 것

**NULL ≠ 0 ≠ 빈 문자열.** NULL은 "값이 없음(미지)"이다. `0`이나 `''`(빈 문자열)은 값이 있는 것이라 NOT NULL을 통과한다.

**NOT NULL + [[DEFAULT]] 조합.** 필수이면서 기본값을 주려면 둘을 함께 쓴다(`NOT NULL DEFAULT 'N'`). 그러면 미입력 시 기본값이 채워져 위반이 안 난다.

---

## 더 알면 좋은 것

📌 [[기본키]]는 자동으로 NOT NULL(+ UNIQUE)이다.
📌 "필수 + 유일"이 필요하면 `NOT NULL UNIQUE`를 같이 건다 — [[UNIQUE]]만으론 NULL 중복이 허용되기 때문.

---

## 관련 개념

- [[제약조건]] — 상위 개념
- [[DEFAULT]] — 미입력 시 기본값(NOT NULL과 조합)
- [[UNIQUE]] — 함께 쓰면 필수+유일
- [[무결성제약]] — NULL 무결성
