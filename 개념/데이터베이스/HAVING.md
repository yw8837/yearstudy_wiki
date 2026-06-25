---
date: 2026-06-09
tags: [HAVING, 그룹필터, GROUP_BY, SQL, 데이터베이스]
---

# HAVING (그룹 필터)

**정의**: [[GROUP BY]] 결과(그룹)에 **조건을 적용**하는 절. `... GROUP BY 컬럼 HAVING 집계조건;`

---

## 왜 필요한가

"앨범이 5개 이상인 아티스트", "매출 100 초과 국가"처럼 **그룹을 만든 뒤** 조건을 걸어야 할 때 쓴다. WHERE로는 집계 결과를 거를 수 없다.

---

## 어떻게 작동하나

```sql
SELECT user_id, COUNT(*) FROM rental
GROUP BY user_id
HAVING COUNT(user_id) > 1;            -- 그룹 후 조건

SELECT ArtistId, COUNT(*) FROM albums
GROUP BY ArtistId HAVING COUNT(*) >= 5;
```

- **단독 사용 불가** — 반드시 GROUP BY 이후에 온다.

---

## 실제 예시

- 국가별 매출 합계 중 100 초과: `... GROUP BY BillingCountry HAVING SUM(Total) > 100;`

---

## 자주 헷갈리는 것

**WHERE vs HAVING**: WHERE는 그룹 **전 행** 필터, HAVING은 그룹 **후 집계** 필터. 집계함수 조건은 HAVING에.

---

## 더 알면 좋은 것

📌 WHERE와 HAVING을 함께 쓸 수 있다: WHERE로 행을 먼저 줄이고 → GROUP BY → HAVING으로 그룹을 거른다(성능에도 유리).

---

## 관련 개념

- [[GROUP BY]] — 선행 그룹화
- [[집계함수]] — HAVING 조건에 쓰는 함수
- [[WHERE]] — 행 단위 필터
