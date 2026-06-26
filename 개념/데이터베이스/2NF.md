---
date: 2026-06-16
tags: [2NF, 2차정규화, 부분함수종속, 완전함수종속, 정규화, 데이터베이스]
aliases: [2NF, 제2정규형, 2차정규화, SecondNormalForm]
---

# 2차 정규화 (2NF)

**정의**: **[[부분함수종속]]을 제거**하여 모든 비키 속성이 후보키에 **[[완전함수종속]]**이 되도록 테이블을 분해하는 [[정규화]] 2단계. ([[1NF]] 만족이 전제)

---

## 왜 필요한가

복합키 일부에만 종속된 컬럼이 여러 행에 중복돼 [[이상현상]]이 생긴다. 2NF는 그 컬럼을 종속된 키 기준으로 분리해 중복을 줄인다.

---

## 어떻게 작동하나

- 단일 1NF 테이블 → **`student_2nf`(학번 기준) / `course_2nf`(과목코드 기준) / `enrollment`(성적, 복합 PK + FK 2개)** 3분리.
- 학생이름→학번, 과목이름→과목코드의 부분 종속을 각각 별도 테이블로.

---

## 실제 예시

```sql
CREATE TABLE enrollment (
    student_id INTEGER NOT NULL,
    course_id  TEXT NOT NULL,
    grade TEXT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES student_2nf(student_id),
    FOREIGN KEY (course_id)  REFERENCES course_2nf(course_id)
);
```

---

## 자주 헷갈리는 것

**2NF여도 이상이 남을 수 있다.** `student_2nf`에 학과 정보가 함께 있으면 `학번→학과코드→학과건물` [[이행함수종속]]이 잔존 → 학과 갱신 이상이 남는다(데모로 실증) → [[3NF]] 필요.

---

## 더 알면 좋은 것

📌 단일 컬럼 PK 테이블은 부분 종속이 불가능해 1NF면 자동으로 2NF.

---

## 관련 개념

- [[부분함수종속]] — 2NF가 제거하는 종속
- [[완전함수종속]] — 2NF의 목표 상태
- [[1NF]] · [[3NF]] — 앞뒤 단계
