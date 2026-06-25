---
date: 2026-06-11
tags: [sqlite3, Python, DB연결, SQLite, matplotlib, SQL, 데이터베이스]
---

# Python으로 DB 연결 (sqlite3)

**정의**: 파이썬 표준 라이브러리 `sqlite3`로 SQLite DB 파일에 접속해 SQL을 실행하고 결과를 받아 처리·시각화하는 것. **DB는 저장소, 분석·서비스는 프로그래밍 언어에서** 접속 → 쿼리 → 처리.

---

## 왜 필요한가

DB는 데이터를 보관할 뿐, 분석·시각화·웹서비스는 프로그램에서 한다. SQL 도구(DBeaver 등)로 직접 치는 것을 넘어, 파이썬에서 쿼리 결과를 받아 `matplotlib`로 그래프를 그리거나 가공한다.

---

## 어떻게 작동하나

```python
import sqlite3

def run_query(db_path, sql, params=()):
    conn = sqlite3.connect(db_path)      # ① 연결 (SQLite는 파일 경로만)
    conn.row_factory = sqlite3.Row       #    컬럼명으로 접근 가능
    cur = conn.cursor()                  # ② 커서
    cur.execute(sql, params)             # ③ 실행 (params로 안전한 바인딩)
    rows = cur.fetchall()                # ④ 결과 받기
    columns = [d[0] for d in cur.description] if cur.description else []
    conn.close()                         # ⑤ 닫기
    return columns, rows
```

- 데이터를 바꾸는 INSERT/UPDATE/DELETE 후엔 `conn.commit()` 필요.
- 시각화: 쿼리 결과 → `ax.bar(...)` / `ax.barh(...)` (matplotlib).

---

## 실제 예시

- classicmodels: 제품라인별 매출(`INNER JOIN` + `GROUP BY`) → 가로막대 그래프.
- 직접 만든 `day04_shop.db`: 4중 JOIN(고객·주문·주문상세·상품), 중첩 [[IN연산자\|IN]] 서브쿼리.

---

## 자주 헷갈리는 것

**연결을 안 닫으면 파일 잠김**: `conn.close()` 누락 시 다음 실행에서 `PermissionError [WinError 32]`(다른 프로세스가 파일 사용 중)로 DB 파일을 못 지운다. 노트북에서 `closed database` 에러가 나면 상단 connect 셀을 다시 실행.

**SQLite vs MySQL 연결**: SQLite는 **파일 기반**(경로만 주면 끝). MySQL은 **원격 연결**이라 host/user/password/port가 필요.

**재삽입 충돌**: 같은 INSERT를 두 번 돌리면 `IntegrityError: UNIQUE constraint failed`(PK 중복). 재실행 시 테이블을 비우거나 `INSERT OR IGNORE`/`INSERT OR REPLACE`.

---

## 더 알면 좋은 것

📌 `cur.execute(sql, params)`의 `params`로 값을 바인딩하면 SQL 인젝션을 막고 따옴표 처리가 안전하다(문자열 직접 결합 금지).

---

## 관련 개념

- [[SQLite]] — 파일 기반 DB
- [[JOIN]] · [[서브쿼리]] — 파이썬에서 실행하는 쿼리
- [[VIEW]] — 뷰도 동일하게 조회 가능
- [[파이썬라이브러리]] — matplotlib 등 외부 라이브러리
