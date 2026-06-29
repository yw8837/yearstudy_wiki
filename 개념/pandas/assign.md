---
date: 2026-06-19
tags: [assign, 파생변수, 메서드체이닝, pandas, 데이터분석]
aliases: [assign, df.assign, 파생변수assign]
---

# assign (파생변수 생성)

**정의**: 기존 [[DataFrame]]에 **새 컬럼을 추가한 새 DataFrame을 반환**하는 메서드. 원본을 바꾸지 않아 메서드 체이닝에 쓰인다. (6/19 라이브 pandas 보충, 문제47)

---

## 왜 필요한가

`df['new'] = ...`는 원본을 직접 수정한다. `assign`은 **원본 보존 + 체이닝**이 가능해 파이프라인식 코드에 적합하다.

---

## 어떻게 작동하나

```python
dr_srv.assign(
    usage_sum = dr_srv['cpu_usage'] + dr_srv['memory_usage'] + dr_srv['disk_usage']
)[['cpu_usage','disk_usage','memory_usage','usage_sum']].head()
```

- `assign(새컬럼=식)` → 새 컬럼이 붙은 복사본 반환. 여러 컬럼을 한 번에 추가 가능.

---

## 자주 헷갈리는 것

**원본은 안 바뀐다** — 결과를 다시 변수에 받아야 유지된다. 강사는 "개인적으로는 비추"라 했는데, 단순 컬럼 추가는 `df['new']=...`가 더 직관적이기 때문(취향·상황에 따라 선택).

---

## 더 알면 좋은 것

📌 `assign`은 메서드 체이닝(`df.query(...).assign(...).groupby(...)`)에서 빛난다. 람다로 직전 단계 결과를 참조할 수도 있다: `df.assign(z=lambda d: d['x']/d['y'])`.

---

## 관련 개념

- [[DataFrame]] — 대상 자료구조
- [[GroupBy]] — 체이닝에서 자주 함께 사용
- [[pandas]] — 파생변수 생성 패턴
