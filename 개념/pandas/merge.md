---
date: 2026-06-18
tags: [merge, pandas, 결합, JOIN, 데이터분석]
aliases: [merge, 머지, 병합]
---

# merge (키 기준 결합)

**정의**: 두 [[DataFrame]]을 **공통 키(on)** 기준으로 결합하는 [[pandas]] 메서드. SQL [[JOIN]]에 대응. `df1.merge(df2, on='key', how='inner')`.

---

## 왜 필요한가

여러 테이블에 흩어진 정보(주문+고객, 서버+그룹평균)를 키로 이어붙여 하나의 분석 테이블을 만든다. 어제 [[정규화|비정규화]] 분석 테이블 생성의 핵심 도구.

---

## 어떻게 작동하나

```python
df1.merge(df2, on='unit_code', how='inner')   # 교집합
loc_cpu = df.groupby('location', as_index=False)['cpu'].mean()
df.merge(loc_cpu, on='location', how='left')  # 그룹평균을 원본에 붙이기(left)
```

- **how 4종:** `inner`(교집합)·`outer`(합집합)·`left`(좌 기준)·`right`(우 기준). `suffixes`로 겹치는 컬럼명 구분.

---

## 자주 헷갈리는 것

**⚠ inner merge 결과가 0행이면 키 도메인 불일치 의심.** 6/18 문제61: `unit_code`가 군수품 `31A`류 vs 복지 `17사단`류라 **교집합 0** → `(0, 33)`. 행 수 0은 조인 실패 신호. merge 전 `set(a)&set(b)`로 **키 정합성 점검**.

---

## 더 알면 좋은 것

📌 SQL [[INNER JOIN]]·[[LEFT JOIN]]과 의미가 같다. 키 이름이 다르면 `left_on`/`right_on`. 인덱스 기준 결합은 `join` 또는 `merge(left_index=True)`.

---

## 관련 개념

- [[JOIN]] · [[INNER JOIN]] · [[LEFT JOIN]] — SQL의 대응
- [[concat]] — 키 없이 단순 이어붙이기
- [[GroupBy]] — 그룹평균을 merge로 원본에 부착
