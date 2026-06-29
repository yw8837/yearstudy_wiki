---
date: 2026-06-18
tags: [concat, pandas, 결합, 데이터분석]
aliases: [concat, 콘캣, 이어붙이기]
---

# concat (단순 결합)

**정의**: 여러 [[DataFrame]]을 **키 없이 축 방향으로 이어붙이는** [[pandas]] 함수. `pd.concat([d1, d2], axis=0)`.

---

## 왜 필요한가

같은 구조의 데이터를 위아래로 쌓거나(여러 달 로그 합치기), 같은 행을 좌우로 붙일 때 쓴다. 키 매칭이 필요 없는 단순 결합.

---

## 어떻게 작동하나

```python
pd.concat([df_a, df_b], axis=0, ignore_index=True)  # 상하(행 쌓기)
pd.concat([df_a, df_b], axis=1)                     # 좌우(열 붙이기)
pd.concat([df_a, df_b], join='inner')               # 공통 컬럼만
```

- `axis=0` 상하 / `axis=1` 좌우. `ignore_index=True`로 인덱스 재부여. `join='inner'/'outer'`로 컬럼 정합 방식.

---

## 자주 헷갈리는 것

**concat은 키 매칭을 안 한다**(단순 이어붙이기). 키로 결합하려면 [[merge]]. axis 방향과 `ignore_index`를 빠뜨리면 인덱스가 중복된다.

---

## 더 알면 좋은 것

📌 [[NumPy]]의 `vstack`/`hstack`/`concatenate`와 같은 발상. concat은 index/columns 정렬을 자동 처리한다는 점이 다르다.

---

## 관련 개념

- [[merge]] — 키 기준 결합(JOIN)
- [[reshape]] — NumPy 결합(vstack/hstack)
- [[DataFrame]] — 결합 대상
