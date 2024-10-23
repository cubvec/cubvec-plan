# CUBVEC Planning and Product Management

## 마일스톤 1: 벡터 데이터 저장 및 조회

### 1. 벡터 데이터 저장 및 조회

- 벡터 데이터를 저장할 수 있다.
- `csql`을 사용하여 벡터 데이터를 조회할 수 있다. (전체 테이블 스캔)

### 2. 대용량 벡터 데이터 벌크 로딩 및 동등 연산자 제공

- `csql`을 통해 대용량 벡터 데이터를 벌크 로딩할 수 있다.  
  _(벌크 로딩 도구는 검토 후 변경 가능)_
- 벡터 타입에 대해 `=` 연산자를 제공한다.
  - 예시: `SELECT * FROM tbl1 WHERE col1 = '[10, 20, 30]';`
  - _(우선순위 변경 가능)_

### 3. k-NN 검색 기능 추가

- k-NN 검색을 수행할 수 있다. (유클리드 거리 기반)
  - kd-tree 사용 _(고려 대상)_
  - _(성능 측정 시 충돌(crash)이 발생하면 재구성이 필수)_

---

## 마일스톤 2: HNSW 인덱스 생성 및 관리

### 1. HNSW 인덱스 생성

- HNSW 인덱스를 생성할 수 있다. (유클리드 거리 기반)
  - HNSW 오픈소스 포팅
- `csql`을 사용해 인덱스 정보를 조회할 수 있다. (유클리드 거리 기반)
  - _(우선순위 변경 가능)_

### 2. HNSW 인덱스 빌드 및 데이터 추가

- HNSW 인덱스를 빌드할 수 있다. (유클리드 거리 기반)
  - 데이터 포함 및 데이터 제외 모두 가능
  - 메모리 기반과 디스크 기반 선택에 따라 인덱스 복구 메커니즘에 영향이 있음
- HNSW 인덱스에 데이터를 추가할 수 있다. (유클리드 거리 기반)
- k-ANN 검색을 수행할 수 있다. (유클리드 거리 기반)
  - _(성능 측정 시 충돌(crash)이 발생하면 재구성이 필수)_
  - 기한: ~내년 6월

---

## 마일스톤 3: HNSW 인덱스 복구 및 트랜잭션 처리

### 1. HNSW 인덱스 복구

- HNSW 인덱스를 복구할 수 있다. (대용량 데이터)
  - 메모리 기반 HNSW 인덱스:
    - 서버 재시작 시 재빌드 가능
    - 또는 메모리 기반 복구 메커니즘 개발 필요
  - 디스크 기반 HNSW 인덱스:
    - 디스크에 HNSW 인덱스 저장 구조 정의
    - HNSW 인덱스의 REDO/UNDO 메커니즘 추가
  - _(QA 팀 성능 측정 시점)_

### 2. 트랜잭션 롤백 및 동시성 지원

- 트랜잭션이 HNSW 인덱스의 변경 사항을 롤백할 수 있다.
- HNSW 인덱스는 트랜잭션 동시성(MVCC)을 지원한다.
  - _(성능 측정에 영향을 줄 수 있는 요소)_
  - _(제품 릴리즈 시 필수, 국책 과제 보고 시 선택)_
  - _(우선순위 변경 가능)_

---

## 마일스톤 4: 다양한 거리 연산자 및 함수 지원

### 1. 다양한 거리 연산자 지원

- 다양한 거리 연산자를 지원할 수 있다:
  - `<#>`: negative inner product
  - `<=>`: 코사인 거리
  - `<+>`: L1 거리, 등

### 2. 다양한 거리 함수 지원

- 다양한 거리 함수를 지원할 수 있다:
  - `l2_distance()`: 유클리드 거리
  - `inner_product()`
  - `cosine_distance()`
  - `l1_distance()`: 택시캡 거리, 등

### 3. HNSW 인덱스 생성 시 거리 함수 선택

- HNSW 인덱스 생성 시 거리 함수를 선택할 수 있다.

### 4. 기타 벡터 함수 지원

- 기타 벡터 함수를 지원할 수 있다:
  - `vector_dims()`
  - `vector_norm()`, 등

---

## 기타 작업 (업데이트 예정)

