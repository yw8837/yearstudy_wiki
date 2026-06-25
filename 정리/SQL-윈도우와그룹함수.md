---
date: 2026-06-12
tags: [SQL, 윈도우함수, 그룹함수, OVER, 순위함수, 누적합, LAG, LEAD, NTILE, ROLLUP, CUBE, 이탈고객, 정리]
aliases: [SQL 윈도우와 그룹함수, 윈도우함수 정리]
---

# SQL 윈도우 함수 & 그룹 함수 정리

> 출처: [[0612-그룹함수와윈도우함수]] · 상세: [[윈도우함수]] · [[OVER절]] · [[PARTITION BY]] · [[윈도우프레임]] · [[순위함수]] · [[집계윈도우]] · [[누적합]] · [[FIRST_VALUE LAST_VALUE]] · [[LAG LEAD]] · [[비율함수]] · [[NTILE]] · [[ROLLUP CUBE GROUPING_SETS]] · [[이탈고객분석]]

분석·통계 쿼리를 위한 두 함수 계열. **윈도우 함수**(행 유지하며 계산) + **그룹 함수**(소계·총계 레벨 추가)를 한눈에.

---

## 1. 윈도우 함수의 정체 → [[윈도우함수]]

```
GROUP BY  → 행 압축 (그룹당 1행)
윈도우     → 행 유지 + 계산 컬럼만 추가
```
- 행과 행의 관계를 정의, **`OVER` 절 필수**, 집계해도 원본 행 수 유지.
- "각 행 옆에 그룹 통계/순위/누적을 붙이고 싶다" → 윈도우.

---

## 2. OVER 절 3요소 → [[OVER절]] · [[PARTITION BY]] · [[윈도우프레임]]

```sql
OVER ( PARTITION BY 칸막이  ORDER BY 정렬  ROWS BETWEEN 시작 AND 끝 )
```
- **PARTITION BY**: 전체를 소그룹으로(생략 시 전체). "칸막이 치고 그 안에서 다시".
- **ORDER BY**: 순위·누적의 기준 정렬.
- **WINDOWING(ROWS/RANGE)**: 더할 행 범위. 생략 시 ORDER BY 있으면 기본 `처음~현재`.

---

## 3. 순위 함수 → [[순위함수]] · [[RANK DENSE_RANK ROW_NUMBER]]

```sql
RANK()       OVER (ORDER BY x DESC)   -- 1,2,3,3,5  (동률 건너뜀)
DENSE_RANK() OVER (ORDER BY x DESC)   -- 1,2,3,3,4  (안 건너뜀)
ROW_NUMBER() OVER (ORDER BY x DESC)   -- 1,2,3,4,5  (고유번호)
```
- 차이는 **동률 처리**뿐. PARTITION BY와 결합 → 그룹별 순위.
- ⚠ 윈도우 결과는 같은 WHERE로 못 거름 → **인라인뷰 감싸고 `WHERE rank=1`**.

---

## 4. 집계 + OVER → [[집계윈도우]]

```sql
AVG(Total) OVER (PARTITION BY BillingCountry)   -- 국가 평균을 각 행에
SUM(Total) OVER ()                              -- 전체합 (단, GrandTotal은 주의)
```
- 스칼라 서브쿼리보다 간결. GROUP BY 없이 그룹 통계를 행에 붙인다.

---

## 5. 누적합 / 이동평균 → [[누적합]] · [[이동평균]]

```sql
SUM(x) OVER (ORDER BY d ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)  -- 누적합
AVG(x) OVER (ORDER BY d ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)          -- 3행 이동평균
```
- 프레임을 `CURRENT ROW AND UNBOUNDED FOLLOWING`으로 → 잔여합(값 달라짐).
- "창의 너비"가 결과를 바꾼다.

---

## 6. 행 순서 함수 → [[FIRST_VALUE LAST_VALUE]] · [[LAG LEAD]]

```sql
FIRST_VALUE(x) OVER (PARTITION BY g ORDER BY x
    ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)  -- 첫(최소)값
LAST_VALUE(x)  OVER (... 위와 동일 프레임 ...)                  -- ⚠ 전체 창 명시 필수
LAG(x,1)  OVER (ORDER BY d)   -- 직전 행 (첫 행 NULL)
LEAD(x,1) OVER (ORDER BY d)   -- 다음 행 (끝 행 NULL)
```
- LAST_VALUE는 프레임을 `UNBOUNDED FOLLOWING`까지 명시 안 하면 현재 행 값이 나옴.
- LAG/LEAD → 전월대비(MoM), 재주문 간격, 이동평균.

---

## 7. 비율·등급 함수 → [[비율함수]] · [[NTILE]] · [[PERCENT_RANK CUME_DIST]]

```sql
x / SUM(x) OVER ()                          -- RATIO_TO_REPORT 대체(SQLite)
PERCENT_RANK() OVER (ORDER BY x DESC)        -- [0,1], 최고=0
CUME_DIST()    OVER (ORDER BY x DESC)        -- (0,1]
NTILE(3)       OVER (ORDER BY x DESC)        -- 1~3 등급
```

---

## 8. 그룹 함수 (차원 집계) → [[ROLLUP CUBE GROUPING_SETS]]

| 함수 | 레벨 | SQLite 대체 |
|:---|:---|:---|
| ROLLUP(A,B) | (A,B)+(A)+전체 = 3 | `UNION ALL` |
| CUBE(A,B) | +(B)별까지 = 4 | `UNION` |
| GROUPING SETS(A,B) | (A)별+(B)별 = 2 | `UNION ALL` |

- SQLite는 셋 다 미지원 → SELECT를 여러 번 UNION(소계 행은 컬럼 `NULL`).

---

## 9. 실전 분석 패턴 → [[이탈고객분석]] · [[strftime julianday]]

```sql
-- 이탈 후보: 마지막 주문 + 일수차 + HAVING
HAVING MAX(orderDate) < '2005-01-01'
CAST(julianday('2005-05-31') - julianday(MAX(orderDate)) AS INTEGER)
-- 재주문 간격: LAG(orderDate) + julianday 차, 첫 주문(NULL) 제외
```
- 월별 집계 = `strftime('%Y-%m', date)`. MoM = LAG로 전월값 빼기.

---

## 보충 — 표준/Oracle ↔ SQLite

| 항목 | 표준/Oracle | SQLite |
|:---|:---|:---|
| 윈도우 함수 | 지원 | **3.25.0+** |
| RATIO_TO_REPORT | 함수 있음 | `x / SUM(x) OVER()` |
| ROLLUP/CUBE/GROUPING SETS | 함수 있음 | **UNION 대체** |
| 전체합 GrandTotal | — | `SUM() OVER()`보다 [[스칼라서브쿼리]] 권장(강사 정정) |
| 연월/일수차 | `TO_CHAR` 등 | `strftime`, `julianday` |

---

## 관련

- [[0612-그룹함수와윈도우함수]] — 강의노트(라이브 진행순서)
- [[SQL-함수와서브쿼리]] · [[SQL-시작하기]] — 이전 SQL 정리
- [[GROUP BY]] · [[집계함수]] · [[HAVING]] · [[스칼라서브쿼리]] · [[UNION]] · [[UNION ALL]] · [[SELF JOIN]] — 토대 개념
