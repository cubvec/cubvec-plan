# **[Survey] PT_TYPE_SEQUENCE 및 DB_TYPE_SEQUENCE 조사**

### Goals

- **PT_TYPE_SEQUENCE와 DB_TYPE_SEQUENCE 분석**: Cubrid의 기존 `PT_TYPE_SEQUENCE` 및 `DB_TYPE_SEQUENCE` 구현 방식을 분석하여, `PT_TYPE_VECTOR`와의 연결 및 확장 가능성을 탐색합니다.
- **필요 파일 수정 항목 정의**: `PT_TYPE_VECTOR`를 추가하고 연동하기 위해 수정이 필요한 파일 및 코드를 식별하고 목록화합니다.

### Description

이 조사는 CubVec 프로젝트에서 `PT_TYPE_SEQUENCE`와 `DB_TYPE_SEQUENCE` 구조를 분석하여 새로운 벡터 타입(`PT_TYPE_VECTOR`)의 추가 가능성을 평가하는 데 목적이 있습니다. PT_TYPE 및 DB_TYPE 구조의 변경을 통해 파서 단에서 벡터 타입을 지원하고, Cubrid 엔진에 벡터 데이터 타입을 연동하는 첫 단계입니다.

#### 조사를 통해 다룰 사항

1. **현재 `PT_TYPE_SEQUENCE` 및 `DB_TYPE_SEQUENCE`의 사용 위치와 기능**:

   - `grep` 명령어를 통해 `PT_TYPE_SEQUENCE`와 `DB_TYPE_SEQUENCE`가 참조되는 파일들을 확인하고, 각 파일의 역할과 관련된 코드 분석을 진행합니다.
   - `csql_grammar`, `parse_tree_cl`, `type_checking`, `semantic_check` 등 필수 파일들을 포함하여, 기능 구현을 위한 코드 모듈 간의 연관성을 문서화합니다.

2. **PT_TYPE 및 DB_TYPE 확장을 통한 기능 구현**:

   - `PT_TYPE_VECTOR` 추가를 위해 파서, 스토리지 엔진, 그리고 SQL 문법 분석에서 필요한 항목을 정의합니다.
   - 추가 작업이 필요한 주요 모듈 및 파일의 연결성을 조사하여, **Storage Engine의 디스크 표현 구조를 변경하지 않는 범위에서 파서 단의 기능을 구현**할 수 있도록 설계 방안을 검토합니다.

3. **정기 회의에서 논의된 핵심 사항 반영**:

   - 10월 22일 회의에서 논의된 사항을 참고하여 `PT_TYPE_VECTOR`와 `DB_TYPE_SEQUENCE` 간의 연계 및 각 파일에 대한 수정 필요성을 검토하고, 필요한 경우 회의록에 명시된 방향을 반영합니다.

### Results

조사 수행 결과는 문서 자료 형태로 제공됩니다. 문서에는 아래 항목을 포함합니다:

- `PT_TYPE_SEQUENCE` 및 `DB_TYPE_SEQUENCE`의 참조 및 연관 파일 목록
- 각 파일 내 관련 코드 설명 및 분석
- `PT_TYPE_VECTOR`와 관련된 파일 수정 항목과 향후 확장성 평가
- 필요 시, 코드 예제와 함께 기능 추가에 대한 상세 설명

**예상 산출물**:

- 분석 보고서 (pdf)
