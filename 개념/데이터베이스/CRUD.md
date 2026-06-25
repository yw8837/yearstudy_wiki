---
date: 2026-06-08
tags: [CRUD, SQL, DML, 데이터베이스]
---

# CRUD

**정의**: 데이터 조작의 4대 기초 — **C**reate(생성) · **R**ead(조회) · **U**pdate(수정) · **D**elete(삭제).

---

## 왜 필요한가

거의 모든 데이터 작업은 이 4가지의 조합이다. DB 기초의 핵심이며, 개발 직무에는 필수. (데이터 분석 직무는 임의 삭제/수정이 위험해 실무에선 제한적으로 쓴다.)

---

## 어떻게 작동하나

| CRUD | SQL | 예 |
|:---|:---|:---|
| Create | [[INSERT]] | 행 추가 |
| Read | [[SELECT]] | 조회 |
| Update | [[UPDATE]] | 값 수정 |
| Delete | [[DELETE]] | 행 삭제 |

> 강사 강조: **어려운 지점은 CRUD 자체가 아니라 여러 테이블 조합 / 조회 조건 설계**에서 발생한다.

---

## 실제 예시

- 게시판: 글 작성(C)·목록 조회(R)·수정(U)·삭제(D).
- HTTP 메서드(POST/GET/PUT·PATCH/DELETE)와도 1:1 대응 → [[HTTP]].

---

## 자주 헷갈리는 것

**Read = SELECT만이 아님**: 집계·조인·서브쿼리 등 조회 기법 전체가 Read의 영역이고 가장 복잡하다.

---

## 더 알면 좋은 것

📌 **DML과의 관계**: Create/Update/Delete가 [[DML]](데이터 조작어)에 해당. Read는 SELECT.

---

## 관련 개념

- [[SQL]] — CRUD를 수행하는 언어
- [[DML]] — INSERT/UPDATE/DELETE
- [[SELECT]] — Read
- [[HTTP]] — 웹 메서드와 대응
