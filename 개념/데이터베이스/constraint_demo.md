---
date: 2026-06-15
tags: [constraint_demo, 제약조건, 실패사례, FK, DEFAULT, CHECK, Streamlit, 데이터베이스]
aliases: [constraint_demo, good_bad_db, 제약데모]
---

# constraint_demo (good.db vs bad.db)

**정의**: "제약조건을 잘못 설계하면 **멀쩡한 UI도 가입이 안 된다**"를 보여주는 실습 미니프로젝트. 같은 Streamlit 회원가입 UI에 **good.db(적절한 제약)** 와 **bad.db(과도/잘못된 제약)** 를 붙여 결과 차이를 본다.

---

## 왜 필요한가

[[제약조건]]은 강력하지만, **과하거나 잘못 설계하면 정상 사용자조차 가입을 막는다.** 제약을 "비즈니스 로직에 맞게 최소한"으로 설계해야 한다는 걸 체감하는 사례.

---

## 어떻게 작동하나

INSERT SQL은 양쪽 동일(`INSERT INTO users (username, email, password) VALUES (?, ?, ?)`). 차이는 **테이블 정의(제약)**.

| 구분 | good.db | bad.db |
|:---|:---|:---|
| 아이디 | 3자 이상 | 10~12자, 소문자·숫자만 |
| 이메일 | 일반 형식(`%@%.%`) | `@school.ac.kr`만 |
| 비밀번호 | 4자 이상 | 정확히 8자 + 숫자·대문자 필수 |
| FK | 없음 | `plan_id` DEFAULT `'VIP'` (부모엔 'BASIC'만 → 항상 위반) |

---

## 실제 예시 (시나리오)

- `kim`/`kim@test.com`/`1234` → **good 성공**, bad는 [[CHECK]] 위반 실패.
- bad의 CHECK를 모두 통과(`kimminjun01`/`minjun@school.ac.kr`/`Pass1234`)해도 → **[[외래키]] 위반 실패**. 앱은 `plan_id`를 안 보내는데 DB가 존재하지 않는 DEFAULT `'VIP'`를 강제하기 때문.

---

## 자주 헷갈리는 것

**"CHECK만 통과하면 끝"이 아니다.** bad.db는 CHECK를 다 만족해도 **잘못된 [[DEFAULT]] + [[외래키]] 조합** 때문에 INSERT 자체가 불가능하다. DEFAULT 값이 FK 참조 대상에 실제로 존재해야 한다.

**문제는 UI가 아니라 DB 설계.** 같은 UI라도 제약 설계가 결과를 가른다.

---

## 더 알면 좋은 것

📌 좋은 제약 = 필요한 무결성만 최소로(good.db). 나쁜 제약 = 과한 CHECK + 모순된 DEFAULT/FK(bad.db).
📌 강사: "쓸모없는 데이터" 판단은 분석가가 아니라 **도메인 지식 보유자**가 한다.

---

## 관련 개념

- [[제약조건]] — 데모의 주제
- [[CHECK]] — 과도한 제약 사례
- [[DEFAULT]] · [[외래키]] — 모순 설계의 핵심
- [[무결성제약]]
