---
date: 2026-06-18
tags: [pandas, DataFrame, Series, loc, iloc, Boolean인덱싱, isin, value_counts, sort_values, reset_index, GroupBy, agg, unstack, merge, concat, pivot_table, 벡터화, 정리, 요약, 치트시트]
---

# pandas 기초 정리

[[NumPy]] 위에 선 **표(table) 가공 라이브러리**. 6/18 1부 실습을 섹션별 치트시트로. (라이브 = 필터링~groupby까지, 후반은 자료 제공)

> 강의 출처: [[0618-pandas와데이터시각화]] · 상세: [[DataFrame]] · [[loc]] · [[Boolean인덱싱]] · [[GroupBy]]

---

## 1. 로드 & 점검 → [[DataFrame]] · [[Series]]

```python
df.shape                  # (행, 열) 튜플
df.info()                 # 열·dtype·비결측수·메모리
df.describe()             # 수치형 요약 (범주형: include='object')
df.columns.tolist(); df.dtypes
df.memory_usage(deep=True).sum()
df.sample(5, random_state=42)   # 재현성
```

- 단일 컬럼 `df['col']` → [[Series]](1차원), 리스트 `df[['col']]` → [[DataFrame]](2차원). 헷갈리면 `type()`.

---

## 2. 행·열 선택 → [[loc]] · [[iloc]]

```python
df.loc[:, ['a','b']]      # 라벨 기반 (끝 포함), 초심자 권장
df[['a','b']]             # [] 열선택
df.iloc[0, :]             # 정수 위치(첫 행) → Series
df.iloc[:5, :3]           # 끝 미포함 (파이썬 규칙)
```

- [[loc]]=라벨/Boolean, [[iloc]]=정수 위치. **loc 슬라이싱 끝 포함 / iloc 끝 미포함**. 컬럼 선택 후 `.copy()` 권장.

---

## 3. ★조건 필터링 → [[Boolean인덱싱]] · [[isin]]

```python
df.loc[df['col'] == '값', :]                          # 단일조건
df.loc[(df['a']==1) & (df['b']==1), :]                # 복합 AND — 괄호+& 필수
df.loc[df['t'].isin(['일간','주간']), :]               # isin (긴 OR 대체)
df.loc[df['cpu'] > 40, :]                             # 부등호
df.loc[df['stock'] < df['threshold'], :]             # ★ 컬럼 간 비교
top = df['region'].value_counts().index[0]           # 최빈값
df.loc[df['flag']==1, :].reset_index(drop=True)      # 필터 후 인덱스 초기화
```

- 조건 = True/False **Boolean Series**(어제 [[NumPy]] 마스킹과 동일). 복합은 `&`·`|`·`~`, **각 조건 괄호 필수**. 필터 후 `reset_index(drop=True)`+`copy()`.

---

## 4. 정렬 → [[sort_values]]

```python
df.sort_values('cpu', ascending=False).head(8)       # 내림차순
df.sort_values(['loc','date'])                       # 다중키
df.sort_values(['a','b'], ascending=[True, False])   # 키별 방향
```

---

## 5. ★GroupBy 집계 → [[GroupBy]] · [[agg]] · [[unstack]]

```python
df.groupby('loc')['log_id'].count().sort_values(ascending=False)
df.groupby('cat')['val'].mean()
df.groupby('t')['hours'].agg(['mean','max'])         # agg 복수집계 → DataFrame
df.groupby(['region','type'])['score'].mean()        # 다중그룹 → MultiIndex
df.groupby(['region','type'])['score'].mean().unstack()   # 표로 펼치기
df.groupby('flag').size()
```

- [[SplitApplyCombine|Split-Apply-Combine]]: 그룹화→연산→결합. `df.groupby(키)[값].집계()`.
- `groupby()`만은 `DataFrameGroupBy` 객체(아직 결과 아님). 다중키→MultiIndex→[[unstack]]으로 펼쳐 [[loc]] 조회.

---

## 6. ⏩(자료) 결측·문자열·시계열 — 후속

```python
df.isna().sum(); df.duplicated().sum()               # 결측·중복
df.drop_duplicates(subset=['id'])
df['name'].str.upper(); df['x'].str.contains('사단', na=False)
pd.to_datetime(df['date'], errors='coerce').dt.year  # 깨진 값 NaT
```

---

## 7. ⏩(자료) 결합·변형 → [[merge]] · [[concat]] · [[pivot_table]]

```python
df1.merge(df2, on='key', how='inner')                # inner/outer/left/right
pd.concat([d1, d2], axis=0, ignore_index=True)       # 상하(0)/좌우(1)
df.pivot_table(values='v', index='cat', aggfunc='mean')
```

- ⚠ inner [[merge]]가 0행이면 **키 도메인 불일치** 의심(6/18 문제61: `unit_code` `31A`류 vs `17사단`류 → (0,33)). merge 전 키 교집합 점검.

---

## 8. ⏩(부록) 벡터화 → [[벡터화]]

```python
out = a*b + np.sqrt(a)       # 루프 대신 배열 일괄 (C 레벨)
# speedup 예 39.4x (난수 기반·환경별 편차)
```

- 어제 [[NumPy]] [[벡터화]]를 pandas 파생변수 대량 생성에 적용. 0나눗셈 방지(`eps`)·NFC 정규화 패턴.

---

## 관련

- [[0618-pandas와데이터시각화]] — 원본 강의 노트
- [[데이터시각화-원칙]] — 같은 회차 2부(시각화 이론)
- [[NumPy-기초]] — pandas의 토대(어제)
- [[DataFrame]] · [[Series]] · [[loc]] · [[iloc]] · [[Boolean인덱싱]] · [[isin]] · [[value_counts]] · [[sort_values]] · [[reset_index]] · [[GroupBy]] · [[SplitApplyCombine]] · [[agg]] · [[unstack]] · [[merge]] · [[concat]] · [[pivot_table]] — 개념 상세
- [[SQL]] · [[GROUP BY]] · [[불리언인덱싱]] · [[axis]] — 재사용 개념(SQL·NumPy)
