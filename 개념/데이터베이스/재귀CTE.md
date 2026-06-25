---
date: 2026-06-10
tags: [재귀CTE, WITH_RECURSIVE, CTE, 계층형질의, 앵커, DBMS차이, SQL, 데이터베이스]
---

# 재귀 CTE (WITH RECURSIVE)

**정의**: **CTE(Common Table Expression)가 자기 자신을 참조**하며 반복 실행되는 구문. 계층형 데이터를 펼치는 표준 SQL 방식이며, Oracle `CONNECT BY`의 대체.

---

## 왜 필요한가

SQLite·SQL Server·MariaDB·MySQL에는 Oracle식 `CONNECT BY`가 없다. 대신 `WITH RECURSIVE`로 [[계층형질의]](조직도·트리)를 구현한다.

---

## 어떻게 작동하나

**앵커 쿼리(루트) → `UNION ALL` → 재귀 쿼리(직속 하위, lvl+1)** 구조. 더 이상 매칭이 없을 때까지 반복.

```sql
WITH RECURSIVE emp_hierarchy(
    EmployeeId, FirstName, LastName, Title, ReportsTo, lvl
) AS (
    -- Anchor: 최상위(Root)
    SELECT EmployeeId, FirstName, LastName, Title, ReportsTo, 0 AS lvl
    FROM employees
    WHERE ReportsTo IS NULL
    UNION ALL
    -- Recursive: 직속 부하 반복 탐색
    SELECT e.EmployeeId, e.FirstName, e.LastName,
           e.Title, e.ReportsTo, h.lvl + 1
    FROM employees e
    JOIN emp_hierarchy h ON e.ReportsTo = h.EmployeeId
)
SELECT lvl, EmployeeId, FirstName, ReportsTo
FROM emp_hierarchy
ORDER BY lvl, EmployeeId;
```

1. **앵커**: `ReportsTo IS NULL`인 루트를 lvl=0으로 시작.
2. **재귀**: 직전 단계 결과(`emp_hierarchy h`)와 employees를 조인해 직속 부하를 찾고 lvl+1.
3. 매칭이 없을 때까지 [[UNION ALL]]로 누적 → 종료.

---

## 실제 예시

- 보고 체계 전체 조회, 경로(PATH) 추적(`부모path || ' > ' || 이름`), Leaf/Branch 판별, 레벨별 인원수 집계.

---

## 자주 헷갈리는 것

**앵커와 재귀는 반드시 [[UNION ALL]]로 연결**한다(UNION 아님). 매 순환 결과를 누적해야 하므로 중복 제거 없는 UNION ALL이 맞다. 또 `SUBSTR(공백, 1, lvl*4)`로 들여쓰기를 흉내 내지만, 표시 환경에 따라 의도대로 안 보일 수 있다.

---

## 더 알면 좋은 것

📌 자기참조 데이터에 **사이클**이 있으면 종료 조건을 못 만나 무한 반복할 수 있다 → 루트가 NULL이고 사이클이 없다는 데이터 무결성이 전제. 지원 버전: SQL Server 2005+, MariaDB 10.2+, SQLite 전반.

---

## 관련 개념

- [[계층형질의]] — 상위 개념(Oracle CONNECT BY와 양대 구현)
- [[UNION ALL]] — 앵커+재귀 연결
- [[JOIN]] · [[집계함수]] — 계층 결과에 결합/집계
