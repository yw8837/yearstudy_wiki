---
date: 2026-06-18
tags: [pandas, DataFrame, Series, loc, iloc, Boolean인덱싱, isin, value_counts, sort_values, reset_index, GroupBy, SplitApplyCombine, agg, unstack, merge, concat, pivot_table, 벡터화, 데이터시각화, RoseDiagram, 앤스컴콰르텟, 비례잉크, 색상스케일, CVD, 미학요소, 좌표계, matplotlib, seaborn, plotly, streamlit]
---

# 06/18 · pandas와 데이터 시각화

4주차 둘째 회차. **1부 = [[pandas]] 데이터 가공**(어제 [[NumPy]] 위에 선다 — pandas는 numpy 기반), **2부 = 데이터 시각화 이론**. 라이브 1부는 Colab 환경세팅 → 로드·점검 → 행·열 선택([[loc]]·[[iloc]]) → ★조건 필터링([[Boolean인덱싱]]·[[isin]]·컬럼간 비교) → 정렬([[sort_values]]) → ★[[GroupBy]] 집계([[agg]]·[[unstack]])까지 군 데이터 70제로 실습. 2부는 『Fundamentals of Data Visualization』 기반으로 [[RoseDiagram]]·[[앤스컴콰르텟]]·시각화 원리·[[미학요소]]·[[좌표계]]·[[색상스케일]]·[[비례잉크원칙]]을 다뤘다.

> 🗂 **당일 라이브 vs 후속 구분(중요).** 6/18 실제 진행 = **pandas 필터링~groupby(agg/unstack)** + 시각화 **이론**. 예제 노트북 후반(§7 결측·문자열·시계열·[[merge]])·[[벡터화]] 부록·시각화 실습도구는 **자료 제공/후속(10~12일차)** → 본문에 ⏩표기.
>
> ⚠ **이번 회차 정정 핵심(보충 참조):** ① 문제27 `mental_wellbeing_score 60~80` → 실데이터(분포 1~10)상 **Empty DataFrame**, 의도는 `6~8`(원본 문제 오류 가능성). ② 문제61 inner [[merge]]가 `(0, 33)` → `unit_code` 도메인 불일치(군수품 `31A` vs 복지 `17사단`)로 교집합 0. ③ 벡터화 speedup 39.4x는 난수 기반이라 환경별 편차 가능.

---

## 01. 도입 — pandas는 [[NumPy]] 위에 선다

- **위치:** [[pandas]]는 어제 배운 [[NumPy]]의 [[ndarray]] 위에 만들어진 **표(table) 자료구조** 라이브러리. numpy가 "숫자 배열의 뼈대"라면 pandas는 "열마다 이름·타입이 있는 엑셀 같은 표".
- **[[SQL]]과의 관계:** 가공 논리(groupby 등)는 [[SQL]]과 유사·문법만 다름. 현실 구조 = **운영 DB는 SQL로 필요한 데이터만 추출 → 파이썬/pandas에서 분석**.
- **어제(6/17) 연계:** [[Boolean인덱싱]]·[[axis]] 집계·[[벡터화]]는 NumPy에서 이미 본 개념이 pandas에서 그대로 반복된다. §08 [[정규화]](DBA)/분석용 비정규화 흐름(6/16~17)이 "왜 분석가가 가공하나"의 배경.

→ [[pandas]] · [[NumPy]] · [[0617-데이터리터러시와NumPy기초]] 참조

---

## 02. 실습 환경설정 (코드노트용 — 디버깅 포인트)

> 운영 공지는 배제하되 실습 환경·디버깅 포인트는 코드노트로 보존. (상세 코드는 블로그 실습노트)

- **Colab은 리눅스** → 시각화 한글 깨짐 → 한글폰트(`fonts-nanum`) 설치 후 **런타임 > 세션 다시 시작**.
- **강사 강조(라이브):**
  - 폰트 설치 셀 + `drive.mount()`는 **항상 선실행**.
  - 데이터 경로는 직접 타이핑 금지 → 파일 메뉴 **"경로 복사"** 붙여넣기. **경로 끝 슬래시(/) 누락**이 잦은 실수.
  - **파일명 대소문자/오타 → `FileNotFoundError`** 반복 강조.
  - 압축 해제 시 한글 파일명 깨짐 → 파일명 통일. 공유 폴더는 읽기 전용 → 다운로드 후 사용.

