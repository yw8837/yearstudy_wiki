---
date: 2026-06-18
tags: [sort_values, pandas, 정렬, 데이터분석]
aliases: [sort_values, 값정렬]
---

# sort_values (값 정렬)

**정의**: [[DataFrame]]·[[Series]]를 **특정 컬럼 값 기준으로 정렬**하는 [[pandas]] 메서드. `df.sort_values(컬럼, ascending=, na_position=)`. 기본 오름차순.

---

## 왜 필요한가

상위/하위 n개(가장 바쁜 서버, 재고 적은 품목)를 보거나, 보고용으로 정렬할 때 쓴다. SQL [[ORDER BY]]에 대응.

---

## 어떻게 작동하나

```python
df.sort_values('cpu_usage', ascending=False).head(8)   # 내림차순 상위 8
df.sort_values('requested_quantity')                   # 기본 오름차순
df.sort_values(['location','check_date'])              # 다중키(리스트)
df.sort_values(['a','b'], ascending=[True, False])     # 키별 방향
```

- 다중키는 리스트로 주고, `ascending`을 리스트로 주면 키마다 방향 지정. 상위 n개는 `head` 조합.

---

## 자주 헷갈리는 것

**원본은 안 바뀐다**(새 정렬본 반환). 원본을 바꾸려면 `inplace=True` 또는 재대입. 인덱스 정렬은 `sort_index`(별개).

---

## 더 알면 좋은 것

📌 결측 위치는 `na_position='first'/'last'`로 제어. 정렬 후 인덱스를 0부터 다시 하려면 [[reset_index]].

---

## 관련 개념

- [[ORDER BY]] — SQL의 대응
- [[reset_index]] — 정렬 후 인덱스 초기화
- [[DataFrame]] — 정렬 대상
