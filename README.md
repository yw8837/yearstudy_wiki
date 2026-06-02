# 📚 yearstudy_wiki

> 이어드림스쿨 AI 6기 **개인 학습 위키** — 강의에서 배운 걸 이해한 내용으로 정리한 지식 저장소.
> **[Obsidian](https://obsidian.md)** 으로 열면 가장 편합니다. 🌱

---

## 🌳 구조

| 폴더 | 역할 |
|:---|:---|
| **`강의/`** | 날짜별 강의 노트 (0526, 0529 …) |
| **`개념/`** | 개념 상세 — 주제별 하위폴더 (파이썬·리눅스·IT기초·AI·도구) |
| **`개념/색인.md`** | 🔍 키워드로 바로 찾기 |
| **`정리/`** | 주제별 요약 치트시트 (파이썬 문법, 리눅스 기초 …) |
| **`인덱스.md`** | 🗺️ 전체 지도 — **여기서 시작** |

> 3층 구조: **강의(그날) → 개념(깊게) → 정리(한눈에)**

---

## 🚀 시작하기 — 3단계

**1. Obsidian 설치** → https://obsidian.md/download (무료)

**2. 이 저장소 내려받기**
- 🟢 *git 몰라도:* 초록 **`<> Code`** 버튼 → **`Download ZIP`** → 압축 해제
- 🔵 *git 쓰면:* `git clone https://github.com/yw8837/yearstudy_wiki.git`

**3. Obsidian에서 열기**
→ `Open folder as vault` → 받은 `yearstudy_wiki` 폴더 선택 → **`인덱스.md`** 부터!

---

## 🧭 이럴 땐 여기로

| 상황 | 위치 |
|:---|:---|
| 용어 찾기 ("range가 뭐였지?") | `개념/색인.md` |
| 그날 복습 | `강의/` |
| 주제 빠르게 훑기 | `정리/` |
| 개념 깊게 이해 | `개념/<주제>/` |

---

## 💡 알아두면 좋은 것

- `[[링크]]`는 **Obsidian에서만** 클릭됨 (GitHub 웹에선 글자로 보임)
- 검색 `Ctrl/Cmd + Shift + F` · 파일 열기 `Ctrl/Cmd + O` · 좌측 **그래프 아이콘**으로 연결 보기
- 사이드바는 **가나다순** 정렬 → `인덱스.md`를 맨 위에 두고 싶으면 **북마크(Bookmarks)** 로 즐겨찾기 (이름 안 바꿔도 OK)

---

## ✍️ 작성 규칙

운영 규칙은 **[`CLAUDE.md`](CLAUDE.md)** 참고 (확실한 정보만 · 초보자 눈높이 · 📌보충 구분).

---

## 🔄 업데이트 방법

> 매일 업데이트됩니다. 터미널(macOS·Linux) 또는 **Git Bash**(Windows)에서 실행하세요.

> ⚠️ **명령어는 반드시 `yearstudy_wiki` 폴더 "안"에서 실행해야 합니다.**
> 바탕화면(Desktop)이나 엉뚱한 위치에서 `git pull`을 치면 `fatal: not a git repository` 에러가 납니다.
> **현재 위치 확인** → `pwd` (지금 폴더 출력) / `ls` (목록에 `README.md·강의·개념·정리`가 보이면 제대로 들어온 것).

### 📥 받아가는 사람 — 최신으로 받기

**① 처음 한 번만 — 저장할 위치로 이동해서 복제(clone)**
```bash
cd ~/Documents          # 위키를 둘 위치로 이동 (원하는 곳 아무 데나)
git clone https://github.com/yw8837/yearstudy_wiki.git
```
→ 지금 위치(`~/Documents`)에 `yearstudy_wiki` 폴더가 생깁니다.

**② 그다음부터 — 그 폴더 "안"으로 들어가서 받기**
```bash
cd ~/Documents/yearstudy_wiki    # ★ ①에서 생긴 폴더로 이동 (꼭 이 안에서!)
git pull                         # 최신 내용 받기
```

> 💡 ZIP으로 받으면 `git pull`이 안 됨 → 자주 받을 거면 처음부터 `git clone` 추천.
> 💡 받은 폴더를 Obsidian `Open folder as vault` 로 열면 됩니다.

### 📤 위키 주인 — 내 노트를 GitHub에 올리기

위키 폴더는 `C:\Users\최용우\yearstudy_wiki` 에 있습니다. **Git Bash를 열고 그 폴더로 먼저 이동**한 뒤 실행하세요. (Desktop에서 치면 안 됩니다!)

```bash
cd /c/Users/최용우/yearstudy_wiki    # ★ 위키 폴더로 이동 (Git Bash는 C:\ → /c/ 로 씀)
git add -A                           # 변경된 파일 전부 담기
git commit -m "0602 강의 정리 추가"   # 무엇을 바꿨는지 메모
git push                             # GitHub에 올리기
```

> 💡 **Git Bash 경로 규칙**: Windows의 `C:\Users\...` 는 `/c/Users/...` 로 씁니다 (드라이브 `C:` → `/c/`, 역슬래시 `\` → 슬래시 `/`).
> 💡 한 줄로: `cd /c/Users/최용우/yearstudy_wiki && git add -A && git commit -m "메모" && git push`
> 🪄 매일 치기 번거로우면 Obsidian 플러그인 **Obsidian Git** 으로 자동 커밋·푸시(예: 10분마다) 설정 가능.

---

*이어드림스쿨 AI 6기 · 개인 학습 기록* 🌱
