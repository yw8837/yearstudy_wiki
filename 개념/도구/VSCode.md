---
date: 2026-06-01
tags: [VSCode, 에디터, 도구, IDE]
---

# VS Code (Visual Studio Code)

**정의**: Microsoft가 만든 **무료 코드 에디터**. 가볍지만 확장(Extension)으로 거의 모든 언어·작업을 지원한다. 개발자가 가장 많이 쓰는 에디터.

---

## 왜 필요한가

메모장으로도 코드를 쓸 순 있지만, VS Code는 **문법 색칠·자동완성·오류 표시·터미널·Git·디버깅**을 한 화면에서 제공한다. 글 쓰고(코드), 실행하고(터미널), 버전관리(Git)까지 도구를 옮겨 다니지 않고 처리할 수 있다.

---

## 어떻게 작동하나

- **내장 터미널**: `Ctrl + `` ` (백틱) → 에디터 안에서 바로 명령어 실행 (`uv run` 등)
- **확장(Extensions)**: 좌측 네모 아이콘 → Python, Jupyter, 한국어 팩 등 설치
- **Git 통합**: 좌측 소스 제어 탭에서 add·commit·push를 버튼으로
- **명령 팔레트**: `Ctrl + Shift + P` → 모든 기능 검색·실행

### 자주 쓰는 단축키

| 단축키 | 동작 |
|:---|:---|
| `Ctrl + `` ` | 터미널 열기/닫기 |
| `Ctrl + Shift + P` | 명령 팔레트 |
| `Ctrl + P` | 파일 빠르게 열기 |
| `Ctrl + /` | 주석 토글 |
| `Ctrl + S` | 저장 |

---

## 실제 예시

```text
1. 폴더 열기 (File → Open Folder)
2. Ctrl + ` 로 터미널 열기
3. uv run jupyter lab 실행
4. 소스 제어 탭에서 commit·push
```

> 강의 환경설정에서도 "VS Code 관리자로 실행 → 터미널에서 uv sync" 흐름을 썼다. → [[개발환경-설정]]

---

## 자주 헷갈리는 것

**VS Code ≠ Visual Studio**: 이름은 비슷하지만 다른 제품. VS Code는 가벼운 에디터, Visual Studio는 무거운 통합개발환경(주로 C#/.NET).

**확장을 너무 많이 깔면 느려짐**: 필요한 것만(Python, Jupyter 등) 설치.

---

## 더 알면 좋은 것

📌 **추천 확장**: Python, Jupyter, Korean Language Pack, GitLens, Prettier.

📌 **settings.json**: 에디터 설정을 코드로 관리. 들여쓰기·폰트·자동저장 등.

---

## 관련 개념

- [[Git]] — VS Code 소스 제어 탭에서 git 사용
- [[uv]] — 내장 터미널에서 `uv run`
- [[개발환경-설정]] — VS Code로 환경 세팅
