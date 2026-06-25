---
date: 2026-06-08
tags: [DELETE, DML, 삭제, SQL, 데이터베이스]
---

# DELETE (삭제)

**정의**: 이미 저장된 값을 **삭제**하는 명령. `DELETE FROM 테이블 WHERE 조건;` [[CRUD]]의 Delete.

---

## 왜 필요한가

불필요해진 행을 제거할 때 쓴다(평점 낮은 책 정리, 재고 소진 항목 삭제 등).

---

## 어떻게 작동하나

```sql
DELETE FROM book WHERE rating = 1;   -- 평점 1점 책 삭제
DELETE FROM book WHERE stock < 16;   -- 재고 16 미만 삭제
```

⚠️ **WHERE 조건이 없으면 모든 데이터가 삭제된다.**
```sql
DELETE FROM book;   -- book 테이블 전체 삭제 (주의!)
```

---

## 실제 예시

- 재고 16 미만 책 정리: `DELETE FROM book WHERE stock < 16;`

---

## 자주 헷갈리는 것

⚠️ **WHERE 누락 = 전체 삭제**: 가장 위험한 실수. 삭제 전 같은 WHERE로 `SELECT` 해보고 대상 행을 확인하는 습관.

**`stock < 16` vs "재고 15"**: 판서 과제 표현("재고=15")과 정답 sql(`stock < 16`)이 다르나 결과는 사실상 동일. 정답 파일 기준 `stock < 16`. `[원문 확인 필요]`

---

## 더 알면 좋은 것

📌 한 번 DELETE한 데이터는 복구가 어렵다(트랜잭션·백업 없으면). 실무에선 "soft delete"(삭제 표시 컬럼)도 많이 쓴다.

---

## 관련 개념

- [[DML]] — 상위 개념
- [[INSERT]] · [[UPDATE]] — 나머지 DML
- [[WHERE]] — 대상 행 지정
