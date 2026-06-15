---
date: 2026-06-04
tags: [PyPI, 패키징, 배포, hatchling, twine, pyproject, uv, 라이브러리, 도구]
---

# PyPI 배포 (패키징)

**정의**: 내가 만든 파이썬 코드를 **패키지로 묶어 PyPI(Python Package Index)에 올려** 누구나 `pip install`로 쓸 수 있게 하는 것. 이 과정에선 `uv` + `hatchling`(빌드) + `twine`(업로드) 흐름으로 진행한다.

> 실습 예제: 강사가 실제 배포한 사칙연산 패키지 **`mathlib-evanjjh`**(3모듈, 실제 v0.1.5). → [[0604-라이브러리활용]]

---

## 왜 필요한가

좋은 코드를 매번 복사·붙여넣기 하지 않고, `pip install` 한 줄로 **재사용·공유**하기 위해서다. 공개 배포는 커뮤니티 기여이자 **포트폴리오·취업 경쟁력**이 된다(오픈소스 기여는 "잘 배우겠다"는 말보다 확실한 검증 근거). 개발 지망생은 작은 라이브러리 1개를 직접 배포해보는 게 좋은 출발점이다.

---

## 어떻게 작동하나

### 전체 흐름

```
0) PyPI 계정 + API 토큰 발급
1) GitHub repo 생성 (Public · .gitignore Python · MIT)
2) src 레이아웃 + __init__.py
3) uv init / uv add --dev pytest twine
4) pyproject.toml (hatchling) 메타정보
5) 코드 작성 + pytest 통과
6) ~/.pypirc 에 토큰 저장 (git 커밋 금지!)
7) version bump → twine upload dist/*
```

### 0) 계정 · API 토큰

- pypi.org 가입 → **Account settings > API tokens > Add API token**
- 토큰 문자열은 `pypi-...`로 시작하고 **1회만 표시** → 즉시 안전 보관.

### 2) src 레이아웃 — 배포명 vs import명

```
mathlib-evanjjh/            # 배포명: 하이픈(-)
├─ src/
│  └─ mathlib_evanjjh/      # import명(폴더명): 언더스코어(_)
│     ├─ __init__.py        # 패키지 표시 (필수)
│     ├─ arithmetic.py
│     ├─ geometric.py
│     └─ trigonometry.py
├─ tests/
│  └─ test_arithmetic.py
├─ pyproject.toml
└─ README.md
```

> ⚠️ **배포명은 하이픈, import명은 언더스코어.** `pip install mathlib-evanjjh` ↔ `import mathlib_evanjjh`. 폴더명(=import명)에 하이픈을 쓰면 import 불가.

### 3) 프로젝트 초기화

```bash
uv init .
uv add --dev pytest twine     # 개발 의존성: 테스트·업로드 도구
```

### 4) pyproject.toml (hatchling 빌드 백엔드)

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "mathlib-evanjjh"
version = "0.1.0"
authors = [{ name = "EvanJJH", email = "you@example.com" }]
description = "A class-based utility package for arithmetic, geometric sequences, and trigonometry"
readme = "README.md"
requires-python = ">=3.8"
```

### 6) `~/.pypirc` — 토큰 저장

홈 디렉터리에 `vi ~/.pypirc`로 작성(`i` 입력 → 작성 → `Esc` → `:wq`).

```ini
[pypi]
username = __token__
password = pypi-...        # 발급받은 토큰
```

### 7) 버전 bump → 업로드

```bash
uv run twine upload dist/*    # (또는 twine upload dist/*)
```

---

## 실제 예시

배포 후 누구나 설치·사용 가능(3모듈 전체):

```python
!pip install mathlib-evanjjh        # → mathlib_evanjjh-0.1.5 설치
from mathlib_evanjjh.arithmetic import Arithmetic
calc = Arithmetic(10, 2)
print(calc.add(), calc.divide())    # 12 5.0
```

> 실제 배포본은 점진적으로 올라가 **v0.1.5**까지 진행됐다. (`geometric`·`trigonometry`는 시그니처·출력만 확인됨, 내부 소스 미확보.)

---

## 자주 헷갈리는 것

**배포명(하이픈) vs import명(언더스코어)**: 설치는 `mathlib-evanjjh`, import는 `mathlib_evanjjh`. 가장 흔한 혼동.

**동일 버전 재업로드 불가**: PyPI는 같은 버전을 덮어쓸 수 없다. 코드를 바꿨으면 **반드시 `version`을 올려야**(bump) 다시 올라간다. `0.1.0 → 0.2.0 → …` 처럼 기능 추가마다 올린다(이 패키지는 실제로 0.1.5까지).

**테스트 통과 후 배포**: 배포 전 `pytest`가 전부 통과해야 한다. 깨진 코드를 올리지 않기 위한 관문. → [[pytest]]

**`.pypirc` 커밋 사고**: 토큰이 평문으로 들어 있어 git에 올라가면 즉시 노출된다. **반드시 `.gitignore`로 제외.** → [[API키보안]]

---

## 더 알면 좋은 것

📌 **빌드 도구 두 갈래**: 이 과정은 `uv` + **hatchling** + `twine`을 메인으로 쓴다. 흔히 보이는 `pip install build` → `python -m build` 흐름은 **대안**으로만 알아두면 된다.

📌 **TestPyPI**: 실수해도 안전한 연습용 저장소(test.pypi.org). 처음엔 여기 먼저 올려 흐름을 익히는 걸 권장.

📌 **시맨틱 버저닝**: `major.minor.patch`. 큰 변화=major, 기능추가=minor, 버그수정=patch.

📌 **라이브 시연 ≠ 실제 배포**: 강의 중 시연은 새 디바이스 인증 메일 문제로 실패했지만, 패키지는 사전/이후에 실제 PyPI에 배포되어 설치된다. 환경 문제는 흔하니 흐름 자체를 익히는 게 중요.

---

## 관련 개념

- [[파이썬라이브러리]] — 라이브러리·pip·PyPI 개요(배포의 상위 개념)
- [[모듈과패키지]] — src 레이아웃·`__init__.py`의 토대
- [[pytest]] — 배포 전 테스트 통과 관문
- [[uv]] — `uv init`·`uv add --dev`·`uv run`으로 빌드·배포
- [[Git]] — GitHub repo·.gitignore·MIT 라이선스
- [[API키보안]] — 토큰·`.pypirc` 노출 방지
