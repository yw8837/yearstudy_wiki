---
date: 2026-06-17
tags: [ndarray, NumPy, 배열, dtype, shape, 데이터분석]
aliases: [ndarray, N-dimensional array, NumPy 배열, 넘파이배열]
---

# ndarray (N-dimensional array)

**정의**: [[NumPy]]의 핵심 자료형. **같은 [[dtype]]의 원소들이 연속 메모리에 담긴 다차원 배열**.

---

## 왜 필요한가

파이썬 [[리스트]]는 타입이 섞일 수 있어 느리다. ndarray는 **단일 타입 + 연속 메모리**라 [[벡터화]] 연산이 C 레벨에서 빠르게 돈다.

---

## 어떻게 작동하나

```python
import numpy as np
a = np.array([1, 2, 3])               # 1차원
b = np.array([[1,2,3],[4,5,6]])       # 2차원 (2행 3열)
b.shape    # (2, 3)
b.ndim     # 2
b.dtype    # int64
```

- 속성: [[shape]](형태)·`ndim`(축 수)·`size`(원소 수)·[[dtype]](타입).
- **단일 dtype** — int/float를 섞으면 float로 업캐스팅된다.

```
2차원 ndarray (shape=(2,3))
  [[1 2 3]      행(axis 0)
   [4 5 6]]     열(axis 1)
```

---

## 실제 예시

- 정형 CSV 한 장 = (행, 열) 2차원 ndarray. 이미지 한 장 = (높이, 너비, 채널) 3차원.

---

## 자주 헷갈리는 것

**ndarray ≠ 파이썬 리스트.** 슬라이싱이 [[뷰와복사|뷰]]를 반환(리스트는 복사), 원소 타입이 단일(리스트는 자유)이라는 점이 다르다.

---

## 관련 개념

- [[NumPy]] — ndarray를 제공하는 라이브러리
- [[dtype]] · [[shape]] — ndarray의 핵심 속성
- [[리스트]] — 비교 대상
