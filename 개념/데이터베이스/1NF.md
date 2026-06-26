---
date: 2026-06-16
tags: [1NF, 1차정규화, 원자값, 정규화, 데이터베이스]
aliases: [1NF, 제1정규형, 1차정규화, FirstNormalForm]
---

# 1차 정규화 (1NF)

**정의**: 테이블의 **모든 컬럼이 하나의 값(원자값, atomic)**만 갖도록 도메인을 원자값으로 만드는 [[정규화]] 1단계.

---

## 왜 필요한가

한 셀에 다중값(`'CS101,CS102'`)이 들어가면 WHERE·JOIN·ORDER BY가 정상 동작하지 않고 무결성이 깨진다. 1NF는 다중값을 행으로 분리해 이를 막는다.

---

## 어떻게 작동하나

- 다중값 컬럼을 분해 → 한 셀에 한 값.
- `student_course_all`(다중값, PK 없음) → `student_course_1nf`(원자값, **복합 PK `(student_id, course_id)`**).

---

## 실제 예시

```sql
CREATE TABLE student_course_1nf (
    student_id INTEGER,
    course_id  TEXT,
    grade      TEXT,
    ...,
    PRIMARY KEY (student_id, course_id)
);
```

---

## 자주 헷갈리는 것

**1NF여도 중복은 남는다.** 원자값으로 만들어도 학과·교수 정보가 행마다 반복된다 → [[2NF]]·[[3NF]]가 추가로 필요.

---

## 더 알면 좋은 것

📌 1NF·관계형 모델은 Codd(1970)가 최초 정의했다.

---

## 관련 개념

- [[정규화]] — 상위 과정
- [[2NF]] — 다음 단계
- [[복합기본키]] — 1NF 결과의 PK 형태
