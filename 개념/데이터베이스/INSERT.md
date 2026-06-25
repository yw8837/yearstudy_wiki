---
date: 2026-06-08
tags: [INSERT, DML, 삽입, SQL, 데이터베이스]
---

# INSERT (삽입)

**정의**: 관계형 데이터베이스의 테이블에 **값(행)을 저장**하는 명령. [[CRUD]]의 Create.

---

## 왜 필요한가

새 데이터를 테이블에 추가하려면 INSERT가 필요하다. 회원 가입·주문 생성 등 "생성" 작업의 기본.

---

## 어떻게 작동하나

```sql
INSERT INTO book (id, title, category, price, rating, stock)
VALUES (13, 'It''s Only the Himalayas', 'Travel', 45.17, 2, 19);

INSERT INTO book VALUES (14, 'Set Me Free', 'Young Adult', 17.46, 5, 19);  -- 컬럼 생략
```

- 컬럼을 **명시하지 않으면 테이블 정의 순서대로** 값이 삽입된다.
- 문자열 내 작은따옴표는 `''`(두 번)로 이스케이프: `'It''s Only...'`.

---

## 실제 예시

- 여러 행 한 번에: `INSERT INTO book (...) VALUES (...), (...), (...);`

---

## 자주 헷갈리는 것

**컬럼/값 개수 불일치 → 오류**: 명시한 컬럼 수와 VALUES 값 수가 같아야 한다.

**아포스트로피 데이터**: `It's` 같은 값은 `It''s`로 써야 한다(강사 강조).

---

## 더 알면 좋은 것

📌 컬럼 생략 방식은 편하지만, 테이블 구조가 바뀌면 깨질 수 있어 **컬럼 명시**가 더 안전하다.

---

## 관련 개념

- [[DML]] — 상위 개념
- [[UPDATE]] · [[DELETE]] — 나머지 DML
- [[기본키]] — id 등 PK
