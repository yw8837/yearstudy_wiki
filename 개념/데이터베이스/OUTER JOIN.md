---
date: 2026-06-11
tags: [OUTER_JOIN, LEFT_JOIN, RIGHT_JOIN, FULL_OUTER_JOIN, NULL, JOIN, SQL, 데이터베이스]
---

# OUTER JOIN (LEFT / RIGHT / FULL)

**정의**: 교집합에 더해 **한쪽(또는 양쪽)에만 있는 행도 보존**하는 [[JOIN]]. 매칭되는 짝이 없으면 반대편 컬럼은 NULL로 채운다. LEFT(왼쪽 보존) / RIGHT(오른쪽 보존) / FULL(양쪽 보존).

---

## 왜 필요한가

[[INNER JOIN]]은 매칭된 행만 남기므로 "앨범이 하나도 없는 아티스트", "주문이 없는 고객"처럼 **짝이 없는 행**은 사라진다. 그걸 봐야 할 때 OUTER JOIN으로 한쪽을 통째로 보존한다.

---

## 어떻게 작동하나

```sql
-- LEFT : 왼쪽 전부 + 매칭(없으면 NULL)
SELECT * FROM USER LEFT  [OUTER] JOIN CLASS ON USER.CLASS_ID = CLASS.CLASS_ID;
-- RIGHT : 오른쪽 전부
SELECT * FROM USER RIGHT [OUTER] JOIN CLASS ON USER.CLASS_ID = CLASS.CLASS_ID;
-- FULL : 양쪽 전부
SELECT * FROM CLASS FULL OUTER JOIN USER ON USER.CLASS_ID = CLASS.CLASS_ID;
```

**짝 없는 행 찾기** = OUTER JOIN + `IS NULL`:
```sql
SELECT ar.Name FROM artists ar
LEFT JOIN albums al ON ar.ArtistId = al.ArtistId
WHERE al.AlbumId IS NULL;          -- 앨범 없는 아티스트
```

---

## 실제 예시

- 주문 내역 없는 고객, 앨범 없는 아티스트, 담당 직원 없는 부서 등 "빈 쪽"을 찾는 분석.

---

## 자주 헷갈리는 것

**ON vs WHERE**: ON은 조인 조건이라 LEFT JOIN의 불일치 행을 NULL로 **남긴다**. WHERE는 조인 후 필터라 그 NULL 행을 **걸러낸다**. "짝 없는 행"은 `WHERE 오른쪽.키 IS NULL`. → [[NULL필터링]]

**Oracle `(+)` 구문**: WHERE에서 보존할 테이블 **반대쪽**에 `(+)`. `WHERE U.CLASS_ID = C.CLASS_ID (+)` = LEFT JOIN. 헷갈리니 표준 LEFT/RIGHT 권장.

**FULL OUTER 미지원 DB**: MySQL은 FULL 미지원 → `LEFT JOIN ... UNION ... RIGHT JOIN`으로 대체([[UNION]]이 중앙 중복 제거).

---

## 더 알면 좋은 것

📌 강사 권장 우선순위: **INNER / LEFT / RIGHT + SELF**. Oracle `(+)`·FULL은 개념만.

📌 SQLite는 **RIGHT·FULL OUTER = 3.39.0(2022-06-25)+** 부터 지원. 구버전이면 테이블 순서 바꿔 LEFT로 대체.

---

## 관련 개념

- [[JOIN]] · [[INNER JOIN]] — 비교
- [[LEFT JOIN]] — LEFT/RIGHT 상세
- [[NULL필터링]] — 짝 없는 행 찾기
- [[UNION]] — FULL OUTER 대체
