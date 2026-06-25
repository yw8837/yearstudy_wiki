---
date: 2026-06-11
tags: [EQUI_JOIN, Non_EQUI_JOIN, 등가조인, JOIN, SQL, 데이터베이스]
---

# EQUI JOIN / Non-EQUI JOIN

**정의**: [[JOIN]]을 **연산자 기준**으로 나눈 분류. **EQUI JOIN(등가 조인)** = `=`로 두 테이블의 정확히 일치하는 행을 결합. **Non-EQUI JOIN(비등가 조인)** = `=` 이외(`>`, `<`, `BETWEEN` 등)로 결합.

---

## 왜 필요한가

대부분의 조인은 "키가 같은 행을 붙이는" 등가 조인이다(PK-FK 관계). 하지만 "급여 구간표에 직원을 매핑"처럼 **정확히 같지 않고 범위로 매칭**해야 하는 경우엔 비등가 조인을 쓴다.

---

## 어떻게 작동하나

```sql
-- EQUI JOIN : = 로 일치하는 행 결합 (대부분의 조인)
SELECT * FROM albums a JOIN artists ar ON a.ArtistId = ar.ArtistId;

-- Non-EQUI JOIN : 범위/대소로 결합 (예: 급여→등급표)
SELECT e.NAME, g.GRADE
FROM EMPLOYEE e JOIN SALARY_GRADE g
  ON e.SALARY BETWEEN g.LOW AND g.HIGH;
```

| 분류 | 연산자 | 특징 |
|:---|:---|:---|
| EQUI | `=` | 주로 PK-FK 기반, 가장 흔함 |
| Non-EQUI | `>` `>=` `<` `<=` `BETWEEN` | 범위·대소 매칭, 빈도 낮음 |

---

## 실제 예시

- EQUI: `tracks.GenreId = genres.GenreId`로 트랙에 장르명 붙이기.
- Non-EQUI: 점수 → 등급, 가격 → 구간 같은 "구간표 매핑".

---

## 자주 헷갈리는 것

**EQUI = INNER JOIN?** 아니다. EQUI/Non-EQUI는 **연산자** 기준 분류, [[INNER JOIN]]/[[OUTER JOIN]]은 **FROM절 형태** 기준 분류. 한 쿼리가 "INNER이면서 EQUI"일 수 있다(서로 다른 축).

---

## 더 알면 좋은 것

📌 실무는 등가 조인이 대부분, 비등가 조인은 사용 빈도가 낮다(강사 언급).

---

## 관련 개념

- [[JOIN]] — 상위 개념
- [[INNER JOIN]] · [[OUTER JOIN]] — FROM절 형태 기준 분류(다른 축)
- [[관계형대수]] — 조인의 이론 토대
