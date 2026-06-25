---
date: 2026-06-12
tags: [NTILE, 비율함수, 등급, 윈도우함수, SQL, 데이터베이스]
aliases: [NTILE]
---

# NTILE(N)

**정의**: 정렬된 행을 **N개의 그룹으로 거의 균등하게 나눠 그룹 번호(1~N)** 를 매기는 [[윈도우함수]]. 데이터를 분위(tier)로 등급화한다.

---

## 왜 필요한가

"신용한도 상/중/하 3등급", "매출 상위 25%·중위·하위" 같은 **분위 등급화**가 필요할 때. 값 자체가 아니라 "몇 분위 그룹에 속하는가"로 나눠 분석·세그먼트한다.

---

## 어떻게 작동하나

```sql
NTILE(3) OVER (ORDER BY creditLimit DESC)   -- 신용한도 높은 순 3등분
```

```
정렬 후 9행을 NTILE(3) → 그룹 1: 3행, 그룹 2: 3행, 그룹 3: 3행
나누어떨어지지 않으면 앞 그룹부터 1행씩 더 채움 (예: 10행 → 4,3,3)
```

---

## 실제 예시

```sql
-- 국가별 신용한도 3등급 — classicmodels
SELECT customerName, country, creditLimit,
       NTILE(3) OVER (PARTITION BY country ORDER BY creditLimit DESC) AS CreditTier
FROM customers WHERE creditLimit IS NOT NULL
ORDER BY country, CreditTier;

-- 직원 고용일 3그룹 — chinook
SELECT EmployeeId, HireDate,
       NTILE(3) OVER (ORDER BY HireDate) AS HireGroup
FROM employees ORDER BY HireDate;
```

---

## 자주 헷갈리는 것

**균등이 안 떨어지면 앞쪽 그룹이 1개 더**: 10행을 NTILE(3)하면 4/3/3. 정확히 N등분이 아니라 "최대한 균등".

**PERCENT_RANK와 다름**: NTILE은 정수 그룹 번호(1~N), [[PERCENT_RANK CUME_DIST|PERCENT_RANK]]는 연속 백분율.

---

## 더 알면 좋은 것

📌 [[PARTITION BY]]와 결합하면 "국가별 3등급"처럼 그룹 안에서 다시 분위.

---

## 관련 개념

- [[비율함수]] — 상위 개념
- [[PERCENT_RANK CUME_DIST]] — 연속 백분위
- [[순위함수]] · [[PARTITION BY]]
