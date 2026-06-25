---
date: 2026-06-11
tags: [ALL, ANY, 다중행서브쿼리, 서브쿼리, SQLite, SQL, 데이터베이스]
---

# ALL / ANY (다중행 비교 연산자)

**정의**: 비교 연산자(`>`, `<` 등)를 다중행 [[서브쿼리]]에 적용하는 표준 SQL 연산자. **ALL** = 서브쿼리의 **모든 값**에 대해 조건 만족. **ANY** = 서브쿼리 값 중 **하나 이상** 만족. **⚠ SQLite는 미지원 → MAX/MIN으로 대체.**

---

## 왜 필요한가

"개발팀 **모든** 직원보다 급여가 많은 사람"(ALL), "개발팀 직원 **누구보다라도** 많은 사람"(ANY)처럼 집합 전체와의 대소 비교에 쓴다.

---

## 어떻게 작동하나

```sql
-- 표준 SQL
SELECT NAME FROM EMPLOYEE
WHERE SALARY >= ALL (SELECT SALARY FROM EMPLOYEE WHERE DEPARTMENT_ID = 1);  -- 모두보다 ≥
SELECT NAME FROM EMPLOYEE
WHERE SALARY >= ANY (SELECT SALARY FROM EMPLOYEE WHERE DEPARTMENT_ID = 1);  -- 하나라도 ≥
```

**SQLite 대체 (MAX / MIN):**
```sql
-- >= ALL  →  >= MAX(...)   (가장 큰 값보다 크면 모두보다 큼)
SELECT Name FROM tracks
WHERE Milliseconds > (SELECT MAX(t.Milliseconds) FROM tracks t
                      JOIN genres g ON t.GenreId=g.GenreId WHERE g.Name='Rock');

-- >= ANY  →  >= MIN(...)   (가장 작은 값보다 크면 하나라도보다 큼)
SELECT Name FROM tracks
WHERE Milliseconds > (SELECT MIN(t.Milliseconds) FROM tracks t
                      JOIN genres g ON t.GenreId=g.GenreId WHERE g.Name='Classical');
```

| 표준 | 의미 | SQLite 대체 |
|:---|:---|:---|
| `> ALL` | 모든 값보다 큼 | `> (SELECT MAX(...))` |
| `> ANY` | 하나 이상보다 큼 | `> (SELECT MIN(...))` |

---

## 실제 예시

- Rock 최장 트랙보다 긴 트랙(ALL→MAX), Classical 최단 트랙보다 긴 트랙(ANY→MIN).

---

## 자주 헷갈리는 것

**SQLite 미지원**: `ALL`/`ANY` 키워드를 쓰면 SQLite에선 오류. 반드시 MAX/MIN 스칼라 서브쿼리로 변환. 슬라이드(표준)와 Day04 답지(SQLite)의 문법이 달라지는 지점.

**방향 주의**: `>= ALL`은 최댓값 기준, `>= ANY`는 최솟값 기준. `<= ALL`이면 반대로 MIN, `<= ANY`면 MAX. 부등호 방향에 따라 MAX/MIN이 바뀐다.

---

## 더 알면 좋은 것

📌 `= ANY`는 [[IN연산자\|IN]]과, `<> ALL`은 `NOT IN`과 동치. 그래서 IN/NOT IN으로도 표현 가능.

---

## 관련 개념

- [[서브쿼리]] — 상위 개념
- [[IN연산자]] — `= ANY` ≡ IN
- [[SQLite]] — ALL/ANY 미지원 환경