---

## 03. 데이터 로드 & 구조 점검 → [[pandas]]

데이터 적재 직후 규모·스키마·분포를 점검하는 기본 메서드 묶음. 데이터셋 3종(각 11000행): 서버 `df_srv`(15열)·군수품 `df_sup`(15열)·복지 `df_wel`(19열).

```python
df_srv.shape                    # (11000, 15)   (행,열) 튜플, [0]=행 [1]=열
df_sup.info()                   # 열·dtype·비결측수·메모리 (object≈문자열)
df_wel.describe()               # 수치형 요약 (범주형은 include='object')
df_srv.columns.tolist()         # 컬럼명 리스트
df_sup.dtypes                   # 열별 dtype
df_sup.memory_usage(deep=True).sum()   # 1729533 (바이트)
df_wel.sample(5, random_state=42)      # random_state로 재현성
```

- **메모:** `head(n)`/`tail(n)`, `len(df)`, `memory_usage(deep=True)`도 점검 묶음. `random_state=42`로 표본 재현성.

→ [[pandas]] 참조

---

## 04. 핵심 이론: [[DataFrame]] vs [[Series]], [[loc]] vs [[iloc]]

🔑 **[[DataFrame]] vs [[Series]] — 라이브 반복 질문:**

| | [[Series]] | [[DataFrame]] |
|:--|:--|:--|
| 차원 | 1차원 (index + value) | 2차원 (index + 복수 column) |
| 생성 | 단일 컬럼 추출 `df['col']` | 컬럼 리스트 `df[['col']]`, 테이블 |
| 비유 | 엑셀 한 열 | 엑셀 시트 전체 |

- "단일 컬럼 → 보통 [[Series]] / 복수 컬럼·복수 값 구조 → [[DataFrame]]. 혼동되면 `type()`으로 확인 습관화." 클래스가 달라 같은 메서드(`value_counts()`)도 결과 형태가 달라진다.
- **dtype 종류:** int·float·bool·datetime·category·object(문자열·복합형).

🔑 **[[loc]] vs [[iloc]]:**

| | [[iloc]] | [[loc]] |
|:--|:--|:--|
| 기준 | 정수 위치(Integer-location) | 라벨/Boolean(명칭기반) |
| 입력 | `[4,3,0]`, 슬라이싱 `1:7`(끝 미포함), boolean | `5`·`'a'`, 라벨 리스트, 슬라이싱 `'a':'c'`(끝 포함) |

- **강사 강조:** 초반엔 혼란 방지 위해 **[[loc]] 중심** 학습 권장(분석 목적이면 loc로 충분). [[iloc]]은 위치기반이라 자동화/속도에 유리(개발·대용량). `df.컬럼명` 방식도 가능하나 한 방식으로 통일 위해 지양. 컬럼 선택 후 `copy()`로 별도 저장 권장.

→ [[DataFrame]] · [[Series]] · [[loc]] · [[iloc]] 참조

---

## 05. 행·열 선택 → [[loc]] · [[iloc]]

```python
df_srv.loc[:, ['server_id','location','cpu_usage']].head()  # loc 열선택
df_sup[['item_name','category','stock_level']].head()       # [] 열선택
df_srv.iloc[0, :]               # 첫 행 → Series
df_sup.iloc[:5, :3]             # 앞5행·앞3열 (iloc 슬라이싱은 끝 미포함)
df_sup.iloc[100:105]            # 중간 구간
```

- **메모:** 단일 `df['col']`→[[Series]], 리스트 `df[['col']]`→[[DataFrame]]. iloc 슬라이싱은 파이썬 규칙(끝 미포함), loc 라벨 슬라이싱은 끝 포함.

→ [[loc]] · [[iloc]] 참조

---

## 06. ★조건 필터링 (라이브 핵심 — 6/18 비중 큼) → [[Boolean인덱싱]] · [[isin]]

