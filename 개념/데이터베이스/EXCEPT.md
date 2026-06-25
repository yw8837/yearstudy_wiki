---
date: 2026-06-10
tags: [EXCEPT, MINUS, 차집합, 집합연산자, DBMS차이, SQL, 데이터베이스]
---

# EXCEPT (차집합, Oracle의 MINUS)

**정의**: **앞 SELECT 결과에서 뒤 SELECT 결과에 있는 행을 빼는** 집합 연산자. 중복은 제거. 관계형 대수의 차집합(A−B) 역할. Oracle에서는 `MINUS`라는 이름.

---

## 왜 필요한가

"전체 중에서 어떤 집합을 제외한 나머지"를 구할 때 쓴다. 예: 2009년 구매했지만 2010년엔 안 산 **이탈 고객**, 플레이리스트에 없는 트랙.

---

## 어떻게 작동하나

```sql
-- 이탈 고객: 2009 구매 − 2010 구매
SELECT CustomerId FROM invoices WHERE strftime('%Y', InvoiceDate) = '2009'
EXCEPT
SELECT CustomerId FROM invoices WHERE strftime('%Y', InvoiceDate) = '2010'
ORDER BY CustomerId;
```

- **방향이 중요하다**: `A EXCEPT B`와 `B EXCEPT A`는 결과가 다르다(앞에서 뒤를 뺌).

---

## 실제 예시

- 청구서 없는 고객(전체 − 청구서 있는 고객), 플레이리스트 미포함 트랙(전체 트랙 − 플리 트랙).
- ⚠ Chinook DB는 고객·인보이스 매칭이 거의 완전해 "청구서 없는 고객" 결과가 비어 보일 수 있다(데이터 특성).

---

## 자주 헷갈리는 것

**EXCEPT는 순서에 의존**한다([[UNION]]·[[INTERSECT]]는 순서 무관). "A에는 있고 B에는 없는" 것을 원하면 A를 앞에 둔다. `NOT IN`/`NOT EXISTS`로도 비슷한 결과를 얻지만, EXCEPT는 컬럼 전체 일치 기준의 집합 차이다.

---

## 더 알면 좋은 것

📌 **DBMS 차이**: SQLite는 `EXCEPT`, Oracle은 `MINUS`, **MariaDB는 10.3+부터 `EXCEPT` 지원**, **MySQL은 미지원**(JOIN+`IS NULL` 또는 `NOT IN`/`NOT EXISTS`로 대체).

---

## 관련 개념

- [[집합연산자]] — 상위 개념
- [[INTERSECT]] — 교집합(대조)
- [[NULL필터링]] · [[EXISTS]] — MySQL 등에서의 대체 수단
