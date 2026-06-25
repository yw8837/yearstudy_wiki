---
date: 2026-06-09
tags: [JOIN, 조인, 다수테이블, 정규화, SQL, 데이터베이스]
---

# JOIN (테이블 결합)

**정의**: 여러 테이블의 정보를 **키로 연결해 한 번에 조회**하는 SQL. 정규화로 나눠둔 테이블을 다시 합친다.

---

## 왜 필요한가

데이터를 중복 없이 관리하려고 테이블을 나누면(정규화), 조회할 땐 다시 합쳐야 한다. JOIN이 그 역할.

**테이블을 나누는 이유**: ① 중복/저장 낭비 방지 ② 수정 시 일괄 관리(한 곳만 고치면 됨) ③ 삭제/무결성 문제 방지.

---

## 어떻게 작동하나

종류:

| 종류 | 결과 |
|:---|:---|
| [[INNER JOIN]] | 교집합(매칭되는 키만) |
| [[LEFT JOIN]] | 왼쪽 전부 + 매칭 NULL |
| RIGHT JOIN | 오른쪽 전부 (SQLite 3.39+) |

```sql
SELECT albums.Title, artists.Name
FROM albums INNER JOIN artists ON albums.ArtistId = artists.ArtistId;
```

`ON A.key = B.key`로 연결 조건을 준다.

---

## 실제 예시

- 3개 조인(Track→Album→Artist): Track↔Artist 직접 연결 불가 → **Album 경유**.

---

## 자주 헷갈리는 것

**컬럼명 충돌**: 두 테이블에 같은 컬럼명(`Name`)이 있으면 `table.column`으로 명시하거나 별칭(AS)으로 구분. 안 하면 `ambiguous column name` 오류.

**`SELECT *`는 확인용**: 조인 성공 여부 확인 뒤 필요한 컬럼으로 좁힌다(실무에선 `*` 금지에 가까움).

---

## 더 알면 좋은 것

📌 ERD/키를 먼저 확인하고 작성. 직접 연결 가능한 키가 없으면 중간 테이블을 경유해야 한다.

📌 "관계 없는 행"(매칭 실패)은 [[LEFT JOIN]] + [[NULL필터링]]으로 찾는다.

---

## 관련 개념

- [[INNER JOIN]] · [[LEFT JOIN]] — 종류
- [[NULL필터링]] — 매칭 실패 행 찾기
- [[관계형데이터베이스]] · [[기본키]] — 연결의 토대
