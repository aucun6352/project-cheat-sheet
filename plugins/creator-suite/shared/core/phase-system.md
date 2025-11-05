# Phase System - 5단계 대화형 생성 프로세스

Creator Suite의 모든 생성 커맨드는 일관된 5단계 Phase 시스템을 사용합니다.

## 개요

Phase 시스템은 사용자와의 대화를 통해 점진적으로 정보를 수집하고, 최종적으로 고품질의 문서를 생성하는 구조화된 프로세스입니다.

## 5단계 Phase 구조

### Phase 0: 대화 분석 (조건부)

**목적**: 현재 세션의 대화 히스토리를 분석하여 자동으로 정보를 추출합니다.

**실행 조건**:
- 대화 히스토리가 존재할 때
- 사용자가 명시적으로 대화 기반 생성을 요청했을 때

**프로세스**:
1. 현재 세션의 대화 히스토리 확인
2. 워크플로우, 패턴, 요구사항 자동 추출
3. 메타데이터 자동 생성 (name, description, type 등)
4. 추출된 정보를 사용자에게 제시

**분기**:
- **추천 모드**: 추출된 정보를 기반으로 자동 완성
- **직접 입력 모드**: 사용자가 수동으로 모든 정보 입력

**구현 참조**: `@conversation-analyzer.md`

---

### Phase 1: 기본 정보 수집

**목적**: 생성할 문서의 핵심 메타데이터를 수집합니다.

**수집 항목**:
- **Name**: kebab-case 형식의 고유 이름
- **Description**: 한 문장으로 된 명확한 설명
- **Type**: 생성 대상의 유형 (Agent/Command/Skill별로 다름)

**검증**:
- Name: kebab-case 형식 검증 (소문자, 숫자, 하이픈만 허용)
- Description: 비어있지 않은지 확인
- Type: 유효한 타입 목록에서 선택되었는지 확인

**예시 (Agent)**:
```
Name: api-integration-specialist
Description: REST API 통합 및 에러 처리 전문 에이전트
Type: Specialist
```

---

### Phase 2: 세부 내용 작성

**목적**: 생성 대상의 핵심 동작과 특성을 정의합니다.

**타입별 세부 항목**:

#### Agent
- **Instructions**: 에이전트의 행동 지침
- **Tools**: 사용 가능한 도구 목록
- **Model**: 사용할 모델 (inherit/sonnet/opus/haiku)

#### Command
- **Triggers**: 커맨드가 실행되는 조건
- **Behavioral Flow**: 실행 단계 및 로직
- **Boundaries**: 실행 범위 및 제약사항

#### Skill
- **Core Concept**: 핵심 개념 설명
- **Practical Application**: 실제 적용 방법
- **Progressive Disclosure**: 단계별 학습 구조

**작성 가이드**:
- 명확하고 구체적으로 작성
- 타입별 템플릿 참조 가능
- 기존 예제를 바탕으로 작성 권장

---

### Phase 3: 예제 및 추가 정보

**목적**: 사용성을 높이기 위한 예제와 추가 문서를 작성합니다.

**선택적 항목**:
- **Examples**: 사용 예시 및 시나리오
- **Best Practices**: 모범 사례 및 권장사항
- **Troubleshooting**: 일반적인 문제 및 해결 방법
- **Related Resources**: 관련 문서 및 참조 링크

**작성 전략**:
- 최소 1-2개의 구체적인 예제 포함
- 실제 사용 시나리오 중심으로 작성
- 일반적인 오류 케이스 포함

---

### Phase 4: 파일 생성 및 검증

**목적**: 수집된 정보를 바탕으로 최종 파일을 생성하고 품질을 검증합니다.

**프로세스**:
1. **파일 경로 결정**:
   - Agent: `.claude/agents/{name}.md`
   - Command: `.claude/commands/{name}.md` 또는 `plugins/*/commands/{name}.md`
   - Skill: `{name}/SKILL.md`

2. **Frontmatter 생성**:
   ```yaml
   ---
   name: {name}
   description: "{description}"
   # 타입별 추가 필드
   ---
   ```

3. **본문 구조화**:
   - Phase 2, 3에서 수집한 내용을 타입별 템플릿에 맞춰 구조화
   - 섹션 헤더, 포맷팅 적용

