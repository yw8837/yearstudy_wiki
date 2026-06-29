---
date: 2026-06-18
tags: [isin, pandas, 필터링, 데이터분석]
aliases: [isin, isin필터]
---

# isin (포함 여부 필터)

**정의**: 값이 **주어진 리스트 중 하나인지** 검사해 Boolean Series를 만드는 [[pandas]] 메서드. `df['col'].isin([...])`. 긴 OR 조건을 한 줄로 대체.

---

## 왜 필요한가

`(df['t']=='일간') | (df['t']=='주간')`처럼 OR가 길어지면 가독성이 떨어진다. `df['t'].isin(['일간','주간'])`이 같은 일을 짧고 명확하게 한다. SQL [[IN연산자|IN]]과 동일.

---

## 어떻게 작동하나

```python
df.loc[df['check_type'].isin(['일간','주간']), :]
df.loc[df['issue'].isin(['CPU과부하','디스크']), :]
df.loc[~df['cat'].isin(['의료']), :]      # ~ 로 부정(NOT IN)
```

- 결과는 [[Boolean인덱싱|Boolean Series]] → [[loc]]에 넣어 필터. 부정은 앞에 `~`.

---

## 자주 헷갈리는 것

**부정은 `!=`가 아니라 `~ ... isin(...)`.** 여러 값 제외는 `~df['c'].isin([...])`로 쓴다.

---

## 더 알면 좋은 것

📌 SQL의 [[IN연산자]]와 같은 의도. 단 NULL 처리는 다르므로 결측이 섞이면 결과를 확인.

---

## 관련 개념

- [[Boolean인덱싱]] — isin이 만드는 마스크
- [[IN연산자]] — SQL의 대응 연산자
- [[loc]] — 필터 적용 접근자
