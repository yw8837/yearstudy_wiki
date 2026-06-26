---
date: 2026-06-15
tags: [ERDEditor, VSCode확장, ERD, SchemaSQL, 도구, 데이터베이스]
aliases: [ERDEditor, ERD Editor, ERD에디터]
---

# ERD Editor (VS Code 확장)

**정의**: SQL 스키마와 [[ERD]]를 **양방향 변환**해 주는 VS Code 확장. SQL을 불러오면 관계도를 자동 생성하고, 반대로 ERD를 SQL로 내보낼 수 있다.

---

## 왜 필요한가

기존 DB의 구조를 그림으로 빠르게 파악하거나(코드→ERD), 설계한 ERD를 CREATE 문으로 변환(ERD→코드)할 때 손으로 그리지 않아도 된다.

---

## 어떻게 작동하나

```
1. VS Code 확장 "ERD Editor" 검색·설치
2. 작업 폴더에 .erd 파일 생성 (예: school_db.erd)
3. ERD 화면 우클릭
     → Database → SQLite → Import → Schema SQL
     → 만든 .sql 파일 선택 → 관계도 자동 생성
4. Export → .sql 로 내보내기 → CREATE TABLE 구문 생성
```

---

## 실제 예시

- `school_db.sql`을 Import → 5테이블 관계도 자동 생성, 다시 Export하면 CREATE 문 복원.

---

## 자주 헷갈리는 것

**⚠ Export한 SQL은 그대로 쓰면 안 된다.** 강사 강조: 코드↔ERD 변환은 되지만 **내보낸 SQL은 반드시 검토·수정 후 적용**한다(자료형·제약이 의도와 다를 수 있음).

---

## 더 알면 좋은 것

📌 Import 시 DBMS를 SQLite로 맞춰야 자료형 해석이 맞다.
📌 ERD로 관계([[카디널리티]])를 확인하면 [[JOIN]] 작성·[[외래키]] 설계가 쉬워진다.

---

## 관련 개념

- [[ERD]] — 생성·표현 대상
- [[IE까마귀발표기법]] — 자동 생성되는 표기
- [[VSCode]] — 실행 환경
- [[SQLite]] — Import 대상 DBMS
