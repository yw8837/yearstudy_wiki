---
date: 2026-06-08
tags: [DML, INSERT, UPDATE, DELETE, 데이터조작어, SQL, 데이터베이스]
---

# DML (데이터 조작어)

**정의**: 테이블의 **데이터를 삽입·수정·삭제**하는 SQL — **INSERT / UPDATE / DELETE.** [[CRUD]]의 Create·Update·Delete에 해당.

---

## 왜 필요한가

조회(SELECT)만으로는 데이터를 바꿀 수 없다. 행을 새로 넣고, 값을 고치고, 지우는 작업이 DML이다.

---

## 어떻게 작동하나

| 명령 | 역할 | 형태 |
|:---|:---|:---|
| [[INSERT]] | 삽입 | `INSERT INTO 테이블(컬럼...) VALUES(값...)` |
| [[UPDATE]] | 수정 | `UPDATE 테이블 SET 컬럼=값 WHERE 조건` |
| [[DELETE]] | 삭제 | `DELETE FROM 테이블 WHERE 조건` |

실습은 chinook를 직접 건드리지 않고 별도 **book.db**를 만들어 연습한다. 재실행 가능하게 `DROP TABLE IF EXISTS book;`로 시작.

```sql
DROP TABLE IF EXISTS book;
CREATE TABLE book (id INTEGER PRIMARY KEY, title TEXT, category TEXT,
                   price REAL, rating INTEGER, stock INTEGER);
```

---

## 실제 예시

```sql
INSERT INTO book VALUES (14, 'Set Me Free', 'Young Adult', 17.46, 5, 19);
UPDATE book SET stock = stock + 5 WHERE category = 'Poetry';
DELETE FROM book WHERE rating = 1;
```

---

## 자주 헷갈리는 것

**WHERE 누락 = 전체 적용**: UPDATE/DELETE에서 WHERE를 빼면 **모든 행**이 수정·삭제된다. 가장 주의할 점.

**원본 DB 보호**: 실습 시 기존 DB(chinook)를 직접 조작하지 말고 별도 DB에서 연습.

---

## 더 알면 좋은 것

📌 **직무별 활용도**: 데이터 분석 직무는 임의 삭제/수정이 위험해 실무에선 제한적, 개발 직무는 DML 필수.

📌 DDL(CREATE/DROP, 구조 정의)과는 구분된다. DML은 "내용", DDL은 "구조".

---

## 관련 개념

- [[INSERT]] · [[UPDATE]] · [[DELETE]] — 세 명령
- [[CRUD]] — 상위 개념틀
- [[기본키]] — 테이블 생성 시 지정
