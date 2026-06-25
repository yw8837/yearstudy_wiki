---
date: 2026-06-12
tags: [LAG, LEAD, 행순서함수, 윈도우함수, MoM, 전월대비, SQL, 데이터베이스]
aliases: [LAG, LEAD]
---

# LAG / LEAD

**정의**: 현재 행을 기준으로 **이전 N번째 행(LAG) / 이후 N번째 행(LEAD)** 의 값을 옆 컬럼으로 가져오는 [[윈도우함수]]. `LAG(컬럼, N)` / `LEAD(컬럼, N)`.

---

## 왜 필요한가

"전월 대비 증감", "직전 주문과의 간격", "이동평균"처럼 **다른 행의 값을 현재 행과 나란히 비교**해야 할 때. "전월/전일 대비", "직전" 키워드가 나오면 LAG/LEAD를 떠올린다(라이브46 §4.2).

---

## 어떻게 작동하나

```sql
LAG(Total,  1) OVER (ORDER BY InvoiceDate) AS PrevTotal   -- 직전 행
LEAD(Total, 1) OVER (ORDER BY InvoiceDate) AS NextTotal   -- 다음 행
```

```
날짜    Total  PrevTotal  NextTotal
01-01    10    NULL       20      ← 첫 행: 이전 없음 → NULL
01-02    20    10         5
01-03     5    20         NULL    ← 끝 행: 이후 없음 → NULL
```

---

## 실제 예시

```sql
-- 전월 대비 매출 증감(MoM) — chinook
SELECT strftime('%Y-%m', InvoiceDate) AS YearMonth,
       SUM(Total) AS MonthlyTotal,
       LAG(SUM(Total),1) OVER (ORDER BY strftime('%Y-%m', InvoiceDate)) AS PrevMonth,
       SUM(Total) - LAG(SUM(Total),1) OVER (ORDER BY strftime('%Y-%m', InvoiceDate)) AS MoM_Diff
FROM invoices GROUP BY strftime('%Y-%m', InvoiceDate);
```

---

## 자주 헷갈리는 것

**경계는 NULL**: LAG는 첫 행, LEAD는 마지막 행에서 참조할 값이 없어 `NULL`. 주문 간격 분석 시 첫 주문(prevDate가 NULL)은 `WHERE prevDate IS NOT NULL`로 제외.

**증감률 분모 0/NULL**: `(이번 - 전월)/전월`에서 전월이 NULL이면 결과도 NULL.

---

## 더 알면 좋은 것

📌 활용: 전월대비(MoM), 재주문 간격(`julianday(orderDate)-julianday(prevDate)`로 이탈 위험 탐지 → [[이탈고객분석]]), 결제 패턴.
📌 [[PARTITION BY]]와 결합하면 "고객별 직전 주문" 등 그룹 내 비교.

---

## 관련 개념

- [[FIRST_VALUE LAST_VALUE]] — 절대 위치(첫/끝) 값
- [[strftime julianday]] — MoM·간격 계산 짝꿍
- [[이탈고객분석]] — 재주문 간격 활용
- [[누적합]] · [[이동평균]] · [[OVER절]]
