---
date: 2026-06-19
tags: [matplotlib, 시각화, fig_ax, 객체지향, seaborn, OkabeIto, Wilke, 색일관성, 정리, 요약, 치트시트]
---

# matplotlib 시각화 실무 정리

6/19 실습 노트북(`matplotlib 시각화 원칙`·`viz_basic_01`·`viz_advanced_01`)의 **코드 시각화 원칙** 요약. 6/18 [[데이터시각화-원칙|시각화 이론]]에 코드가 붙는 부분.

> 강의 출처: [[0619-통계자료요약]] · 상세: [[matplotlib객체지향]] · [[시각화디자인원칙]] · [[OkabeIto]]

---

## 1. fig, ax 명시적 방식을 써라 → [[matplotlib객체지향]]

```python
# ❌ plt 직접 호출 (암묵적·전역 상태)
plt.figure(figsize=(6,4)); plt.plot(x, y); plt.title("t"); plt.show()
# ✅ fig, ax 명시적 (객체지향·권장)
fig, ax = plt.subplots(figsize=(6,4))
ax.plot(x, y); ax.set_title("t"); ax.set_xlabel("x"); plt.show()
```

- **plt 직접 호출이 위험한 7가지:** ① 전역 상태(활성 Figure 조용히 바뀜) ② 반복문·함수 오작동 ③ axes 인자 못 받아 재사용 불가 ④ subplot 독립 제어 불가 ⑤ `fig.suptitle`·`tight_layout` 접근 불가 ⑥ **seaborn(`ax=`)과 충돌** ⑦ 셀 실행 순서에 결과 의존(디버깅 난이도).
- **한 줄 요약:** subplot·함수화·seaborn 혼용이 조금이라도 있으면 `fig, ax`가 사실상 필수.

---

## 2. 색은 데이터를 인코딩할 때만 → [[시각화디자인원칙]] · [[OkabeIto]]

> Claus O. Wilke 『Fundamentals of Data Visualization』

- **6원칙:** ① 색=데이터 인코딩 전용(장식 무지개 금지) ② **동일 범주=동일 색**(전체 일관) ③ 색각친화([[OkabeIto]]·[[CVD]]) ④ 강조 절제(강조만 vermillion #D55E00) ⑤ 연속/분포는 단일 색조 농도·투명도 ⑥ Data-ink(상·우 spine 제거).

```python
PALETTE_OI = ["#E69F00","#56B4E9","#009E73","#F0E442",
              "#0072B2","#D55E00","#CC79A7","#000000"]  # Okabe-Ito
COLOR_SEQ="#0072B2"; COLOR_EMPHASIS="#D55E00"; COLOR_NEUTRAL_BAR="#B0B0B0"
def stable_cat_colors(values, palette=PALETTE_OI):
    cats = sorted(pd.Series(values).dropna().unique())
    return {c: palette[i % len(palette)] for i, c in enumerate(cats)}   # 범주→색 고정
```

---

## 3. 색-일관성 (advanced 노트북 핵심 메시지)

- **같은 범주는 차트가 바뀌어도 같은 색.** `COLOR_BY_ISSUE`·`COLOR_BY_CHECK` 키를 차트끼리 공유 → 독자 인지부하↓.
- 중립은 회색, 강조 1개만 vermillion. 연속값은 청색 단일 색조. 상관행렬은 diverging(`vlag`, center=0).

---

## 4. 차트별 매핑 (실습 11종+)

| 목적 | seaborn/mpl | 색 규칙 |
|:--|:--|:--|
| 범주 개수 | `barh`/`bar` | 중립 회색 + 최다 강조 / 범주별 고정색 |
| 분포 | `histplot`/`kdeplot` | 단일 청색 |
| 그룹 분포 | `boxplot`/`violinplot` | 범주별 고정색(키 공유) |
| 2D 밀도 | `hexbin` | 그룹별 Blues/Oranges |
| 추이 | `lineplot` | 단일 시계열 단색 |
| 상관 | `heatmap` | diverging(center=0) |
| 구성비 | pie/도넛 | 범주 고정색 |

---

## 5. 한글 폰트 (코드노트용 디버깅)

```python
# Colab(리눅스) 최초 1회 → 런타임 재시작 필요
!sudo apt-get install -y -qq fonts-nanum
# set_nanum_gothic(): Windows=Malgun Gothic, Linux=NanumGothic 탐색·등록
plt.rcParams['axes.unicode_minus'] = False   # 마이너스 깨짐 방지
```

---

## 관련

- [[0619-통계자료요약]] — 원본 강의 노트
- [[데이터시각화-원칙]] — 6/18 시각화 이론(왜·무엇)
- [[matplotlib객체지향]] · [[시각화디자인원칙]] · [[OkabeIto]] · [[색상스케일]] · [[CVD]] — 개념 상세
