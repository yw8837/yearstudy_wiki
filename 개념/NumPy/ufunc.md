---
date: 2026-06-17
tags: [ufunc, 유니버설함수, NumPy, 원소별연산, exp, sqrt]
aliases: [ufunc, 유니버설 함수, universal function, 유니버설함수]
---

# 유니버설 함수 (ufunc)

**정의**: [[ndarray]]의 **모든 원소에 같은 수학 함수를 한 번에 적용**하는 NumPy 함수. `np.exp`·`np.sqrt`·`np.sin` 등.

---

## 왜 필요한가

원소마다 파이썬 루프로 함수를 돌리면 느리다. ufunc은 [[벡터화]]로 **C 레벨에서 원소별 연산**을 처리해 빠르다.

---

## 어떻게 작동하나

```python
import numpy as np
a = np.array([1, 4, 9])
np.sqrt(a)        # [1. 2. 3.]   원소별 제곱근
np.exp(a)         # 원소별 e^x
np.where(a > 3, 1, 0)   # 조건부 선택 [0 1 1]
np.clip(a, 2, 5)        # 범위로 자르기 [2 4 5]
```

```
[1 4 9] ──np.sqrt──▶ [1 2 3]   (원소별 일괄 적용)
```

---

## 실제 예시

- `np.exp`로 점수를 지수 변환, `np.clip`으로 이상치를 범위로 제한.

---

## 자주 헷갈리는 것

**ufunc(원소별) vs 집계(axis).** ufunc은 모양을 유지하며 원소별 적용, 집계(`sum`/`mean`)는 [[axis|축]]을 접어 차원을 줄인다.

---

## 관련 개념

- [[벡터화]] — ufunc이 빠른 이유
- [[브로드캐스팅]] — 다른 shape 간 ufunc 연산
- [[axis]] — 집계 함수와 비교
