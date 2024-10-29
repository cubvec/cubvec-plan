# CUBRID에서 벡터 데이터 저장 및 조회

## Description

## CUBRID의 기능 확장의 일환으로, 벡터 데이터를 저장하고 조회할 수 있도록 벡터 타입을 추가하고, 데이터 직렬화 및 역직렬화를 지원하며 벡터 연산에 대한 쿼리 및 유틸리티 기능을 강화하는 것을 목표로 합니다

## Goals

1. **벡터 데이터 타입 추가**: CUBRID에 새로운 벡터 데이터 타입 (`VECTOR (DIM, TYPE)`)을 구현합니다.
2. **데이터 직렬화 및 역직렬화**: 벡터 데이터를 직렬화하여 디스크에 저장하고 역직렬화하여 조회할 수 있도록 합니다.
3. **유틸리티 지원**: 벡터 컬럼 스키마와 데이터 출력에 대한 CSQL 유틸리티 지원을 확장합니다.

---

## Description

1. **벡터 데이터 타입 추가**: CUBRID에 새로운 벡터 데이터 타입 (`VECTOR (DIM, TYPE)`)을 구현합니다.

   - `VECTOR` 컬럼 타입 추가.
   - `INSERT INTO tbl1 VALUES ...`
   - `SELECT * from table_with_vector;`

2. **데이터 직렬화 및 역직렬화**: 벡터 데이터를 직렬화하여 디스크에 저장하고 역직렬화하여 조회할 수 있도록 합니다.
3. **유틸리티 지원**: 벡터 컬럼 스키마와 데이터 출력에 대한 CSQL 유틸리티 지원을 확장합니다.

---

## Expected Results

```sql
CREATE TABLE table_with_vector( vec VECTOR float );
```

```sql
INSERT INTO table_with_vectors VALUES ( {1, 2.0, 3.3} );
```

```sql
SELECT * FROM table_with_vector;
```

```txt
vec
---
[10, 20, 30]
[20, 30, 40]
```
