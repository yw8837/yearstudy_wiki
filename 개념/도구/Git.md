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

### 🚀 처음 시작하기 (첫 실습 흐름: repo 생성 → clone → config → push)

GitHub를 처음 쓸 때 한 번 거치는 전체 흐름이다.

```bash
# 1) GitHub에서 New Repository 생성 (웹)
#    프로필 > Repositories > New
#    - .gitignore: Python 선택  (파이썬 프로젝트면 .venv 등 자동 무시)
#    - License: 교육·공유 목적이면 MIT 권장 (인용 조건 전제)
#    - 공개 범위: Private(공유 원치 않음) / Public(패키지 등 공유 목적)

# 2) clone — 원격 저장소를 내 PC로 복제
#    Code > HTTPS 탭에서 주소 복사
#    (작업 위치: Windows는 C드라이브, Mac은 Desktop 등 → 터미널로 이동 후)
git clone https://github.com/yourname/your-repo.git

# 3) config — 커밋에 남길 본인 신원 등록 (최초 1회, --global)
git config --global user.email "you@example.com"
git config --global user.name "Your Name"

# 4) add → commit → push
git add .
git commit -m "my first commit"
git push
```

> ✅ **정상 동작 확인**: GitHub 레포를 새로고침했을 때 커밋 내용이 보이고, 터미널에 빨간 에러·경고 없이 `main` 브랜치로 push 완료가 뜨면 성공.
> ⚠️ **흔한 실수**: `git config`에 **강사(또는 남)의 이메일·이름을 그대로 따라 치는 것**. 반드시 본인 계정 정보로 설정하라.

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

**환경 오류가 안 풀릴 때**: 개인 PC 환경 오류는 원인 파악이 어렵다. 검색·질문으로 해결을 시도하고, **최후의 수단은 레포를 새로 만들어 처음부터 다시** 진행하는 것이다(첫 실습 단계에선 이게 더 빠를 때가 많다).

---

## 더 알면 좋은 것

📌 **브랜치(branch)**: 본 코드(main)를 건드리지 않고 따로 작업하는 가지. 완성되면 main에 merge.

📌 **merge conflict(충돌)**: 같은 부분을 둘이 다르게 고치면 발생. 직접 어느 쪽을 쓸지 정해 해결.

📌 **README.md**: 저장소 첫 화면 설명 문서. → [[마크다운]]으로 작성.

📌 **커밋 메시지 관례**: `feat:`(기능) `fix:`(수정) `docs:`(문서)처럼 접두어를 붙이면 이력이 깔끔.

📌 **개발 루틴**: 강사 왈 — 개발자는 GitHub에 일상적으로 접속하는 수준으로 자주 쓴다. git/환경 세팅은 매일 반복 학습으로 손에 익히는 게 목표.

📌 **단일 파일 100MB 초과 시 `git push` 실패**: GitHub는 파일당 용량 제한이 있어 대용량 파일이 섞이면 push가 막힌다. `git add .`를 남발하면 불필요·대용량 파일이 함께 올라가니, `.gitignore`로 데이터·모델·`.venv` 등을 먼저 제외하자.

---

## 관련 개념

- [[마크다운]] — README·이슈·PR이 전부 마크다운
- [[리눅스]] — git은 터미널에서 명령어로 사용
- [[VSCode]] — 에디터에서 git을 GUI로 편하게
- [[uv]] — git으로 받은 프로젝트의 가상환경 세팅·실행
