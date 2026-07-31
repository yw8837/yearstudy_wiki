---
date: 2026-06-29
tags: [로지스틱회귀, titanic, 이진분류, 실전, statsmodels, NumPy, 오즈비, 특성중요도, 정리]
aliases: [로지스틱회귀-titanic실전, 로지스틱회귀 실전, titanic 로지스틱]
---

# 로지스틱 회귀 titanic 실전 정리

> 출처: [[0629-로지스틱회귀실습]] · 상세: [[로지스틱회귀]] · [[시그모이드함수]] · [[오즈비]] · [[학습검증분할]] · [[특성중요도]]
> ⚠ sklearn 미사용(NumPy·statsmodels 직접). 노션 원문 기반이라 **전처리·모델적합·예측 코드는 갭**([원문 확인 필요]).

titanic 생존예측을 로지스틱 회귀로 푸는 **전과정 파이프라인**. 분할→적합→해석→평가→시각화 5단계.

---

## 1. 학습/검증 분할 (NumPy) → [[학습검증분할]]

```python
np.random.seed(42)                       # 재현성
indices = np.arange(len(df_model)); np.random.shuffle(indices)
train_size = int(0.8 * len(df_model))
train_df = df_model.iloc[indices[:train_size]].reset_index(drop=True)
test_df  = df_model.iloc[indices[train_size:]].reset_index(drop=True)
```

- **seed 고정 → 셔플 → 슬라이싱** 순서. 셔플을 빼면 정렬 편향으로 분할이 망가진다.
- sklearn `train_test_split`을 NumPy로 직접 구현한 것.

---

## 2. 특성/목표 분리

```python
feature_cols = ['pclass', 'sex', 'age', 'sibsp', 'parch', 'fare']   # 6개
X_train = train_df[feature_cols].values;  y_train = train_df['survived'].values
X_test  = test_df[feature_cols].values;   y_test  = test_df['survived'].values
```

- 특성 6개 → 목표 `survived`(0/1). `.values`로 NumPy 배열화.
- ⚠ `sex`는 수치 인코딩 전제(인코딩 코드는 노션에 없음 — [원문 확인 필요]).

---

## 3. 모델 적합 (statsmodels) → [[로지스틱회귀]]

- `result` = statsmodels `sm.Logit(y, X_const).fit()` 결과로 **추정**되나 **적합 코드가 노션에 없음** → [원문 확인 필요].
- 절편(const) 추가는 후속 코드(`result.params[1:]`, `'const (intercept)'`)에서 역추론.

---

## 4. 계수 해석 4단계 → [[오즈비]]

```python
coef_df = pd.DataFrame({
    'Feature': ['const (intercept)'] + feature_cols,
    'Coefficient': result.params, 'Std Error': result.bse,
    'p-value': result.pvalues, 'Odds Ratio': np.exp(result.params)
})
```

- **계수 부호** = 생존확률 방향(+/−), **[[p-value]]** = 유의성(<0.05), **[[오즈비]] exp(β)** = 오즈 배수.
- 계수는 로그오즈라 비직관적 → `exp`로 오즈비 변환해 해석.

---

## 5. 특성 중요도 시각화 → [[특성중요도]]

```python
coef_vals = result.params[1:]                     # 절편 제외
sorted_idx = np.argsort(np.abs(coef_vals))[::-1]   # 절댓값 내림차순
colors = ['steelblue' if c > 0 else 'salmon' for c in coef_vals[sorted_idx]]
plt.barh([feature_cols[i] for i in sorted_idx], coef_vals[sorted_idx], color=colors)
```

- 절댓값 순 barh, 양=파랑(생존↑)·음=빨강(생존↓).
- ⚠ 표준화 안 하면 스케일 왜곡 — 계수 크기 비교 주의.

---

## 관련

- [[분류모델-평가지표]] — 평가 단계(혼동행렬·정밀도/재현율/F1) 정리
- [[로지스틱회귀]] · [[시그모이드함수]] · [[오즈비]] · [[의사결정경계]] — 모델 개념
- [[학습검증분할]] · [[특성중요도]] — 파이프라인 개념
- [[0629-로지스틱회귀실습]] — 원본 강의노트
- [[0622-확률과통계적추론]] — 로지스틱 이론(직전 회차)
