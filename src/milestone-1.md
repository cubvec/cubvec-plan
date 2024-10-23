# Milestone 1: 벡터 데이터 저장 및 조회

| 구분 | 대분류       | 중분류         | 소분류                                  | 설명                                                                       | 이슈 번호 |
| ---- | ------------ | -------------- | --------------------------------------- | -------------------------------------------------------------------------- | --------- |
| 1    | 데이터 타입  | 벡터 타입 추가 | VECTOR (DIM, TYPE)                      |                                                                            |           |
|      |              | 타입 변환      | 문자열 to 벡터 타입                     | "varchar -> vector<br>insert 시 '[10, 20, 30]' -> [10, 20, 30]"            |           |
|      |              |                | 벡터 타입 to 문자열                     | "vector -> varchar<br>select 시 [10, 20, 30] -> '[10, 20, 30]'"            |           |
|      | 파서(syntax) | 클래스         | 벡터 타입 컬럼 처리                     | "클래스 생성 시 벡터 타입 처리<br>create table tbl1 (col1 vector(3));"     |           |
|      |              | 데이터         | 벡터 데이터 처리                        | "문자열을 벡터 데이터로 변환<br>insert into tbl1 values ('[10, 20, 30]');" |           |
|      | 힙/로케이터  | 클래스         | 벡터 타입 컬럼 정보 저장                | "스키마 정보 저장<br>메모리 표현 구조 - 디스크 표현 구조 (직렬화)"         |           |
|      |              |                | 벡터 타입 컬럼 정보 조회                | "스키마 정보 참조<br>디스크 표현 구조 - 메모리 표현 구조 (역직렬화)"       |           |
|      |              | 데이터         | 벡터 데이터 삽입                        | 벡터 데이터 직렬화                                                         |           |
|      |              |                | 벡터 데이터 조회                        | 벡터 데이터 역직렬화                                                       |           |
|      | 유틸리티     | csql           | 벡터 타입 컬럼 정보 조회                | desc tbl1;                                                                 |           |
|      |              |                | 벡터 타입 데이터 조회 (full table scan) | "stdout에 벡터 데이터 출력<br>col1<br>---<br>[10, 20, 30]<br>[20, 30, 40]" |           |

## 위 이슈들을 이루기 위한 사전 조사 및 설계

### Survey Issues

#### CUBVEC

-