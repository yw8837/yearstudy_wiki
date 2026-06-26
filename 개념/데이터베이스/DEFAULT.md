---
date: 2026-06-15
tags: [DEFAULT, 제약조건, 기본값, SQL, 데이터베이스]
aliases: [DEFAULT, 디폴트, 기본값제약]
---

# DEFAULT

**정의**: 값을 지정하지 않으면 **자동으로 채워지는 기본값**을 정하는 [[제약조건]].

---

## 왜 필요한가

대부분 같은 값이 들어가는 컬럼(예: 가입 상태 'N', 정원 30)에서 매번 입력하지 않아도 되게 한다. 미입력 시 NULL 대신 의미 있는 값을 넣는다.

---

## 어떻게 작동하나

```sql
location     VARCHAR(50) DEFAULT '미정',   -- 미입력 시 '미정'
max_students INT         DEFAULT 30,
tuition_paid VARCHAR(1)  DEFAULT 'N'

-- location 빼고 INSERT → '미정' 자동
INSERT INTO department (dept_id, dept_name) VALUES ('MATH', '수학과');
```

---

## 실제 예시

- `department.location DEFAULT '미정'`, `course.max_students DEFAULT 30`, `student.tuition_paid DEFAULT 'N'`.
- SQLite의 시간 기본값: `created_at TEXT NOT NULL DEFAULT (datetime('now','localtime'))`.

---

## 자주 헷갈리는 것

**DEFAULT vs [[NOT NULL]] — 비즈니스로 판단.** 강사: 배송지는 DEFAULT로 아무 값이나 채우기보다 **NOT NULL로 강제**하는 게 나을 수 있다. 기본값이 의미 없으면 오히려 위험.

**⚠ 잘못된 DEFAULT는 사고를 부른다.** [[constraint_demo]]의 bad.db는 `plan_id DEFAULT 'VIP'`인데 부모 테이블엔 'VIP'가 없어 **모든 가입이 FK 위반으로 실패**한다 — DEFAULT 값이 [[외래키]] 참조 대상에 실제로 있어야 한다.

---

## 더 알면 좋은 것

📌 `NOT NULL DEFAULT 값`을 함께 쓰면 "필수지만 미입력 시 기본값"이 되어 NOT NULL 위반을 피한다.
📌 SQLite는 ALTER로 DEFAULT 변경을 직접 못 함 → 테이블 재생성 우회(예제15). → [[ALTER TABLE]]

---

## 관련 개념

- [[제약조건]] — 상위 개념
- [[NOT NULL]] — 택일·조합 대상
- [[constraint_demo]] — 잘못된 DEFAULT 사례
- [[외래키]] — DEFAULT 값이 참조 대상에 있어야 함
