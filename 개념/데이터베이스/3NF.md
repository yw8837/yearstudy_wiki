---
date: 2026-06-16
tags: [3NF, 3차정규화, 이행함수종속, 정규화, 기준선, 데이터베이스]
aliases: [3NF, 제3정규형, 3차정규화, ThirdNormalForm]
---

# 3차 정규화 (3NF)

**정의**: **[[이행함수종속]]을 제거**하도록 테이블을 분해하는 [[정규화]] 3단계. B→C(비키 경유 종속)를 독립 테이블로 분리하고 [[외래키|FK]]로 연결. **실무의 타협 불가 기준선.**

---

## 왜 필요한가

[[2NF]]를 만족해도 비키 속성을 경유한 [[이행함수종속]]이 남으면 갱신 이상이 잔존한다. 3NF가 이를 제거해 **모든 갱신/삽입/삭제 이상을 해소**한다.

---

## 어떻게 작동하나

- `학번→학과코드→학과건물`의 `학과코드→학과건물`을 독립 테이블로 분리.
- 결과: **`department` / `professor` / `student` / `course` / `enrollment_3nf`** 5테이블. `student`는 `dept_id`(FK)만 보유.

---

## 실제 예시

```sql
CREATE TABLE department (
    dept_id INTEGER PRIMARY KEY AUTOINCREMENT,
    dept_code TEXT UNIQUE NOT NULL,
    dept_name TEXT NOT NULL,
    dept_building TEXT NOT NULL
);
CREATE TABLE student (
    student_id INTEGER PRIMARY KEY,
    student_name TEXT NOT NULL,
    dept_id INTEGER NOT NULL,
    FOREIGN KEY (dept_id) REFERENCES department(dept_id)
);
```

---

## 자주 헷갈리는 것

**어디까지 정규화?** 실무는 보통 **3NF로 충분**(99%). [[BCNF]]/[[4NF]]/[[5NF]]는 이론적 완전성, 성능이 필요하면 [[역정규화]]로 되돌린다. "3NF 이탈 시 성능 측정·근거 필요."

---

## 더 알면 좋은 것

📌 Codd: "3NF는 응용 프로그램 생명주기를 크게 연장한다"(관계를 이해·조작하기 쉽게).

---

## 관련 개념

- [[이행함수종속]] — 3NF가 제거하는 종속
- [[2NF]] · [[BCNF]] — 앞뒤 단계
- [[역정규화]] — 3NF에서 성능 위해 되돌림
