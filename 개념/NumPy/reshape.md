---
date: 2026-06-17
tags: [reshape, NumPy, 배열변형, newaxis, 결합, 분할, shape]
aliases: [reshape, 리셰이프, 배열 변형, 형태 변경]
---

# reshape (배열 형태 변경)

**정의**: [[ndarray]]의 **원소 개수를 보존하면서 [[shape]]를 바꾸는** 연산. 결합·분할·축 추가와 함께 배열 모양을 다룬다.

---

## 왜 필요한가

분석·모델 입력은 특정 차원 형태를 요구한다(예: 1차원 → (n,1) 열벡터). reshape로 데이터를 **원하는 모양**으로 맞춘다.

---

## 어떻게 작동하나

```python
import numpy as np
a = np.arange(6)
a.reshape(3, 2)        # [[0 1][2 3][4 5]]   (6 = 3×2, 원소 수 보존)
a[:, np.newaxis]       # (6,1) 열벡터
a[np.newaxis, :]       # (1,6) 행벡터
```

- **원소 개수 보존** 필수: 좌·우변 곱이 같아야 함(불일치 시 오류).
- 축 추가: `np.newaxis`/`np.expand_dims`. 전치: `a.T`.

```python
# 결합 / 분할
np.vstack((a1, a2))    # 행 방향 쌓기
np.hstack((a1, a2))    # 열 방향 붙이기
np.stack((a1, a2))     # 새 축으로 쌓기
np.split(m, [idx], axis=0)
```

---

## 실제 예시

- `np.arange(12).reshape(3, 4)` — 1차원 0..11을 3×4 표로.

---

## 자주 헷갈리는 것

**개수가 안 맞으면 에러.** `np.arange(6).reshape(4, 2)`는 6≠8이라 오류. `-1`을 한 축에 쓰면 자동 계산(`reshape(3, -1)`).

---

## 관련 개념

- [[shape]] — 변경 대상
- [[ndarray]] — 변형 대상 배열
- [[axis]] — 결합/분할의 기준 축