- 벡터 데이터 갱신, 삭제, 업서트(upsert) 기능.
- 스키마 변경 (벡터 타입 컬럼 추가/제거).
- 벡터 타입 이항 연산자 (`+`, `-`, `*`, `||`, 등) 제공.
- 벡터 타입 집계 함수 (`AVG()`, `SUM()`, 등) 제공.
- SQL 힌트.
- SQL 실행 최적화.
- 다양한 벡터 타입 지원.
- 다양한 인덱스 지원 (예: IVFFlat).
- 벡터 관련 시스템 카탈로그 지원.
- JSON, CLOB 등으로 변환 지원.
- 에러 처리:
  - 잘못된 차원의 벡터 데이터 삽입.
  - 벡터 타입이 아닌 컬럼에 벡터 인덱스 생성.
  - 벡터 타입 컬럼에 B-트리 인덱스 생성, 등.
- 제약 사항 처리:
  - 벡터 타입 컬럼에 PK, FK 등 제약 조건 생성.
  - 벡터 타입 기준 파티션 테이블 생성.
  - 벡터 타입 컬럼을 조인 조건으로 사용, 등.
- JDBC 및 기타 드라이버.
- 성능 모니터링/디버깅.
- 다른 DB와의 호환성.
- 마이그레이션.

---

## 포함되지 않음

- D사의 인덱스 포함

---

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Milestone 1

### 1. Vector Data Storage and Querying

- Vector data can be stored.
- Can query vector data with `csql` (full table scan).

### 2. Bulk Loading and Equality Operator for Vector Data

- Can bulk load a large volume of vector data via `csql`.  
  _(Bulk loading tool can be changed after review.)_
- Provides `=` operator for vector types in `SELECT` statements.
  - Example: `SELECT * FROM tbl1 WHERE col1 = '[10, 20, 30]';`
  - _(Priority may change.)_

### 3. k-NN Search

- Can perform k-NN search (Euclidean distance).
  - Using kd-tree _(to be considered)_.
  - _(If performance measurement causes crashes, restructuring is mandatory.)_

---

## Milestone 2

### 1. HNSW Index Creation

- Can create HNSW index (Euclidean distance).
  - HNSW open-source porting.
- Can query index information via `csql` (Euclidean distance).
  - _(Priority may change.)_

### 2. HNSW Index Building and Data Insertion

- Can build HNSW index (Euclidean distance).
  - With data and without data.
  - Need to choose between memory-based and disk-based, affecting index recovery mechanisms.
- Can add data to HNSW index (Euclidean distance).
- Can perform k-ANN search (Euclidean distance).
  - _(If performance measurement causes crashes, restructuring is mandatory.)_
  - Deadline: ~Next June

---

## Milestone 3

### 1. HNSW Index Recovery

- Can recover HNSW index (large data volumes).
  - Memory-based HNSW index:
    - Rebuild on server restart.
    - Or develop memory-based recovery mechanism.
  - Disk-based HNSW index:
    - Define disk storage structure for HNSW index.
    - Add REDO/UNDO mechanism for HNSW index.
  - _(QA team performance measurement point.)_

### 2. Transactional Rollback and Concurrency

- Can rollback HNSW index changes with transactions.
- HNSW index supports transaction concurrency (MVCC).
  - _(May affect performance measurement.)_
  - _(Mandatory for product release; optional for national project reporting.)_
  - _(Priority may change.)_

---

## Milestone 4

### 1. Support for Various Distance Operators

- Can support various distance operators:
  - `<#>`: negative inner product
  - `<=>`: cosine distance
  - `<+>`: L1 distance, etc.

### 2. Support for Various Distance Functions

- Can support various distance functions:
  - `l2_distance()`: Euclidean distance
  - `inner_product()`
  - `cosine_distance()`
  - `l1_distance()`: taxicab distance, etc.

### 3. Distance Function Selection for HNSW Index Creation

- Can choose distance function when creating HNSW index.

### 4. Support for Other Vector Functions

- Can support additional vector functions:
  - `vector_dims()`
  - `vector_norm()`, etc.

---

## Other Tasks (To Be Updated)

- Vector data update, delete, upsert.
- Schema changes (adding/removing vector type columns).
- Vector type binary operators (`+`, `-`, `*`, `||`, etc.).
- Vector type aggregate functions (`AVG()`, `SUM()`, etc.).
- SQL hints.
- SQL execution optimization.
- Support for various vector types.
- Support for various indexes (e.g., IVFFlat).
- Support for vector-related system catalogs.
- Conversion to JSON, CLOB, etc.
- Error handling:
  - Insertion of incorrect dimension vector data.
  - Creation of vector index on non-vector column.
  - Creation of B-tree index on vector type column, etc.
- Constraint handling:
  - Creation of PK, FK constraints on vector type columns.
  - Creation of partition tables based on vector type columns.
  - Using vector type columns as join conditions, etc.
- JDBC and other drivers.
- Performance monitoring/debugging.
- Compatibility with other databases.
- Migration.

---

### Not Included