4. **파일 작성**:
   - Write 도구를 사용하여 최종 파일 생성

5. **품질 검증**:
   - 검증 엔진 실행 (100점 만점)
   - A/B/C/D/F 등급 산출
   - 검증 결과를 사용자에게 보고

**검증 구현 참조**: `@validation-engine.md`

---

## Phase 간 전환 규칙

### 순방향 전환
```
Phase 0 → Phase 1 → Phase 2 → Phase 3 → Phase 4
```

각 Phase는 순차적으로 진행되며, 이전 Phase의 정보를 바탕으로 다음 Phase가 진행됩니다.

### 역방향 전환 (수정)
```
Phase N → Phase N-1 (사용자 요청 시)
```

사용자가 이전 단계의 정보를 수정하고 싶을 때 역방향 전환이 가능합니다.

### 건너뛰기
```
Phase 0 → Phase 1 (대화 히스토리 없음)
Phase 3 → Phase 4 (예제 생략)
```

일부 Phase는 조건에 따라 건너뛸 수 있습니다.

---

## 사용자 경험 패턴

### 대화형 프롬프트
각 Phase에서 사용자에게 명확한 안내를 제공합니다:

```
🔷 Phase 1: 기본 정보 수집

다음 정보를 입력해주세요:

1. Name (kebab-case):
2. Description (한 줄 설명):
3. Type (선택):
   - Option 1
   - Option 2
   - Option 3
```

### 진행 상태 표시
현재 Phase와 전체 진행 상황을 표시합니다:

```
📊 진행 상황: Phase 2/4 (50%)
```

### 확인 및 수정 기회
각 Phase 완료 시 사용자에게 확인 기회를 제공합니다:

```
✅ Phase 1 완료

입력하신 정보:
- Name: api-specialist
- Description: REST API 통합 전문가
- Type: Specialist

계속 진행하시겠습니까? (yes/수정할 항목 번호)
```

---

## 에러 처리

### 입력 검증 실패
```
❌ Name 검증 실패: kebab-case 형식이 아닙니다.
   입력값: "API_Specialist"
   올바른 형식: "api-specialist"

다시 입력해주세요:
```

### Phase 중단
사용자가 언제든지 프로세스를 중단하고 나중에 재개할 수 있습니다:

```
⏸️ 프로세스 일시 중단

현재 진행 상황이 저장되었습니다.
나중에 동일한 커맨드를 실행하여 이어서 진행할 수 있습니다.
```

---

## 타입별 차이점

### Agent 생성
- Phase 2에서 도구 선택 인터페이스 제공
- 모델 선택 옵션 포함
- 템플릿 기반 Instructions 작성 지원

### Command 생성
- Triggers, Flow, Boundaries 3단계 구조
- Tool Coordination 섹션 추가
- 워크플로우 다이어그램 지원

### Skill 생성
- Progressive Disclosure 구조 강조
- 학습 단계별 섹션 구성
- 실습 예제 중심 작성

---

## 베스트 프랙티스

### Phase 설계 원칙
1. **점진적 공개**: 한 번에 모든 정보를 요구하지 않음
2. **명확한 안내**: 각 입력의 목적과 예시 제공
3. **유연한 수정**: 언제든지 이전 단계로 돌아가 수정 가능
4. **품질 검증**: 최종 단계에서 자동 검증 및 피드백

### 사용자 입력 처리
1. **기본값 제공**: 가능한 경우 합리적인 기본값 제시
2. **자동 완성**: 대화 분석을 통한 자동 채우기 지원
3. **템플릿 활용**: 타입별 템플릿으로 작성 부담 감소
4. **검증 피드백**: 입력 오류 시 즉시 명확한 피드백

### 문서 품질
1. **구조화**: 일관된 섹션 구조 유지
2. **완성도**: 모든 필수 섹션 포함 확인
3. **명확성**: 모호하지 않은 설명 작성
4. **실용성**: 실제 사용 가능한 예제 포함

---

## 관련 문서

- `@conversation-analyzer.md`: Phase 0 대화 분석 상세 구현
- `@validation-engine.md`: Phase 4 품질 검증 상세 구현
- `@../agent/type-system.md`: Agent 타입별 Phase 차이
- `@../command/type-system.md`: Command 타입별 Phase 차이
- `@../skill/type-system.md`: Skill 타입별 Phase 차이
