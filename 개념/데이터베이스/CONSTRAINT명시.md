---
date: 2026-06-15
tags: [CONSTRAINT, 제약조건명명, named_constraint, SQL, 데이터베이스]
aliases: [CONSTRAINT명시, CONSTRAINT, 제약조건명명, 명명된제약]
---

# CONSTRAINT 명시 (제약조건 이름 부여)

**정의**: [[제약조건]]에 **이름을 붙여** 정의하는 방식. `CONSTRAINT 이름 제약(...)` 형태로, 나중에 조회·관리가 쉬워진다.

---

## 왜 필요한가

이름 없는 제약은 DBMS가 임의 이름을 붙여 식별이 어렵다. 이름을 주면 어떤 제약인지 바로 알고, (지원 DBMS에선) 이름으로 추가·삭제할 수 있다.

---

## 어떻게 작동하나

```sql
CREATE TABLE classroom (
    room_id  VARCHAR(10),
    building VARCHAR(20) NOT NULL,
    capacity INT,
    CONSTRAINT classroom_pk   PRIMARY KEY (room_id),
    CONSTRAINT capacity_check CHECK (capacity > 0 AND capacity <= 200)
);

-- 복합 PK에도 이름 부여
CONSTRAINT enrollment_pk PRIMARY KEY (student_id, course_id)
```

---

## 실제 예시

- `enrollment_pk`([[복합기본키]]), `capacity_check`(CHECK) 등 답지에서 명명 사용.

---

## 자주 헷갈리는 것

**⚠ SQLite는 이름으로 ADD/DROP을 못 한다.** 슬라이드의 `ALTER TABLE ... ADD/DROP CONSTRAINT 이름`은 표준/MySQL 문법으로 **SQLite 미지원**. SQLite에선 CREATE 시 이름을 줄 수는 있지만, 이후 이름으로 떼어내려면 테이블 재생성이 필요하다.

**제약 조회도 DBMS마다 다름.** 표준은 `information_schema.table_constraints`, SQLite는 `PRAGMA table_info(...)`/`foreign_key_list(...)`.

---

## 더 알면 좋은 것

📌 복합 PK([[복합기본키]])는 컬럼 옆에 못 쓰고 테이블 레벨에서 정의 → 이때 `CONSTRAINT 이름 PRIMARY KEY (a, b)`로 이름과 함께 쓰면 깔끔하다.

---

## 관련 개념

- [[제약조건]] — 상위 개념
- [[복합기본키]] — 명명된 PK의 대표 사례
- [[CHECK]] — 이름 붙여 관리
- [[SQLite]] — 명명·관리 제약
