---
date: 2026-06-12
tags: [strftime, julianday, 날짜함수, SQLite, 시계열, SQL, 데이터베이스]
aliases: [strftime, julianday, SQLite 날짜함수]
---

# strftime / julianday (SQLite 날짜 함수)

**정의**: SQLite의 날짜·시간 함수. `strftime(형식, 날짜)`는 날짜를 원하는 문자열로 포맷(연월 추출 등), `julianday(날짜)`는 율리우스일(연속 숫자)로 변환해 **두 날짜의 일수 차** 계산에 쓴다.

---

## 왜 필요한가

"월별 매출"(연·월 그룹화), "마지막 주문 후 며칠"(이탈 분석), "재주문 간격"처럼 **날짜를 묶거나 빼는** 작업이 시계열·고객 분석의 기본이다.

---

## 어떻게 작동하나

```sql
strftime('%Y-%m', orderDate)   -- '2005-05' (연-월 추출 → 월별 그룹화)
strftime('%Y', orderDate)      -- '2005'    (연도)
julianday('2005-05-31') - julianday(MAX(orderDate))   -- 두 날짜 일수 차(실수)
CAST(julianday(a) - julianday(b) AS INTEGER)          -- 정수 일수
```

| 형식 | 의미 |
|:---|:---|
| `%Y` | 4자리 연도 |
| `%m` | 2자리 월 |
| `%d` | 2자리 일 |

---

## 실제 예시

```sql
-- 월별 매출 집계 — classicmodels
SELECT strftime('%Y-%m', o.orderDate) AS YearMonth, SUM(...) 
FROM orders o GROUP BY strftime('%Y-%m', o.orderDate);

-- 마지막 주문 후 경과일 — classicmodels
CAST(julianday('2005-05-31') - julianday(MAX(o.orderDate)) AS INTEGER) AS DaysSinceLastOrder
```

---

## 자주 헷갈리는 것

**julianday는 실수 반환** → 일수로 보려면 `CAST(... AS INTEGER)`.

**DB마다 다르다**: 이건 SQLite 함수. Oracle은 `TO_CHAR`/날짜 뺄셈, MySQL은 `DATE_FORMAT`/`DATEDIFF`. 슬라이드와 실습 문법이 다를 수 있음.

---

## 더 알면 좋은 것

📌 [[LAG LEAD|LAG]](orderDate)로 직전 주문일을 가져와 `julianday` 차이를 내면 재주문 간격 → [[이탈고객분석]].

---

## 관련 개념

- [[이탈고객분석]] — 마지막 주문·간격 계산에 사용
- [[LAG LEAD]] — 직전 날짜 참조
- [[SQLite]] · [[GROUP BY]]