- **개념:** `df[조건]`의 조건은 비교연산이 만든 **True/False([[Boolean인덱싱|Boolean]]) Series**. 불리언 마스크를 [[loc]] 첫 인자에 넣어 행 필터링. **어제 [[NumPy]] 마스킹과 동일 원리.**
- **강사 강조(라이브):**
  - **괄호로 틀 먼저 만들고 조건 채우기** 권장. 문법 오류는 반복으로 감소.
  - 복합조건은 `and`/`or` 아닌 **비트연산자 `&`,`|`** + **각 조건 괄호 필수**(우선순위 문제 방지). `not`은 `~`.
  - "부등호는 한 번에 하나의 비교만 가능."
  - 필터 후 `reset_index(drop=True)` + `copy()`로 결과 저장 권장.

```python
# 단일조건
df_srv.loc[df_srv['issue_category'] == '디스크', :].head()
# value_counts 최빈값 기반
top_r = df_wel['region'].value_counts().index[0]            # .index[0]=최빈값
df_wel.loc[df_wel['region'] == top_r, :].head()
# 복합조건 AND (괄호 + &)
df_srv.loc[(df_srv['fix_required']==1) & (df_srv['issue_detected']==1), :].head()
# isin (여러 값 중 하나 — 긴 OR 대체)
df_srv.loc[df_srv['check_type'].isin(['일간','주간']), :].head()
# 부등호(수치 임계값)
df_srv.loc[df_srv['cpu_usage'] > 40, :].head()
# ★ 컬럼 간 비교 (컬럼 vs 컬럼 — 행 단위 True/False 축약)
df_sup.loc[df_sup['stock_level'] < df_sup['reorder_threshold'], :].head()
# 필터 후 인덱스 초기화
df_srv.loc[df_srv['temp_abnormal_flag']==1, :].reset_index(drop=True).head()
```

- **★컬럼 간 비교(문제26):** iris 예시 `Sepal.Width > Petal.Length`처럼 **행 단위 비교를 True/False 한 줄로 축약**(중간과정 없이). 라이브 4-6교시 강조 포인트.
- **⚠ 문제27 정정(보충):** `mental_wellbeing_score 60~80` 구간 필터 → **Empty DataFrame**(분포가 1~10). 의도는 `6~8`로 추정. 라이브에서도 "재확인 필요"로 기록됨 → 원본 문제 오류 가능성.
- 실습은 28번까지, 29~31은 자율로 생략.

→ [[Boolean인덱싱]] · [[isin]] · [[value_counts]] · [[reset_index]] 참조

---

## 07. 정렬 → [[sort_values]]

```python
df_srv.sort_values('cpu_usage', ascending=False).head(8)   # 내림차순, max cpu=100.0
df_sup.sort_values('requested_quantity').head(8)           # 기본 오름차순
df_srv.sort_values(['location','check_date']).head(8)      # 다중키(리스트)
```

- `ascending=[True,False]`로 키별 방향 지정 가능. `na_position`으로 결측 위치. 상위 n개는 `head` 조합.

→ [[sort_values]] 참조

---

## 08. ★[[GroupBy]] 집계 (라이브 4-6교시 핵심) → [[GroupBy]] · [[agg]] · [[unstack]]

🔑 **[[SplitApplyCombine|Split-Apply-Combine]]:** 전체 → **Split**(그룹화) → **Apply**(연산) → **Combine**(결합). 문법 `df.groupby(그룹컬럼)[수치컬럼].집계함수()`. **어제 [[axis]] 집계의 pandas판.** [[SQL]]의 [[GROUP BY]]와 같은 논리.

- **강사 강조(라이브):**
  - **그룹 컬럼 기준:** 고유값 너무 많은 연속형(예 `total_bill`)은 부적절. 범주형(성별·요일)·제한된 수치(`size`)는 가능.
  - `groupby()` 결과 자체는 DataFrame 아닌 **`DataFrameGroupBy` 객체**.
  - [[agg|`agg([...])`]]로 복수 집계 동시 → 결과가 복수 컬럼이면 **[[DataFrame]]**.
  - 다중그룹은 결과가 Series여도 인덱스가 **MultiIndex**가 됨.
  - ★[[unstack|`unstack()`]]: MultiIndex 결과는 조회가 불편 → **표 형태로 펼쳐** 가독성·조회 개선 → 이후 [[loc]]로 특정 조합 조회.

