---
date: 2026-06-15
tags: [ALTER, ALTER_TABLE, DDL, 스키마변경, SQLite, SQL, 데이터베이스]
aliases: [ALTER TABLE, ALTER, 테이블수정, 컬럼추가]
---

# ALTER TABLE

**정의**: 이미 만든 테이블의 **구조를 변경**하는 [[DDL]] 명령. 컬럼 추가·수정·삭제, 테이블명 변경 등.

---

## 왜 필요한가

운영 중 요구사항이 바뀌면(컬럼 추가·이름 변경 등) 테이블을 다시 만들지 않고 구조만 고쳐야 한다.

---

## 어떻게 작동하나

```sql
ALTER TABLE 테이블명 ADD COLUMN 컬럼명 데이터타입 [제약];   -- 컬럼 추가
ALTER TABLE 테이블명 MODIFY COLUMN 컬럼명 데이터타입;       -- 컬럼 수정 (MySQL)
ALTER TABLE 테이블명 CHANGE COLUMN 기존 새 데이터타입;      -- 컬럼명 변경 (MySQL)
ALTER TABLE 테이블명 DROP COLUMN 컬럼명;                    -- 컬럼 삭제
ALTER TABLE 기존명 RENAME 새명;                            -- 테이블명 변경
```

---

## 실제 예시

```sql
-- SQLite에서 가능한 ALTER (실습 예제28)
ALTER TABLE student ADD COLUMN phone VARCHAR(20);
```

---

## 자주 헷갈리는 것

**⚠ SQLite는 ALTER 제약이 크다.** `MODIFY/CHANGE COLUMN`, 제약 추가/삭제(`ADD/DROP CONSTRAINT`)는 **SQLite 미지원**(슬라이드는 MySQL 문법). SQLite에서 컬럼 추가(`ADD COLUMN`)·테이블명 변경(`RENAME`) 정도만 직접 가능.

**제약/기본값 변경은 우회.** SQLite에선 **새 테이블 생성 → `INSERT … SELECT` 복사 → 기존 DROP → `RENAME`** 패턴으로 처리(실습 예제14~16).

---

## 더 알면 좋은 것

📌 DDL이라 보통 자동 커밋 → 변경 전 백업·확인 권장.
📌 FK가 걸린 테이블 구조 변경은 참조 무결성에 주의. → [[무결성제약]]

---

## 관련 개념

- [[DDL]] — 상위 분류
- [[DROP TABLE]] — 삭제
- [[SQLite]] — ALTER 제약의 배경
- [[제약조건]]
