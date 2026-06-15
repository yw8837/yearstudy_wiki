---
date: 2026-06-04
tags: [pytest, 테스트, 단위테스트, assert, 배포, 파이썬, 도구]
---

# pytest

**정의**: 파이썬에서 가장 널리 쓰는 **테스트 프레임워크**. `assert` 한 줄로 "기대한 결과가 맞는지" 자동 검사한다. 코드를 고친 뒤 깨진 게 없는지 빠르게 확인하고, **배포 전 통과 관문**으로 쓴다.

> 실습 맥락: `mathlib-evanjjh` 패키지 배포 전 `Arithmetic` 클래스를 테스트했다. → [[PyPI배포]]

---

## 왜 필요한가

코드를 바꾸면 "이게 아직 잘 동작하나?"를 매번 손으로 확인하긴 번거롭고 빠뜨리기 쉽다. 테스트를 한 번 짜두면 명령 한 줄로 **전부 자동 검증**된다. 특히 라이브러리 배포에서는 **테스트 통과 = 배포 가능**의 기준이 되어, 깨진 코드를 PyPI에 올리는 사고를 막는다.

---

## 어떻게 작동하나

### 기본 — `assert`

```python
# tests/test_arithmetic.py
from mathlib_evanjjh.arithmetic import Arithmetic

def test_add():
    assert Arithmetic(3, 2).add() == 5      # 같으면 통과, 다르면 실패
```

- `test_`로 시작하는 함수를 pytest가 자동으로 찾아 실행한다.
- `assert 조건` — 조건이 참이면 통과, 거짓이면 그 줄에서 실패를 보고.

### 클래스로 묶기 + 예외 검사 — `pytest.raises`

```python
import pytest
from mathlib_evanjjh.arithmetic import Arithmetic

class TestArithmetic:                 # Test로 시작하는 클래스도 자동 수집
    def test_subtract(self):
        assert Arithmetic(10, 4).subtract() == 6

    def test_divide(self):
        assert Arithmetic(10, 2).divide() == 5.0

    def test_divide_by_zero(self):
        with pytest.raises(ValueError):       # 이 블록에서 ValueError가 나야 통과
            Arithmetic(5, 0).divide()
```

> `pytest.raises(예외)`: "여기서 그 예외가 **발생해야** 정상"임을 검사한다. 0으로 나눌 때 `ValueError`가 제대로 터지는지 확인하는 식.

### 실행

```bash
uv run pytest          # (또는 그냥 pytest)
```

통과하면 점(`.`)으로 표시되고, 전부 통과해야 배포를 진행한다.

---

## 실제 예시

```bash
$ uv run pytest
collected 5 items
tests/test_arithmetic.py .....        [100%]
============== 5 passed ==============
```

> 5개 테스트(add·subtract·multiply·divide·divide_by_zero) 통과 → 배포 진행. → [[PyPI배포]]

---

## 자주 헷갈리는 것

**이름 규칙**: 파일은 `test_*.py`, 함수는 `test_*`, 클래스는 `Test*`. 이 규칙을 따라야 pytest가 자동으로 찾는다.

**`assert`만 쓰면 된다**: 자바 등과 달리 별도의 `assertEqual` 메서드가 필요 없다. 파이썬 기본 `assert`로 충분하고, pytest가 실패 시 좌우 값을 친절히 보여준다.

**정상 동작 vs 예외 동작**: 보통은 `assert 결과 == 기대값`, "에러가 나야 정상"인 경우만 `pytest.raises`로 감싼다.

---

## 더 알면 좋은 것

📌 **테스트 통과 = 배포 관문**: 라이브러리 배포 흐름에서 `pytest` 통과는 사실상 필수 단계. 깨진 코드를 올리지 않게 해준다.

📌 **개발 의존성으로 설치**: `uv add --dev pytest` — 테스트 도구는 배포 패키지엔 안 들어가는 개발용 의존성으로 둔다.

📌 **회귀 방지**: 한 번 짠 테스트는 이후 코드를 고칠 때마다 "예전 기능이 깨졌는지" 자동 확인하는 안전망이 된다.

---

## 관련 개념

- [[PyPI배포]] — 배포 전 테스트 통과 관문으로 사용
- [[클래스]] — 테스트 대상이 된 `Arithmetic` 클래스
- [[함수]] — 테스트 함수·`assert`의 토대
- [[uv]] — `uv add --dev pytest` · `uv run pytest`
