---
date: 2026-06-08
tags: [연산자, 비교연산자, 논리연산자, BETWEEN, IN, WHERE, SQL, 데이터베이스]
---

# SQL 연산자 (비교·논리·기타)

**정의**: [[WHERE]] 조건을 만들 때 쓰는 연산자들 — 비교(`=`,`>`…), 논리(`AND`,`OR`,`NOT`), 기타(`BETWEEN`,`IN`,`NOT IN`).

---

## 왜 필요한가

"USA이면서 CA", "5~10 사이", "이 목록 중 하나" 같은 조건을 표현하려면 연산자 조합이 필요하다.

---

## 어떻게 작동하나

**비교 연산자**

| 연산자 | 의미 |
|:---|:---|
| `>` `<` | 초과 / 미만 |
| `>=` `<=` | 이상 / 이하 |
| `=` | 같다 |
| `!=` | 같지 않다 |

**복합 조건**: `AND`/`&&`(둘 다), `OR`/`||`(또는), `NOT`/`!`(아닌 값).

**기타**: `BETWEEN A AND B`(A·B 경계 포함), `IN (...)`(목록 포함), `NOT IN (...)`(목록 미포함).

```sql
SELECT * FROM customers WHERE Country = 'USA' AND State = 'CA';
SELECT * FROM invoices  WHERE Total BETWEEN 5 AND 10;
SELECT * FROM customers WHERE Country IN ('USA','Canada','Brazil');
SELECT * FROM customers WHERE Country NOT IN ('USA','Canada','Brazil');
```

---

## 실제 예시

- `WHERE NOT Country = 'USA'` = `WHERE Country != 'USA'` (동일).

---

## 자주 헷갈리는 것

**NOT 남용보다 IN**: 여러 국가 조건은 `NOT`/`OR` 나열보다 `IN(...)`이 더 명확하다(강사 안내).

**BETWEEN은 경계 포함**: `BETWEEN 5 AND 10`은 5와 10도 포함.

**NULL은 못 거른다**: `= NULL`은 동작 안 함 → [[NULL필터링]].

---

## 더 알면 좋은 것

📌 `NOT IN`은 서브쿼리 결과에 NULL이 섞이면 빈 결과가 날 수 있다 → [[EXISTS]]의 `NOT EXISTS`가 안전.

---

## 관련 개념

- [[WHERE]] — 연산자를 쓰는 절
- [[LIKE]] — 패턴 매칭 연산자
- [[NULL필터링]] — NULL 처리
