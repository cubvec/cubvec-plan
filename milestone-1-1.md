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

- 목적: 벡터 데이터 저장 및 조회

- 설명: 벡터 데이터를 저장하고 조회할 수 있도록 CUBRID에 벡터 타입을 추가한다.

- 구현 방법: 벡터 타입을 추가하고, 벡터 데이터를 직렬화하여 저장하고 역직렬화하여 조회한다.

- 검증 방법: 벡터 데이터를 저장하고 조회하는 테스트 케이스를 작성하고 검증한다.

## 사전 조사

### PT_TYPE_SEQUENCE

grep으로 PT_TYPE_SEQUENCE를 찾아보면 다음과 같이 나타납니다.

```sh
14 src/parser/csql_grammar.y
13 src/parser/type_checking.c
13 src/parser/semantic_check.c
8 src/parser/name_resolution.c
6 src/parser/parse_tree_cl.c
5 src/parser/parse_dbi.c
4 src/parser/parse_tree.h
4 src/parser/func_type.cpp
2 src/query/execute_schema.c
1 src/parser/view_transform.c
1 src/parser/method_transform.c
1 src/optimizer/query_rewrite.c
1 src/optimizer/query_graph.c
```

위 결과를 보면, PT_TYPE_SEQUENCE를 참고하여 PT_TYPE_VECTOR를 추가하기 위해서는
csql_grammar, parse_tree_cl, type_checking, semantic_check 파일들이 수정되어야 합니다.

앞 단부터 차근차근 수정해보겠습니다.

### DB_TYPE_SEQUENCE

grep으로 DB_TYPE_SEQUENCE를 찾아보면 다음과 같이 나타납니다.

```sh
21 src/object/transform.c
17 src/object/class_object.c
14 src/object/set_object.c
12 src/object/object_domain.c
12 src/compat/db_set.c
12 src/base/object_representation_sr.c
10 src/storage/catalog_class.c
10 src/query/query_opfunc.c
10 src/compat/db_macro.c
8 src/parser/parse_dbi.c
8 src/object/object_primitive.c
5 src/object/object_accessor.c
5 src/compat/dbtype_function.i
5 src/broker/cas_execute.c
4 src/query/cursor.c
4 src/object/virtual_object.c
4 src/object/object_representation.c
4 src/method/query_method.cpp
4 src/method/method_struct_value.cpp
4 src/executables/unload_object.c
3 src/parser/xasl_generation.c
3 src/object/transform_cl.c
3 src/compat/cnv.c
2 src/transaction/log_manager.c
2 src/storage/system_catalog.c
2 src/storage/statistics_sr.c
2 src/sp/jsp_cl.c
2 src/query/query_executor.c
2 src/parser/type_checking.c
2 src/object/object_template.c
2 src/method/method_query_handler.cpp
2 src/executables/unload_schema.c
2 src/executables/unload_object_file.c
2 src/compat/db_value_printer.cpp
2 src/compat/dbtype_def.h
2 src/base/memory_hash.c
2 src/api/db_value_table.c
1 src/storage/heap_file.c
1 src/storage/compactdb_sr.c
1 src/query/query_dump.c
1 src/parser/parse_tree_cl.c
1 src/object/schema_template.c
1 src/object/object_printer.cpp
1 src/object/object_domain.h
1 src/object/authenticate_grant.cpp
1 src/method/method_query_util.cpp
1 src/method/method_oid_handler.cpp
1 src/method/method_invoke_group.cpp
1 src/loaddb/load_sa_loader.cpp
1 src/loaddb/load_db_value_converter.cpp
1 src/executables/esql_translate.c
1 src/executables/csql_result_format.c
1 src/executables/compactdb.c
1 src/compat/db_vdb.c
1 src/cm_common/cm_dep_tasks.c
```

이 중 24년 10월 22일 정기 회의에서 언급된 내용에 따라,
위 두 enum이 겹치는 parser 디렉토리에 있는 파일들을 수정합니다.
새로 추가될 PT_TYPE_VECTOR와 DB_TYPE_SEQUENCE를 연결하여
Storage Engine의 디스크 표현 구조를 최대한 변경하지 않은 상태에
파서 단의 기능들을 구현해보겠습니다.

따라서, 이번 마일스톤에서는 PT_TYPE_VECTOR와 DB_TYPE_SEQUENCE를 연결하고,
PT_TYPE_VECTOR를 추가하기 위해 csql_grammar, parse_tree_cl, type_checking, semantic_check 파일들을 수정합니다.

특히, csql_grammar 파일을 수정하여 PT_TYPE_VECTOR를 추가하고,
유저로 하여금 간단한 SQL 질의를 통해 벡터 데이터를 저장하고 조회할 수 있도록 합니다.

## 진행 과정
