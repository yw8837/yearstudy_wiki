---
date: 2026-06-12
tags: [FIRST_VALUE, LAST_VALUE, 행순서함수, 윈도우함수, 윈도우프레임, SQL, 데이터베이스]
aliases: [FIRST_VALUE, LAST_VALUE]
---

# FIRST_VALUE / LAST_VALUE

**정의**: 윈도우(파티션+정렬) 안에서 **첫 번째 값 / 마지막 값**을 반환하는 [[윈도우함수]]. 정렬 기준에 따라 그룹의 최소·최대·시작·끝 값을 각 행에 붙인다.

---

## 왜 필요한가

"각 국가에서 가장 작은/큰 청구금액을 모든 행 옆에 표시"처럼, 그룹의 양 끝 값을 원본 행과 함께 보고 싶을 때. 총액 오름차순으로 정렬하면 FIRST_VALUE=최소, LAST_VALUE=최대가 된다(라이브13 §5.5).

---

## 어떻게 작동하나

```sql
FIRST_VALUE(Total) OVER (
    PARTITION BY BillingCountry ORDER BY Total ASC
    ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING   -- 전체 창 명시!
) AS CountryMinTotal,
LAST_VALUE(Total) OVER (
    PARTITION BY BillingCountry ORDER BY Total ASC
    ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
) AS CountryMaxTotal
```

---

## 실제 예시

```sql
-- 국가별 최소/최대 청구금액 — chinook
SELECT InvoiceId, BillingCountry, Total,
       FIRST_VALUE(Total) OVER (PARTITION BY BillingCountry ORDER BY Total
           ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS MinTotal,
       LAST_VALUE(Total)  OVER (PARTITION BY BillingCountry ORDER BY Total
           ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS MaxTotal
FROM invoices ORDER BY BillingCountry, Total;
```

---

## 자주 헷갈리는 것

**⚠ LAST_VALUE 함정**: `ORDER BY`만 쓰고 프레임을 생략하면 기본값이 `UNBOUNDED PRECEDING ~ CURRENT ROW`(처음~현재)다. 그러면 LAST_VALUE가 "지금까지 중 마지막" = **현재 행 값**이 되어 의도한 최대값이 안 나온다 → **반드시 `... AND UNBOUNDED FOLLOWING`까지 명시**. FIRST_VALUE는 첫 행이라 영향 적지만 함께 명시하는 게 안전.

---

## 더 알면 좋은 것

📌 단순 그룹 최소/최대만 필요하면 [[GROUP BY]] + MIN/MAX가 더 간단. FIRST/LAST_VALUE는 "원본 행 유지 + 끝값 표시"가 목적.
📌 이전/이후 N번째 행 값은 [[LAG LEAD]].

---

## 관련 개념

- [[윈도우프레임]] — 프레임 명시 필수 이유
- [[LAG LEAD]] — 상대 위치 행 참조
- [[PARTITION BY]] · [[OVER절]] · [[윈도우함수]]
