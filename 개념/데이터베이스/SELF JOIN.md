---
date: 2026-06-11
tags: [SELF_JOIN, 셀프조인, 계층형, 자기참조, JOIN, SQL, 데이터베이스]
---

# SELF JOIN (셀프 조인)

**정의**: **같은 테이블을 자기 자신과 조인.** 테이블·컬럼 이름이 모두 같으므로 별칭 2개로 분리해 구분한다(**별칭 필수**). 계층형 데이터나 같은 집합 내 쌍 비교에 쓴다.

---

## 왜 필요한가

한 테이블 안에 관계가 들어 있을 때 쓴다. ① **계층형**(직원-관리자: `employees.ReportsTo`가 같은 테이블의 `EmployeeId`를 가리킴), ② **쌍 비교**(같은 앨범의 트랙끼리, 같은 국가 고객끼리).

---

## 어떻게 작동하나

```sql
-- 직원 ↔ 관리자 (자기참조 FK)
SELECT e.FirstName AS 직원, m.FirstName AS 관리자
FROM employees e
LEFT JOIN employees m ON e.ReportsTo = m.EmployeeId;   -- 같은 employees 두 별칭

-- 같은 앨범 트랙 쌍 비교 (공통 그룹키)
SELECT t1.Name AS A, t2.Name AS B
FROM tracks t1
JOIN tracks t2
  ON t1.AlbumId = t2.AlbumId        -- 같은 앨범끼리
 AND t1.TrackId < t2.TrackId;       -- 중복쌍·자기쌍 제거
```

---

## 실제 예시

- 차상위 관리자: `employees`를 3번(e/m/g) 셀프 조인해 직원→직속관리자→그 위 관리자를 한 줄에.
- 같은 국가 고객 쌍, 재생시간이 비슷한 트랙 쌍.

---

## 자주 헷갈리는 것

**자기참조 FK가 없어도 가능**: 전형적 SELF JOIN은 `ReportsTo` 같은 자기참조 FK에서 자연스럽지만, **공통 그룹키(AlbumId, Country)** 로 묶어 쌍을 비교하는 것도 SELF JOIN이다. 조인 키가 꼭 자기참조 FK일 필요는 없다(라이브 Q&A 핵심).

**`키1 < 키2` 트릭**: `t1.TrackId < t2.TrackId` 조건이 ① 자기 자신과의 쌍(A-A)과 ② 순서만 다른 중복쌍(A-B / B-A)을 한 번에 없앤다. `<` 대신 `<>`만 쓰면 (A,B),(B,A) 둘 다 나온다.

---

## 더 알면 좋은 것

📌 계층을 **깊이 제한 없이** 펼치려면 SELF JOIN 대신 [[재귀CTE]](`WITH RECURSIVE`)를 쓴다. SELF JOIN은 조인한 횟수만큼의 단계만 펼친다(2단계면 직속+차상위까지).

📌 강사 권장 학습범위에 SELF JOIN 포함(INNER/LEFT/RIGHT + SELF).

---

## 관련 개념

- [[JOIN]] · [[INNER JOIN]] · [[LEFT JOIN]] — 토대
- [[계층형질의]] · [[재귀CTE]] — 깊은 계층 펼치기
- [[CROSS JOIN]] — 조건 없는 쌍(대비)