```python
df_srv.groupby('location')['log_id'].count().sort_values(ascending=False).head(10)
df_srv.groupby('issue_category')['cpu_usage'].mean()
df_srv.groupby('check_type')['fix_duration_hours'].agg(['mean','max'])   # agg 복수집계
df_wel.groupby(['region','facility_type'])['satisfaction_score'].mean().head(15)  # 다중그룹
df_sup.groupby('auto_reorder_flag').size()
```

```
# 문제35 출력 (check_type별 수리시간) — agg 복수집계 → DataFrame
                mean    max
check_type
월간          2.983514  11.56
일간          3.016626  11.79
주간          2.998414  11.47
```

- **집계 메서드:** count·sum·min·max·mean·median·std·var·quantile·first·last.
- **메모:** "location별 log_id count", "다중키 그룹", "unstack → loc 조회"가 핵심 시연. 다음 시간 groupby 연습 이어가기로 예고.

→ [[GroupBy]] · [[SplitApplyCombine]] · [[agg]] · [[unstack]] 참조

---

## 09. ⏩(자료 제공·후속) 예제 노트북 추가 범위

> 예제 70제 중 6/18 라이브는 ~문제40 부근까지. 아래는 노트북에 포함된 **후속 주제**(자료 제공). 블로그 실습노트 참조.

- **결측·중복:** `isna().sum()`(열별 결측, 본 데이터는 0)·`duplicated().sum()`·`drop_duplicates(subset=[...])`. 수치 결측은 중앙값, 범주는 `fillna('Unknown')`.
- **파생·구간화:** `assign()`(원본 보존)·`pd.cut(bins, labels)`(구간화).
- **문자열(`str` 접근자):** `.str.upper()`·`.str.contains('사단', na=False)`(na=False로 결측 처리)·`.str[:2]`·`.str.len()`.
- **시계열(`to_datetime`·`.dt`):** `pd.to_datetime(col, errors='coerce')`(깨진 값 NaT)·`.dt.year`·`.dt.to_period('M')`·`.dt.dayofweek`(월=0…일=6).
- **결합·변형 → [[merge]] · [[concat]] · [[pivot_table]]:**

```python
m = df_sup.merge(df_wel, on='unit_code', how='inner', suffixes=('_sup','_wel'))
m.shape   # (0, 33)  ← ★ 주의: unit_code 도메인 불일치(아래 정정)
loc_cpu = df_srv.groupby('location', as_index=False)['cpu_usage'].mean()
df_srv.merge(loc_cpu, on='location', how='left')   # 그룹평균을 원본에 붙이기(left)
pd.concat([df_srv.iloc[:2], df_srv.iloc[2:4]], axis=0)        # 상하 결합
df_sup.pivot_table(values='avg_daily_usage', index='category', aggfunc='mean')
```

- **⚠ 문제61 정정(보충):** inner [[merge]]가 `(0, 33)` → `unit_code` 값 도메인이 테이블 간 불일치(군수품 `31A/18A`류 vs 복지 `17사단/14사단`류)로 **교집합 0**. **merge 키 정합성 점검의 반례.**
- **merge how 4종:** inner(교집합)/outer(합집합)/left(좌 기준)/right(우 기준). **concat:** `axis=0` 상하·`axis=1` 좌우, `join='inner'/'outer'`, `ignore_index`.

→ [[merge]] · [[concat]] · [[pivot_table]] 참조

---

## 10. ⏩(부록) [[벡터화]] — `vectorization.ipynb`

> 어제 [[NumPy]]에서 만든 [[벡터화]] 개념을 pandas 파생변수 생성에 적용. 자료 제공 부록.

- **개념:** 행 단위 `for` 루프 대신 **열 전체(배열)에 한 번에** 연산 → 속도·가독성. pandas 스칼라 연산 / numpy ufunc / 축별 집계(`mean(axis=1)`).

```python
rng = np.random.default_rng(0); n = 50_000
a = rng.random(n); b = rng.random(n)
out_vec = a*b + np.sqrt(a)        # 벡터화 (루프 없이)
# 결과 예: loop 17.3 ms / vector 0.4 ms / speedup 39.4x  ← ★난수라 환경별 편차
```

