---
date: 2026-06-18
tags: [reset_index, pandas, 인덱스, 데이터분석]
aliases: [reset_index, 인덱스초기화]
---

# reset_index (인덱스 초기화)

**정의**: 필터·정렬 뒤 **들쭉날쭉해진 행 인덱스를 0부터 다시** 부여하는 [[pandas]] 메서드. `df.reset_index(drop=True)`.

---

## 왜 필요한가

`df.loc[조건]`으로 거르면 원래 인덱스(3, 7, 12…)가 그대로 남아 보기 불편하고 위치 접근이 꼬인다. reset_index로 깔끔한 0,1,2…를 만든다.

---

## 어떻게 작동하나

```python
df.loc[df['flag']==1, :].reset_index(drop=True)   # 필터 후 인덱스 0부터
```

- `drop=True`: 기존 인덱스를 **버린다**. `drop=False`(기본): 기존 인덱스를 새 컬럼으로 보존.

---

## 자주 헷갈리는 것

**`drop=True`를 빼면** 기존 인덱스가 `index`라는 **새 열로 추가**된다. 보통 필터/정렬 후엔 `drop=True`를 쓴다.

---

## 더 알면 좋은 것

📌 [[GroupBy]] 결과의 MultiIndex를 평탄화할 때도 `reset_index()`를 쓴다(또는 `as_index=False`). 필터 결과 저장은 `.copy()`와 함께.

---

## 관련 개념

- [[Boolean인덱싱]] — 필터 후 자주 동반
- [[sort_values]] — 정렬 후 인덱스 정리
- [[GroupBy]] — 집계 결과 인덱스 평탄화
