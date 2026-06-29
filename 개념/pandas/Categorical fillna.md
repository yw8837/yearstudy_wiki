---
date: 2026-06-19
tags: [Categorical, fillna, category, dtype, 결측치, pandas, 데이터분석]
aliases: [Categorical fillna, category fillna, 카테고리결측치, Categorical에러]
---

# Categorical fillna 에러

**정의**: `category` dtype 컬럼에 **등록되지 않은 새 값**으로 `fillna`하면 발생하는 에러. 해결은 dtype을 `object`로 바꾼 뒤 채우기. (6/19 라이브 pandas 보충)

---

## 왜 필요한가

`category` 타입은 메모리 절약·성능을 위해 **허용된 카테고리 목록을 고정**한다. 그래서 목록에 없는 값으로 결측치를 채우려 하면 거부당한다. 결측 처리에서 흔히 만나는 함정.

---

## 어떻게 작동하나

```python
df = titanic.copy()
df['deck'] = df['deck'].astype('object')   # ✅ category → object 변환
df['deck'] = df['deck'].fillna('ABC')      # 변환 후엔 정상

# 변환 전 바로 fillna('ABC') 하면:
# TypeError: Cannot setitem on a Categorical with a new category (ABC),
#            set the categories first
```

---

## 자주 헷갈리는 것

**두 해결책 중:** 방법1=`cat.add_categories('ABC')`로 카테고리 먼저 등록(복잡, 잘 안 씀) / **방법2=`astype('object')`로 변환 후 채우기**(강사 권장). 보통 방법2가 간단하다.

---

## 더 알면 좋은 것

📌 `seaborn`의 titanic `deck` 같은 컬럼이 기본 category라 자주 걸린다. 결측 채우기 전 `df.info()`로 dtype을 확인하는 습관이 좋다.

---

## 관련 개념

- [[결측치]] — 결측 처리 전략
- [[DataFrame]] — dtype 관리
- [[pandas]] — category/object 타입
