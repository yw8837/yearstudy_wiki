---
date: 2026-06-12
tags: [OVER, 윈도우함수, PARTITION_BY, ORDER_BY, WINDOWING, SQL, 데이터베이스]
aliases: [OVER, OVER 절]
---

# OVER 절

**정의**: [[윈도우함수]]의 **적용 범위를 정의**하는 필수 절. `OVER ( [PARTITION BY ...] [ORDER BY ...] [WINDOWING ...] )` 형태로, 함수를 "어느 행 묶음에, 어떤 순서로, 어디부터 어디까지" 적용할지 지정한다.

---

## 왜 필요한가

윈도우 함수는 `OVER` 없이는 쓸 수 없다. 같은 `SUM(Total)`이라도 `OVER()`면 전체합, `OVER(PARTITION BY Country)`면 국가별 합, `OVER(ORDER BY date ROWS ...)`면 누적합이 된다. **OVER의 내용이 결과를 통째로 바꾼다.**

---

## 어떻게 작동하나

| 구성요소 | 역할 | 생략 시 |
|:---|:---|:---|
| **PARTITION BY** | 전체를 소그룹(칸막이)으로 나눔 | 전체가 한 묶음 |
| **ORDER BY** | 묶음 내 정렬(순위·누적 기준) | 정렬 없음(순위/프레임 무의미) |
| **WINDOWING(ROWS/RANGE)** | 더할 행 범위 | ORDER BY 있으면 기본 `UNBOUNDED PRECEDING ~ CURRENT ROW` |

```sql
SUM(Total) OVER ()                                  -- 전체합
SUM(Total) OVER (PARTITION BY Country)              -- 국가별 합
SUM(Total) OVER (ORDER BY date
                 ROWS BETWEEN UNBOUNDED PRECEDING
                 AND CURRENT ROW)                   -- 누적합
```

---

## 실제 예시

```sql
-- 같은 행에 국가 평균 붙이기 — chinook
SELECT InvoiceId, BillingCountry, Total,
       AVG(Total) OVER (PARTITION BY BillingCountry) AS CountryAvg
FROM invoices;
```

---

## 자주 헷갈리는 것

**빈 OVER()의 의미**: `OVER()`는 "파티션·정렬 없이 전체"다. 단, [[GROUP BY]] 결과 위에 `SUM() OVER()`를 다시 쓰면 전체합이 의도와 달라질 수 있어 강사가 [[스칼라서브쿼리]] 사용을 권장(GrandTotal 정정).

**ORDER BY 두 곳**: `OVER(ORDER BY ...)`(윈도우 정렬)와 쿼리 끝 `ORDER BY`(결과 정렬)는 다른 것.

---

## 더 알면 좋은 것

📌 WINDOWING 생략 + ORDER BY 존재 = 기본 프레임이 "처음~현재 행"이라, [[FIRST_VALUE LAST_VALUE|LAST_VALUE]]가 의도대로 안 나옴 → [[윈도우프레임]] 참고.

---

## 관련 개념

- [[윈도우함수]] — OVER를 쓰는 함수들
- [[PARTITION BY]] — 칸막이
- [[윈도우프레임]] — ROWS/RANGE 범위
