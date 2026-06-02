---
date: 2026-06-01
tags: [Git, GitHub, 버전관리, 협업, 도구]
---

# Git · GitHub

**정의**: **Git** = 코드의 변경 이력을 기록·관리하는 분산 버전관리 시스템. **GitHub** = Git 저장소를 인터넷에 올려두고 공유·협업하는 클라우드 서비스.

> 한 줄 구분: **Git = 도구(내 컴퓨터)**, **GitHub = 그 결과물을 올리는 사이트(클라우드)**.

---

## 왜 필요한가

- **되돌리기**: "어제는 됐는데 지금 안 돼" → 과거 시점으로 복구
- **이력 추적**: 누가·언제·무엇을·왜 바꿨는지 기록
- **협업**: 여러 명이 같은 코드를 충돌 없이 함께 작업
- **백업·배포**: GitHub에 올려 백업하고, GitHub Pages로 사이트 배포

---

## 어떻게 작동하나

### 기본 흐름 (가장 많이 쓰는 4단계)

```bash
# 1. 변경한 파일을 무대(staging)에 올림
git add .

# 2. 의미 있는 단위로 저장(스냅샷) + 메시지
git commit -m "로그인 기능 추가"

# 3. GitHub(원격)로 올림
git push

# 4. 원격의 최신 내용 받아오기
git pull
```

> 💡 **add → commit → push** 3단계가 핵심. add(고를 것 선택) → commit(내 PC에 기록) → push(GitHub에 업로드).

### 자주 쓰는 명령어

```bash
git init                 # 현재 폴더를 git 저장소로 시작
git clone <주소>          # 원격 저장소를 내 PC로 복제
git status               # 현재 변경 상태 확인
git log --oneline        # 커밋 이력 보기
git branch               # 브랜치 목록
git checkout -b feature  # 새 브랜치 만들고 이동
git merge feature        # 브랜치 합치기
```

### GitHub 핵심 개념

- **Repository(저장소)**: 프로젝트 단위 공간
- **Pull Request(PR)**: "내 변경을 합쳐주세요" 요청 → 리뷰 후 병합
- **Issue**: 할 일·버그 기록
- **GitHub Pages**: 저장소의 정적 파일을 웹사이트로 무료 배포

---

## 실제 예시

```bash
# 이 블로그도 git으로 배포한다 (Hugo → public/ → GitHub Pages)
git add .
git commit -m "deploy: 6/1 포스트"
git push origin main
```

> 우리 블로그 `yw8837.github.io`가 바로 GitHub Pages 배포다.

---

## 자주 헷갈리는 것

**add vs commit vs push**:
- `add` = 저장할 파일 **고르기**(무대에 올리기)
- `commit` = **내 PC에** 스냅샷 기록 (아직 GitHub엔 없음)
- `push` = GitHub로 **업로드**

**Git ≠ GitHub**: Git은 프로그램(도구), GitHub은 서비스(사이트). GitLab·Bitbucket 같은 대체 사이트도 있다.

**`.gitignore`**: git이 추적하지 않을 파일 목록(예: `.venv`, 비밀키). 올리면 안 되는 건 여기 적는다.

---

## 더 알면 좋은 것

📌 **브랜치(branch)**: 본 코드(main)를 건드리지 않고 따로 작업하는 가지. 완성되면 main에 merge.

📌 **merge conflict(충돌)**: 같은 부분을 둘이 다르게 고치면 발생. 직접 어느 쪽을 쓸지 정해 해결.

📌 **README.md**: 저장소 첫 화면 설명 문서. → [[마크다운]]으로 작성.

📌 **커밋 메시지 관례**: `feat:`(기능) `fix:`(수정) `docs:`(문서)처럼 접두어를 붙이면 이력이 깔끔.

---

## 관련 개념

- [[마크다운]] — README·이슈·PR이 전부 마크다운
- [[리눅스]] — git은 터미널에서 명령어로 사용
- [[VSCode]] — 에디터에서 git을 GUI로 편하게
