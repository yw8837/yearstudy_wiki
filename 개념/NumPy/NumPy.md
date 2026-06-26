---
date: 2026-06-17
tags: [NumPy, Numerical Python, 배열, 수치연산, 데이터분석, 라이브러리]
aliases: [NumPy, numpy, Numerical Python, np]
---

# NumPy (Numerical Python)

**정의**: Python에서 **대규모 다차원 배열([[ndarray]])과 수치 연산**을 빠르게 다루는 라이브러리. 데이터 분석 패키지의 **뼈대**.

---

## 왜 필요한가

데이터의 대부분은 숫자 배열로 볼 수 있다. 파이썬 [[리스트]]는 유연하지만 느리고 메모리도 비효율적이다. NumPy는 **C 레벨 [[벡터화]]**로 빠르고 메모리 효율적인 배열 연산을 제공한다.

```
파이썬 list: 객체 포인터들의 모음 (느림)
NumPy ndarray: 같은 타입 값이 연속 메모리 (빠름)
```

---

## 어떻게 작동하나

```python
import numpy as np
a = np.array([1, 2, 3, 4, 5, 6])   # 리스트 → ndarray
a.dtype     # int64   (단일 타입)
a + 5       # 모든 원소에 +5 (벡터화·브로드캐스팅)
```

- 핵심 타입은 [[ndarray]](단일 [[dtype]], [[shape]] 보유).
- pandas·seaborn·scipy·statsmodels·scikit-learn이 **모두 NumPy 기반**.

---

## 실제 예시

- CSV → `to_numpy()` → [[ndarray]] → 표준화·필터·[[브로드캐스팅]] 연산으로 분석 전처리.

---

## 자주 헷갈리는 것

**언제 직접 쓰나?** 서비스 구현은 보통 라이브러리 활용 중심. NumPy를 직접 깊게 쓰는 건 **새 알고리즘을 개발할 때**가 대표적.

---

## 더 알면 좋은 것

📌 코랩은 분석용 기본 세팅이라 무리한 버전 업그레이드(pip)는 환경이 꼬일 수 있어 기본 환경 권장.

---

## 관련 개념

- [[ndarray]] · [[dtype]] · [[shape]] — 기본 구조
- [[브로드캐스팅]] · [[벡터화]] — 핵심 연산 원리
- [[리스트]] — 비교 대상(파이썬 기본 시퀀스)