- **파생변수 패턴:** 군수품(열간 사칙·`clip` eps로 0나눗셈 방지)·서버(`mean(axis=1)`·`np.log1p`·`pd.factorize`)·복지(브로드캐스팅·min-max 정규화)·Excel(열방향 z-score).
- **메모:** 한글 파일명 NFC/NFD 차이 대응(`unicodedata.normalize("NFC", ...)`) — macOS(NFD) vs Windows/Linux(NFC) 자모분리. 0으로 나누기 방지(`eps`·`np.where(denom<1e-9,1.0,denom)`) 패턴 반복(실무 디버깅).
- **⚠ 정정(보충):** speedup **39.4x**는 `np.random` 난수 입력이라 머신·시드별로 수치가 달라질 수 있음(경향만 신뢰).

→ [[벡터화]] · [[NumPy]] 참조

---

# 2부 · 데이터 시각화 (이론)

> 출처: 『Fundamentals of Data Visualization』(C. Wilke)의 한글 번안 교안. 6/18은 **이론**, 코드 실습은 10~12일차 예고.

## 11. 시각화의 중요성 → [[RoseDiagram]] · [[앤스컴콰르텟]]

### 11.1 나이팅게일 [[RoseDiagram|Rose Diagram]]
- 나이팅게일(1820~1910, 간호사·통계학자)이 크림전쟁 사망 데이터를 **Rose Diagram(장미도표/polar area)**으로 시각화. 색상: 파랑=예방가능 전염병, 빨강=전상, 검정=기타.
- 두 시기 비교(위생개혁 전/후) → **Before/After 정책개입 효과를 시계열 비교**.
- **강사 강조:** 의회·육군 수뇌부에 **숫자 표가 아닌 시각적 형태로 제출** → "데이터 자체 보고(X), 데이터 **전달 방식의 차별화**(O)".

### 11.2 [[앤스컴콰르텟|앤스컴 콰르텟]] (Anscombe's Quartet)
- 4개 데이터셋(I~IV)이 **동일 기술통계량**(x평균 9·y평균 7.5·상관 0.816·회귀선 y=3.00+0.500x·R²=0.67)이지만 **시각화하면 완전히 다름**.
- **강사 강조:** 수치 통계만 보면 안 되고 **반드시 시각화로 분포·패턴 확인**(어제 [[데이터커뮤니케이션]] 연계).

→ [[RoseDiagram]] · [[앤스컴콰르텟]] 참조

---

## 12. 시각화의 원리: 삭제 > 분리 > 강조 > 배열

> 출처: Gene Zelazny 『Say it with Charts』

| 원리 | 설명 |
|:--|:--|
| **삭제(Delete)** | 필수 데이터만 남기고 의미 없는 차트는 삭제 |
| **분리(Divide)** | 데이터 분리 후 별도 차트로, 논리 순서로 배열 |
| **강조(Highlight)** | 필수는 강조, 부가는 약하게/숨김(대조) |
| **배열(Arrange)** | 영역별 그룹핑 후 영역 간·내 데이터를 논리 순서로 구조화 |

---

## 13. 시각화 기초 → [[미학요소]] · [[좌표계]] · [[색상스케일]]

### 13.1 좋은 시각화의 3대 문제 & 목표
- **심미적**(매력적인가)·**지각적**(명확/정보과다)·**수학적**(수치 왜곡 — 축·면적 임의조정).
- **목표:** 명확(Clear)·정직(Honest)·간결(Concise).

### 13.2 [[미학요소|미학 요소(Aesthetics)]]
- 시각 속성: **위치(Position)**·형태(Shape)·크기(Size)·**색상(Color)**. 데이터 타입: 연속형/이산형/범주형.
- **핵심 원칙:** ① **위치·길이가 가장 정밀하게 인식** → 중요 변수 우선 매핑(색상·면적은 보조). ② **일대일 매핑(가역성)** — 값↔시각요소 1:1, 역추정 가능. ③ **접근성([[CVD]])** — 색만 말고 모양/패턴 병행. ④ **단순성** — 미학 요소 **3개 이내**(인지부하).

