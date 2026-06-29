---
date: 2026-06-18
tags: [Boolean인덱싱, pandas, 필터링, 데이터분석]
aliases: [Boolean인덱싱, Boolean indexing, 불리언인덱싱pandas, 조건필터링]
---

# Boolean indexing (조건 필터링)

**정의**: 비교연산이 만든 **True/False Series**(불리언 마스크)로 [[pandas]] 행을 필터링하는 방식. `df[조건]` 또는 `df.loc[조건, :]`. 어제 [[불리언인덱싱|NumPy 마스킹]]과 같은 원리.

---

## 왜 필요한가

"CPU 40 초과", "카테고리가 의료" 같은 조건 추출이 분석의 절반이다. SQL의 [[WHERE]]에 해당하며, 6/18 라이브에서 가장 비중이 컸다.

---

## 어떻게 작동하나

```python
df.loc[df['cpu'] > 40, :]                       # 단일조건
df.loc[(df['a']==1) & (df['b']==1), :]          # 복합 AND (괄호+&)
df.loc[df['x'].isin(['일간','주간']), :]         # isin
df.loc[df['stock'] < df['threshold'], :]        # ★ 컬럼 간 비교
```

- `df['cpu'] > 40`은 길이가 같은 **Boolean Series** → loc 첫 인자에 넣어 True인 행만 남긴다.
- 복합조건은 `and`/`or` 아닌 **비트 `&`,`|`,`~`** + **각 조건 괄호 필수**(연산자 우선순위).

---

## 자주 헷갈리는 것

**파이썬 `and`/`or`를 쓰면 에러.** 반드시 `&`·`|`·`~`. 또 괄호를 빼면(`df['a']==1 & df['b']==1`) 우선순위 때문에 틀린다 — **각 조건을 괄호로** 감싼다.

---

## 더 알면 좋은 것

📌 컬럼 간 비교(`df['a'] < df['b']`)는 행 단위 비교를 **True/False 한 줄로 축약**한다(중간과정 없이). 필터 후 `reset_index(drop=True)`+`copy()`로 결과 저장 권장.

---

## 관련 개념

- [[isin]] — 여러 값 중 하나 (긴 OR 대체)
- [[loc]] — 마스크를 넣는 접근자
- [[불리언인덱싱]] — NumPy 마스킹(동일 원리)
- [[reset_index]] — 필터 후 인덱스 초기화
