---
date: 2026-06-15
tags: [UNIQUE, 제약조건, 중복금지, NULL, SQL, 데이터베이스]
aliases: [UNIQUE, 유니크, 중복금지제약, 고유제약]
---

# UNIQUE

**정의**: 컬럼 값의 **중복을 허용하지 않는** [[제약조건]]. 같은 값을 두 번 넣으면 에러. 단 **NULL은 중복 허용**.

---

## 왜 필요한가

이메일·주민번호처럼 "한 값은 한 번만" 존재해야 하는 컬럼의 중복을 막는다.

---

## 어떻게 작동하나

```sql
email VARCHAR(50) NOT NULL UNIQUE   -- 같은 이메일 중복 INSERT → 에러
```

---

## 실제 예시

- `professor.email`, `student.email`에 UNIQUE.
- **email NULL인 학생 2명 INSERT → 둘 다 성공**(실습 예제21). NULL은 비교 대상이 아니라 중복으로 안 본다.

---

## 자주 헷갈리는 것

**⚠ UNIQUE는 NULL 중복을 허용한다.** "값이 같으면 안 된다"는 규칙인데 NULL은 "값이 없음"이라 비교가 안 된다 → NULL은 여러 개 들어갈 수 있다. **"필수 + 유일"을 원하면 `NOT NULL UNIQUE`** 를 함께 건다.

**이메일은 적합, 이름은 부적합.** 강사: 이메일은 UNIQUE가 맞지만 이름은 **동명이인** 때문에 UNIQUE를 걸면 안 된다.

---

## 더 알면 좋은 것

📌 [[기본키]] = NOT NULL + UNIQUE. PK는 UNIQUE에 NULL 금지까지 더한 것.
📌 여러 컬럼을 묶어 UNIQUE를 걸 수도 있다(복합 UNIQUE).

---

## 관련 개념

- [[제약조건]] — 상위 개념
- [[NOT NULL]] — 함께 쓰면 필수+유일
- [[기본키]] — NOT NULL + UNIQUE
- [[무결성제약]] — 고유 무결성
