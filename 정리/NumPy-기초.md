---
date: 2026-06-17
tags: [NumPy, ndarray, dtype, shape, 인덱싱슬라이싱, 뷰vs복사, reshape, 불리언인덱싱, axis, ufunc, 브로드캐스팅, 벡터화, 정리, 요약, 치트시트]
---

# NumPy 기초 정리

분석 패키지(pandas·scikit-learn 등)의 **뼈대**. 6/17 후반부 실습을 섹션별로 한눈에.

> 강의 출처: [[0617-데이터리터러시와NumPy기초]] · 상세: [[ndarray]] · [[뷰와복사]] · [[브로드캐스팅]] · [[axis]]

---

## 1. 배열 생성·속성 → [[ndarray]] · [[dtype]] · [[shape]]

```python
import numpy as np
a = np.array([1, 2, 3])          # 리스트 → ndarray (단일 dtype)
np.zeros(2); np.ones(2)          # 기본 float64
np.arange(2, 9, 2)               # [2 4 6 8]
np.linspace(0, 10, num=5)        # [0. 2.5 5. 7.5 10.]  (양끝 포함)
```

- 한 배열은 **단일 [[dtype]]**(int/float 섞이면 float 업캐스팅). `astype()`로 변환.
- 속성: `ndim`(축수)·`shape`(축별 길이)·`size`(원소수)·`itemsize`·`nbytes`. → [[shape]]

---

## 2. 인덱싱·슬라이싱·뷰 vs 복사 → [[인덱싱슬라이싱]] · [[뷰와복사]]

```python
a = np.array([10, 2, 3, 4, 5, 6])
a[3:]              # 슬라이스 = 뷰 (원본과 메모리 공유)
b = a[3:]; b[0]=40 # → 원본 a 도 바뀜!
c = a[3:].copy()   # 독립 복사본
```

- ★ **슬라이싱은 항상 뷰**(원본 공유), **불리언·정수배열(고급) 인덱싱은 항상 복사**.
- `flatten`=항상 복사, `ravel`=가능하면 뷰. → [[뷰와복사]]

---

## 3. reshape·결합·분할 → [[reshape]]

```python
np.arange(6).reshape(3, 2)       # 원소 수 보존 (6 = 3×2)
a[:, np.newaxis]                 # 열벡터 (6,1)
np.vstack((a1, a2))              # 행 방향 쌓기
np.hstack((a1, a2))              # 열 방향 붙이기
np.concatenate((a, b), axis=0)
```

- [[reshape]]는 좌·우변 원소 개수가 같아야 함. `stack`은 **새 축**으로 쌓음.

---

## 4. 불리언(마스킹) 인덱싱 → [[불리언인덱싱]]

```python
a = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
a[a < 5]                  # [1 2 3 4]
a[(a > 2) & (a < 11)]     # 괄호 필수! & AND, | OR
a[a > 8] = 0              # 마스크 위치 일괄 대입
```

- Pandas 필터링과 **동일 원리**. 마스크는 **bool 배열**이라야 함(정수 배열은 fancy 인덱싱).

---

## 5. 연산·집계(axis)·ufunc → [[axis]] · [[ufunc]]

```python
b = np.array([[1,1],[2,2]])
b.sum()          # 6
b.sum(axis=0)    # [3 3]   축0 접기 = 열 합
b.sum(axis=1)    # [2 4]   축1 접기 = 행 합
np.exp(b); np.sqrt(b)            # ufunc (원소별)
np.where(b > 1, 1, 0); np.clip(b, 0, 1)
```

- 같은 shape이면 `+ - * /`는 **요소별**(행렬 곱은 `@`). [[axis]] 개념은 판다스에서 그대로 반복.

---

## 6. 브로드캐스팅·벡터화 (★핵심) → [[브로드캐스팅]] · [[벡터화]]

```python
data = np.array([1.0, 2.0]);  data * 1.6        # 스칼라 확장
np.array([[1,2],[3,4],[5,6]]) + np.array([[1,1]])  # (3,2)+(1,2)
np.arange(3).reshape((3,1)) + np.arange(3)      # (3,1)+(3,) → (3,3)
```

- [[브로드캐스팅]]: shape를 **뒤에서부터 비교**, 같거나 **한쪽이 1**이면 호환(1인 축 확장).
- [[벡터화]]: 파이썬 루프 대신 배열 연산 → C 레벨 처리로 **수십~수백 배** 빠름.

---

## 7. 정형 데이터 가공 (실무 패턴)

```python
X = df.to_numpy(dtype=float)               # CSV → ndarray (pandas는 1회만)
ok = ~np.any(np.isnan(X), axis=1); X[ok]   # 결측 행 제거
Z = (X - X.mean(0)) / X.std(0, ddof=0)     # z-score (브로드캐스팅, 열별)
cat, codes = np.unique(labels, return_inverse=True)   # 범주 → 0..K-1
np.argsort(-col)                           # 내림차순 순위
```

- `np.std`의 ddof: z-score는 ddof=0(N), 표본표준편차/추론통계는 ddof=1(N−1).

---

## 관련

- [[0617-데이터리터러시와NumPy기초]] — 원본 강의 노트
- [[데이터리터러시-기초]] — 같은 회차 전반부(이론)
- [[ndarray]] · [[dtype]] · [[shape]] · [[뷰와복사]] · [[reshape]] · [[불리언인덱싱]] · [[axis]] · [[ufunc]] · [[브로드캐스팅]] · [[벡터화]] — 개념 상세
- [[인덱싱슬라이싱]] · [[리스트]] — 파이썬 인덱싱과 비교
