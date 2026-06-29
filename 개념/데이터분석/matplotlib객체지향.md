---
date: 2026-06-19
tags: [matplotlib객체지향, matplotlib, figax, subplots, 시각화, 데이터분석, 도구]
aliases: [matplotlib객체지향, fig ax, fig, ax, subplots방식, 객체지향matplotlib]
---

# matplotlib 객체지향 방식 (fig, ax)

**정의**: matplotlib을 `plt` 전역 호출(암묵적)이 아니라 `fig, ax = plt.subplots()`로 **Figure·Axes 객체를 직접 다루는** 방식. 명시적·재사용 가능해 권장된다.

---

## 왜 필요한가

`plt` 직접 호출은 "현재 활성 그래프"라는 전역 상태에 의존해, subplot·함수화·seaborn 혼용 시 예측 불가하게 깨진다. 객체지향 방식은 어느 axes에 그릴지 명시해 이 문제를 없앤다.

---

## 어떻게 작동하나

```python
# ✅ 권장 — 객체를 명시
fig, ax = plt.subplots(figsize=(6,4))
ax.plot(x, y); ax.set_title("t"); ax.set_xlabel("x")
# ❌ 비권장 — 전역 상태
plt.plot(x, y); plt.title("t")
```

- `fig`=전체 도화지, `ax`=하나의 좌표축. seaborn은 `sns.boxplot(..., ax=ax)`처럼 axes를 받는다.

---

## 자주 헷갈리는 것

**plt 직접 호출의 7가지 함정:** ① 전역 상태(활성 Figure 조용히 바뀜) ② 반복문·함수 오작동(`gca()`) ③ axes 인자 못 받아 재사용 불가 ④ subplot 독립 제어 불가 ⑤ `fig.suptitle`·`tight_layout` 접근 불가 ⑥ **seaborn(`ax=`)과 충돌** ⑦ 셀 실행 순서 의존(디버깅 난이도).

---

## 더 알면 좋은 것

📌 빠른 단발 그래프는 `plt`로도 충분하지만, subplot이 둘 이상이거나 함수로 차트를 만들거나 seaborn을 섞으면 `fig, ax`가 사실상 필수다.

---

## 관련 개념

- [[시각화디자인원칙]] — 무엇을 어떻게 그릴지
- [[OkabeIto]] — 색 팔레트
- [[데이터시각화-원칙]] — 시각화 이론(6/18)
