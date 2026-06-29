---
date: 2026-06-18
tags: [Series, pandas, 자료구조, 데이터분석]
aliases: [Series, 시리즈]
---

# Series (시리즈)

**정의**: [[pandas]]의 **1차원** 자료구조. **index + value** 한 쌍. [[DataFrame]]에서 단일 컬럼을 추출하면 Series가 된다. "엑셀 한 열".

---

## 왜 필요한가

한 변수(열) 단위 연산·집계의 기본 단위다. `value_counts()`·`mean()` 등 한 열에 대한 결과가 Series로 나오고, 이를 다시 필터·정렬·매핑한다.

---

## 어떻게 작동하나

```python
s = df['cpu_usage']         # 단일 컬럼 → Series
s.mean(); s.value_counts()  # 1차원 집계
df['region'].value_counts().index[0]   # 최빈값
```

- 비교연산 `s > 40`은 같은 길이의 **Boolean Series**를 만든다 → [[Boolean인덱싱]]의 마스크.

---

## 자주 헷갈리는 것

**클래스가 [[DataFrame]]과 달라 같은 메서드도 결과 형태가 다르다.** 예: `value_counts()`를 Series에 쓰면 Series, 다중집계는 DataFrame. 단일 컬럼이면 Series, 복수 컬럼·리스트면 DataFrame.

---

## 더 알면 좋은 것

📌 Series 내부도 [[NumPy]] [[ndarray]] + index. 그래서 [[벡터화]] 연산(`s * 2`, `np.sqrt(s)`)이 그대로 먹힌다.

---

## 관련 개념

- [[DataFrame]] — 2차원(복수 컬럼)
- [[Boolean인덱싱]] — 비교연산이 만드는 Boolean Series
- [[value_counts]] — Series 값 빈도
