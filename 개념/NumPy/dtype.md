---
date: 2026-06-17
tags: [dtype, NumPy, 자료형, 타입, astype]
aliases: [dtype, data type, 데이터타입, 넘파이 자료형]
---

# dtype (data type)

**정의**: [[ndarray]] 원소의 **자료형**. 한 배열의 모든 원소는 **동일한 dtype**을 가진다.

---

## 왜 필요한가

타입을 고정하면 메모리 크기·연산 방식이 정해져 **빠르고 예측 가능한 연산**이 된다(파이썬 [[리스트]]가 느린 이유의 반대).

---

## 어떻게 작동하나

| 분류 | 예 |
|:---|:---|
| 정수 | `int32`, `int64`(i8) |
| 실수 | `float32`, `float64`(f8) |
| 문자 | `U`(유니코드, 예 U32) |
| 불리언 | `bool` |

```python
import numpy as np
a = np.array([1, 2, 3])
a.dtype                 # int64
a.astype(float)         # [1. 2. 3.]  (타입 변환)
np.array([1, 2.0])      # float64  (섞이면 업캐스팅)
```

---

## 실제 예시

- `df.to_numpy(dtype=float)` — CSV 수치를 float64 배열로 적재.
- `np.zeros(3, dtype=int)` — 생성 시 dtype 지정.

---

## 자주 헷갈리는 것

**섞으면 업캐스팅.** `[1, 2.0]`은 int+float이라 전체가 float64가 된다. 단일 타입 규칙 때문.

---

## 관련 개념

- [[ndarray]] — dtype을 갖는 배열
- [[NumPy]] — 라이브러리
