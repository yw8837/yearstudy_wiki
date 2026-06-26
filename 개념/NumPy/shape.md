---
date: 2026-06-17
tags: [shape, ndim, size, NumPy, 배열속성, 차원]
aliases: [shape, ndim, size, 배열 형태, 넘파이 shape]
---

# shape / ndim / size (배열 속성)

**정의**: [[ndarray]]의 **형태 정보**. `shape`=각 축 길이(튜플), `ndim`=축 개수, `size`=원소 총개수.

---

## 왜 필요한가

연산·[[reshape]]·[[브로드캐스팅]]·[[axis|축]] 집계는 모두 **형태(shape)에 의존**한다. shape를 정확히 읽어야 차원 오류 없이 배열을 다룰 수 있다.

---

## 어떻게 작동하나

```python
import numpy as np
a = np.arange(12).reshape(3, 4)
a.shape       # (3, 4)   각 축 길이
a.ndim        # 2        축 개수 = len(shape)
a.size        # 12       원소 총수 = shape 곱(3×4)
a.itemsize    # 8        원소 1개 바이트
a.nbytes      # 96       전체 바이트 ≈ size×itemsize
```

```
shape=(3, 4)
  axis0 길이 3 (행)
  axis1 길이 4 (열)
```

---

## 실제 예시

- 3차원 `shape=(3,2,4)` → `size=24`. "페이지 3장이 쌓인 것"처럼 첫 축이 블록.

---

## 자주 헷갈리는 것

**`size` vs `len`.** `size`는 전체 원소 수(shape 곱), `len(a)`는 첫 축 길이만. 헷갈리면 `size` 사용.

---

## 관련 개념

- [[ndarray]] — 형태를 갖는 배열
- [[reshape]] — shape 변경 / [[axis]] — 축 기준 연산
