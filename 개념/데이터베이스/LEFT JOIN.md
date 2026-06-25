---
date: 2026-06-09
tags: [LEFT_JOIN, RIGHT_JOIN, JOIN, NULL, SQL, 데이터베이스]
---

# LEFT JOIN / RIGHT JOIN

**정의**: **LEFT JOIN** = 왼쪽 테이블의 모든 행을 출력하고, 매칭되는 오른쪽 값을 붙이되 없으면 NULL. **RIGHT JOIN** = 오른쪽 테이블 기준.

---

## 왜 필요한가

"앨범이 하나도 없는 아티스트"처럼 **매칭 실패 행까지 포함**해서 봐야 할 때 쓴다. [[INNER JOIN]]은 매칭된 것만 남기지만, LEFT JOIN은 한쪽을 전부 보존한다.

---

## 어떻게 작동하나

```sql
SELECT * FROM user LEFT  JOIN rental ON user.id = rental.user_id;  -- 왼쪽(user) 전부
SELECT * FROM user RIGHT JOIN rental ON user.id = rental.user_id;  -- 오른쪽(rental) 전부
```

- INNER = 겹치는 부분만, LEFT = 왼쪽 + 겹침, RIGHT = 오른쪽 + 겹침.
- 매칭 실패 시 반대편 컬럼은 NULL → [[NULL필터링]]으로 "관계 없는 행"을 찾는다.

---

## 실제 예시

```sql
-- 앨범 없는 아티스트만
SELECT artists.Name
FROM artists LEFT JOIN albums ON artists.ArtistId = albums.ArtistId
WHERE albums.AlbumId IS NULL;
```

---

## 자주 헷갈리는 것

**RIGHT보다 LEFT 권장**: 두 테이블 조인이면 **테이블 순서를 바꿔** RIGHT 효과를 LEFT로 대체할 수 있다(강사 권장).

**RIGHT JOIN 지원**: SQLite는 **3.39.0 이상**에서만 RIGHT JOIN 동작.

---

## 더 알면 좋은 것

📌 다중 테이블 조인에선 RIGHT가 필요할 수 있다(추후 학습 범위).

---

## 관련 개념

- [[JOIN]] · [[INNER JOIN]] — 비교
- [[NULL필터링]] — 매칭 실패 행 찾기