### 13.3 [[좌표계]]
- **직교(Cartesian):** 표준 2D, 같은 단위면 종횡비 1:1. **로그 스케일:** 한 단위=고정 배수(곱셈), 비율·넓은 범위, 0·음수 정의 안 됨(기준 1). **극좌표(Polar):** 각도·거리, 주기 데이터(연 기온·24시간), 파이차트=극좌표 stacked bar 변형. **지도 투영:** 3D→2D 필연 왜곡, 면적/형태/거리 trade-off(Mercator 정각/Goode 정적/Robinson 절충).
- **실무체크:** 같은 물리단위면 1:1, **막대는 0 시작**·산점/선은 0 불필요.

### 13.4 [[색상스케일|색상 스케일]] — 3대 용도
| 용도 | 스케일 | 예 |
|:--|:--|:--|
| **구분(Distinguish)** | Qualitative | Okabe-Ito (순서없는 범주) |
| **값 표현(Represent)** | Sequential/Diverging | Blues(밀도)·PiYG(편차, 중립점) |
| **강조(Highlight)** | Accent | 회색조 배경 + 강조색 |
- **[[CVD]]:** 남성 약 8% 적록색맹 → 빨강-초록 대비 회피, 명도 차이 병행(Viridis/Okabe-Ito).
- **무지개색 함정:** Rainbow/Jet은 비단조 밝기로 순서 왜곡·인위 경계 → **Viridis/Magma/Inferno**(지각적 균등) 권장.

→ [[미학요소]] · [[좌표계]] · [[색상스케일]] · [[CVD]] 참조

---

## 14. 목적별 차트 유형

| 목적 | 대표 차트 | 핵심 |
|:--|:--|:--|
| **양(Amounts)** | Bar/Dot/Heatmap | 막대 0 시작, 의미있는 정렬, 라벨 길면 수평 |
| **분포(Distributions)** | Histogram/KDE/Boxplot/Violin/ECDF/Q-Q | 히스토그램=bin 민감, KDE=대역폭 민감, ECDF=손실 없음 |
| **비율(Proportions)** | Pie/Stacked bar/Treemap/Mosaic | 부분-전체 |
| **관계(Relationships)** | Scatter/Line/Correlogram | 시계열=Line+Direct Labeling, 추세=Smoothing |
| **지리(Geospatial)** | Choropleth/Cartogram | 단계구분도는 비율(rate), 면적왜곡 주의 |
| **불확실성(Uncertainty)** | Error bar/CI band | SD/SE/CI 명시, 빈도 프레이밍("100명 중 5명") |

- **세부:** 분포 그룹비교 Boxplot(요약·이상치) vs Violin(밀도) vs Strip(개별점+jitter) vs Sina(하이브리드) vs Ridgeline. 시계열은 Direct Labeling(범례 대신 선 끝 라벨, Eye Tennis↓). 오버플로팅은 부분투명도/지터링/2D히스토그램·헥사곤.

---

## 15. 디자인 원칙 → [[비례잉크원칙]] · [[CVD]]

- **5대 핵심:** 매핑·좌표계·색상·유형별 시각화·불확실성.
- **★[[비례잉크원칙|비례 잉크 원칙(Proportional Ink, Value ∝ Area)]]:** 음영 면적은 수치에 정비례. 선형축 막대는 **반드시 0에서 시작**(축 절단=왜곡, 작은 차이는 Dot Plot). 로그축 막대는 기준선 1. → 어제 [[데이터커뮤니케이션]]의 "영점 부재" 왜곡과 직결.
- **중복코딩(Redundant Coding):** 색+모양/선종류 병행(흑백 인쇄·[[CVD]] 대비).
- **복합 구성:** Small Multiples(동일 축 공유)·Compound Figures(이질 차트+일관 시각언어+패널 라벨 a,b,c). 3D 효과 지양.

→ [[비례잉크원칙]] · [[CVD]] 참조

---

## 16. ⏩(후속·10~12일차) 실습 도구 예고

> 코드 실습은 10~12일차. 6/18 라이브에선 미진행.

- **matplotlib + seaborn (10-11일차):** seaborn은 pandas DataFrame에서 쉽게 시각화·통계(회귀선) 구현·기본 색/글꼴 담당, matplotlib으로 틀·세부 옵션.
- **plotly (10-11일차):** 대화형 차트·대시보드. Graph Objects(Low-level)/Plotly Express(High-level). 데이터→Figure→add_traces→update_layout.
- **streamlit (12일차):** 빠른 대시보드 프레임워크. "파이썬 스크립팅처럼", 위젯=변수(`x=st.slider('x')`), 캐시(`@st.cache_data`/`@st.cache_resource`).

