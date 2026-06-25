---
date: 2026-06-11
tags: [NATURAL_JOIN, USING, ON, INNER_JOIN, JOIN, SQL, 데이터베이스]
---

# NATURAL JOIN / USING / ON

**정의**: [[INNER JOIN]]에서 **조인 조건을 거는 3가지 방식.** **ON** = 임의 조건 직접 명시 / **USING(컬럼)** = 같은 이름 컬럼 일부 선택 / **NATURAL JOIN** = 같은 이름 컬럼 전체 자동 등가 조인.

---

## 왜 필요한가

조인하려면 "어떤 컬럼이 같을 때 붙일지"를 알려줘야 한다. 컬럼명이 다르면 ON으로 직접, 같으면 USING/NATURAL로 짧게 쓸 수 있다. 다만 USING·NATURAL은 제약이 많아 실무에선 ON을 주로 쓴다.

---

## 어떻게 작동하나

```sql
-- ON : 컬럼명이 달라도 조건 자유 지정 (가장 일반적)
SELECT * FROM 테이블1 JOIN 테이블2 ON 테이블1.컬럼A = 테이블2.컬럼B;

-- USING : 양쪽에 같은 이름인 컬럼 중 일부만 골라 등가 조인
SELECT * FROM 테이블1 JOIN 테이블2 USING(기준칼럼);

-- NATURAL : 같은 이름인 컬럼 전부 자동으로 등가 조인
SELECT * FROM 테이블1 NATURAL JOIN 테이블2;
```

| 방식 | 조인 조건 | 별칭 | 추가조건(WHERE/ON) | 비고 |
|:---|:---|:---:|:---:|:---|
| ON | 직접 명시(컬럼명 달라도 OK) | OK | OK | 표준·실무 기본 |
| USING | 동일 컬럼명 일부 선택 | **금지** | OK | SQL Server 미지원 |
| NATURAL | 동일 컬럼명 전부 자동 | **금지** | **불가** | 제약 많음 |

---

## 실제 예시

- 컬럼명이 양쪽 모두 `ArtistId`라면 `JOIN artists USING(ArtistId)`로 간결하게.
- 하지만 의도치 않은 동일 컬럼명까지 묶이는 사고를 막으려면 ON이 안전.

---

## 자주 헷갈리는 것

**USING·NATURAL은 별칭 금지**: USING/NATURAL로 묶은 컬럼은 테이블 별칭으로 수식할 수 없다(`a.컬럼` 불가). NATURAL은 추가 조인 조건(ON/USING/WHERE)도 정의 못 한다.

**NATURAL의 위험**: 우연히 이름만 같은 컬럼(예: 양쪽 `Name`)까지 조인 조건에 끌려 들어가 결과가 틀어질 수 있다 → 실무 회피.

---

## 더 알면 좋은 것

📌 강사 권장: USING/NATURAL은 **개념만 경험**하고 실제로는 **ON 중심**으로 작성. SQLite는 셋 다 지원(USING/NATURAL 포함).

---

## 관련 개념

- [[INNER JOIN]] — 이 조건 방식들이 쓰이는 조인
- [[JOIN]] · [[EQUI JOIN]] — 상위/연관 분류
