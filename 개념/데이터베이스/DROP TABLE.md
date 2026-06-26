---
date: 2026-06-15
tags: [DROP, DROP_TABLE, DDL, 삭제, 외래키, SQL, 데이터베이스]
aliases: [DROP TABLE, DROP, 테이블삭제]
---

# DROP TABLE

**정의**: 테이블 자체를 **구조째 삭제**하는 [[DDL]] 명령. 데이터와 정의가 모두 사라진다.

---

## 왜 필요한가

더 이상 필요 없는 테이블을 제거하거나, 실습에서 깨끗이 초기화(재생성)할 때.

---

## 어떻게 작동하나

```sql
DROP TABLE 테이블명;
DROP TABLE IF EXISTS 테이블명;   -- 없어도 에러 안 남 (재실행 안전)
```

---

## 실제 예시

```sql
-- FK 참조 역순으로 삭제 (자식 → 부모)
DROP TABLE IF EXISTS enrollment;
DROP TABLE IF EXISTS classroom;
DROP TABLE IF EXISTS course;
DROP TABLE IF EXISTS student;
DROP TABLE IF EXISTS professor;
DROP TABLE IF EXISTS department;
```

---

## 자주 헷갈리는 것

**⚠ FK가 있으면 순서가 중요.** 부모 테이블을 먼저 지우면 자식의 [[외래키]]가 깨져 에러. **반드시 자식 → 부모 역순**으로 DROP(live46 §4.2.3).

**DROP vs DELETE.** `DROP TABLE`은 표 자체를 없앤다([[DDL]]). `DELETE`는 표는 두고 행만 지운다([[DML]]). → [[DELETE]]

---

## 더 알면 좋은 것

📌 `IF EXISTS`를 붙이면 스크립트를 여러 번 돌려도 안전(없는 테이블 DROP 시 에러 방지).

---

## 관련 개념

- [[DDL]] — 상위 분류
- [[ALTER TABLE]] — 구조 변경
- [[DELETE]] — 행만 삭제(대비)
- [[외래키]] — DROP 순서의 이유