---

## 보충

📌 **★문제27 — Empty DataFrame 정정.** `mental_wellbeing_score`의 `60~80` 구간 필터는 **빈 결과**다. 이 점수 분포가 **1~10**이라 60~80에 해당하는 행이 없기 때문. 라이브 노트에도 "`60~80` vs `6~8` 재확인 필요"로 기록 → 의도된 구간은 `6~8`로 보이며 **원본 문제의 오타/오류 가능성**. 필터 결과가 비면 "조건이 틀렸나, 데이터에 그 값이 없나"를 `value_counts()`·`describe()`로 먼저 확인하는 습관 권장.

📌 **★문제61 — inner [[merge]] (0,33) 정정.** `df_sup.merge(df_wel, on='unit_code', how='inner')` 결과가 `(0, 33)`(0행). 원인은 **두 테이블의 `unit_code` 값 도메인 불일치** — 군수품은 `31A/18A`류, 복지는 `17사단/14사단`류로 **교집합이 0**. 행 수가 0이면 키 자체가 안 맞는 것 → merge 전 `set(a) & set(b)`로 키 교집합 점검하는 **키 정합성 검사의 반례**로 유용. (의도된 학습용 반례인지, 데이터 정의 불일치인지 강사 확인 가능 시 확인.)

📌 **벡터화 speedup 39.4x는 참고치.** `np.random.default_rng(0)` 난수 입력·`n=50_000` 기준 측정값이라 CPU·캐시·시드에 따라 수치가 흔들린다. **"수십 배 빨라진다"는 경향**만 신뢰하고 절대 수치는 환경별 재측정.

📌 **[[loc]] 슬라이싱은 끝 포함, [[iloc]]은 끝 미포함.** `df.loc['a':'c']`는 `'c'` 포함(라벨 기반), `df.iloc[0:3]`은 인덱스 3 **미포함**(파이썬 정수 슬라이싱 규칙). 라벨/위치 혼동이 잦은 지점.

📌 **`groupby()` 결과는 아직 DataFrame이 아니다.** `df.groupby('x')`는 `DataFrameGroupBy` 객체 → 뒤에 집계함수를 붙여야 결과(Series/DataFrame)가 나온다. [[agg]] 복수집계는 컬럼이 늘어 [[DataFrame]], 단일집계는 보통 [[Series]]. 다중키 그룹은 MultiIndex → [[unstack]]으로 펼친다.

📌 **시각화 2부는 이론·후속 실습 분리.** 6/18은 시각화 **개념/원칙**(왜·무엇)만. 실제 matplotlib/seaborn/plotly/streamlit **코드**는 10~12일차. 본 노트의 §16과 차트 유형(§14)은 "도구가 붙기 전 이론 지도".

---

## 🔗 연관 노트

- [[0617-데이터리터러시와NumPy기초]] — 직전 강의(pandas는 NumPy 위에 섬, Boolean·axis·벡터화 연계, 정규화/비정규화 배경)
- [[pandas-기초]] · [[데이터시각화-원칙]] — 본 차시 정리노트
- [[DataFrame]] · [[Series]] · [[loc]] · [[iloc]] · [[Boolean인덱싱]] · [[isin]] · [[value_counts]] · [[sort_values]] · [[reset_index]] · [[GroupBy]] · [[SplitApplyCombine]] · [[agg]] · [[unstack]] · [[merge]] · [[concat]] · [[pivot_table]] — pandas 핵심
- [[RoseDiagram]] · [[앤스컴콰르텟]] · [[미학요소]] · [[좌표계]] · [[색상스케일]] · [[CVD]] · [[비례잉크원칙]] — 시각화 이론
- [[NumPy]] · [[ndarray]] · [[axis]] · [[벡터화]] · [[불리언인덱싱]] — 어제 NumPy에서 재사용
- [[SQL]] · [[GROUP BY]] · [[정규화]] · [[데이터커뮤니케이션]] · [[정형데이터]] — 재사용 개념
