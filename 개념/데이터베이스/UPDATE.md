---
date: 2026-06-08
tags: [UPDATE, DML, 수정, SQL, 데이터베이스]
---

# UPDATE (수정)

**정의**: 이미 저장된 값을 **수정**하는 명령. `UPDATE 테이블 SET 컬럼=값 WHERE 조건;` [[CRUD]]의 Update.

---

## 왜 필요한가

가격 변경·재고 증감 등 기존 데이터를 바꿔야 할 때 쓴다.

---

## 어떻게 작동하나

```sql
UPDATE book SET price = 39.99 WHERE title = 'Soumission';   -- 특정 행
UPDATE book SET stock = stock + 5 WHERE category = 'Poetry'; -- 기존 값 기반 연산
```

- `SET 컬럼=값`으로 바꿀 값을 지정.
- **기존 값 기반 연산**도 가능: `stock = stock + 5`.

---

## 실제 예시

- Poetry 책 재고 일괄 +5, 특정 책 가격 인하 등.

---

## 자주 헷갈리는 것

⚠️ **WHERE 누락 = 전체 수정**: `UPDATE book SET price = 0;`은 **모든 행**의 가격을 바꾼다. 항상 WHERE를 먼저 확인.

---

## 더 알면 좋은 것

📌 수정 전 `SELECT`로 WHERE 조건이 맞는 행만 잡는지 먼저 확인하면 사고를 줄일 수 있다.

---

## 관련 개념

- [[DML]] — 상위 개념
- [[INSERT]] · [[DELETE]] — 나머지 DML
- [[WHERE]] — 대상 행 지정
