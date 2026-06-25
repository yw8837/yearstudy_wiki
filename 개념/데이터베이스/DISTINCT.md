---
date: 2026-06-08
tags: [DISTINCT, 중복제거, 조회, SQL, 데이터베이스]
---

# DISTINCT

**정의**: 뒤에 오는 컬럼의 **중복을 제거**하고 보여주는 SQL 키워드. ("뚜렷한, 분명한")

---

## 왜 필요한가

"어떤 종류가 있는지"를 볼 때(국가 목록·단가 종류 등) 중복을 빼고 고유 값만 확인하기 위해서다.

---

## 어떻게 작동하나

```sql
SELECT DISTINCT Country FROM customers;        -- 국가 종류
SELECT DISTINCT City, Country FROM customers;  -- 도시+국가 조합 기준
```

⚠️ 컬럼을 **2개 이상** 적으면 **조합 기준**으로 중복을 제거한다. 한쪽 컬럼이 같아도 다른 쪽이 다르면 다른 행으로 취급.

---

## 실제 예시

- `SELECT DISTINCT UnitPrice FROM tracks;` — 존재하는 단가 종류만.

---

## 자주 헷갈리는 것

**여러 컬럼 DISTINCT**: 첫 컬럼만 중복 제거하는 게 아니라 **행 전체(조합)** 기준이다.

---

## 더 알면 좋은 것

📌 그룹별 개수가 필요하면 DISTINCT보다 [[GROUP BY]] + COUNT가 적합할 때가 많다.

---

## 관련 개념

- [[SELECT]] — 함께 쓰는 명령
- [[GROUP BY]] — 그룹 단위 집계
