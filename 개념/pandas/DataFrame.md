---
date: 2026-06-18
tags: [DataFrame, pandas, 자료구조, 데이터분석]
aliases: [DataFrame, 데이터프레임]
---

# DataFrame (데이터프레임)

**정의**: [[pandas]]의 **2차원 표** 자료구조. 행 index + **복수 column**(각 열은 이름·dtype 보유). "엑셀 시트 전체".

---

## 왜 필요한가

분석 데이터는 대부분 행·열의 [[정형데이터]]다. DataFrame은 열마다 타입을 갖고 필터·집계·조인을 메서드로 제공해 표 데이터를 다루는 표준 그릇이 된다.

---

## 어떻게 작동하나

```python
df[['a','b']]            # 컬럼 리스트 선택 → DataFrame
df.loc[df['x']>0, :]     # 행 필터 → DataFrame
df.groupby('g').agg(['mean','max'])   # agg 복수집계 → DataFrame
```

- 단일 컬럼 `df['col']`은 [[Series]](1차원)지만, **리스트 `df[['col']]`는 DataFrame**(2차원). [[agg]] 복수집계처럼 결과가 복수 컬럼이면 DataFrame이 된다.

---

## 자주 헷갈리는 것

**`df['col']`(Series) vs `df[['col']]`(DataFrame).** 대괄호 한 겹은 1차원, 두 겹(리스트)은 2차원. 헷갈리면 `type()`으로 확인.

---

## 더 알면 좋은 것

📌 내부는 [[NumPy]] [[ndarray]]의 묶음. `df.to_numpy()`로 ndarray 변환 가능. [[axis]]=0(열 방향)/1(행 방향) 개념도 numpy와 동일.

---

## 관련 개념

- [[Series]] — 단일 컬럼(1차원)
- [[pandas]] — DataFrame을 제공하는 라이브러리
- [[loc]] · [[iloc]] — DataFrame 행·열 선택
