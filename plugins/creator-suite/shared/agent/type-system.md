# 서브 에이전트 타입 시스템

서브 에이전트의 3가지 타입(Specialist, Analyst, Orchestrator)에 대한 상세 설명과 선택 가이드입니다.

---

## 📊 타입 비교 요약

| 타입 | 복잡도 | 단어 수 | 용도 | 권장 모델 |
|------|--------|---------|------|-----------|
| **Specialist** | 낮음 | 100-300 | 단일 작업 전문 | inherit, haiku |
| **Analyst** | 중간 | 300-800 | 분석/리뷰 | inherit, sonnet |
| **Orchestrator** | 높음 | 800-2000+ | 복잡한 워크플로우 | sonnet, opus |

---

## 🔧 Specialist (전문가)

### 개요

**목적**: 단일 작업에 특화된 전문 에이전트

**특징**:
- ✅ 한 가지 작업을 빠르고 정확하게 수행
- ✅ 명확한 입력 → 처리 → 출력 흐름
- ✅ 간단한 프롬프트 구조
- ✅ 빠른 실행 시간

**권장 단어 수**: 100-300 단어

**권장 모델**: `inherit` 또는 `haiku`

---

### 적합한 사용 사례

- ✅ 코드 포맷팅 (ESLint, Prettier, Black)
- ✅ 링크 검증
- ✅ 테스트 실행
- ✅ 파일 정리
- ✅ 간단한 변환 작업
- ✅ 빠른 검증 작업

---

### 부적합한 사용 사례

- ❌ 코드 리뷰 (Analyst 사용)
- ❌ 배포 프로세스 (Orchestrator 사용)
- ❌ 다단계 분석
- ❌ 복잡한 의사결정

---

### 프롬프트 구조

```markdown
---
name: {agent-name}
description: "{간단한 설명}"
tools: {필요한_최소_도구}
model: inherit
---

# {Agent Name}

{1-2문장 역할 설명}

## Role
{구체적인 역할 정의}

## Triggers
{트리거 조건 3-5개}

## Behavioral Guidelines
1. **{단계1}**: {설명}
2. **{단계2}**: {설명}
3. **{단계3}**: {설명}

## Output Format
```
{출력 예시}
```

## Boundaries

**Will:**
- {수행할 작업}

**Will Not:**
- {금지 작업}
```

---

### 실제 예시: ESLint Enforcer

```yaml
---
name: eslint-enforcer
description: "JavaScript/TypeScript 파일에 ESLint 규칙을 자동으로 적용"
tools: Read,Write,Bash
model: haiku
---
```

**복잡도**: 낮음
**단어 수**: 약 200 단어
**실행 시간**: < 30초

---

## 📊 Analyst (분석가)

### 개요

**목적**: 코드 분석, 리뷰, 평가를 수행하는 전문 에이전트

**특징**:
- ✅ 여러 관점에서 종합적 분석
- ✅ 구체적인 피드백과 예제 제공
- ✅ 심각도별 분류
- ✅ 건설적인 개선 제안

**권장 단어 수**: 300-800 단어

**권장 모델**: `inherit` 또는 `sonnet`

---

### 적합한 사용 사례

- ✅ 코드 리뷰
- ✅ 보안 감사
- ✅ 성능 분석
- ✅ 아키텍처 평가
- ✅ 테스트 커버리지 검토
- ✅ 문서 품질 평가

---

### 부적합한 사용 사례

- ❌ 코드 자동 수정 (Specialist 사용)
- ❌ 복잡한 배포 (Orchestrator 사용)
- ❌ 단순 포맷팅 (Specialist 사용)

---

### 프롬프트 구조

```markdown
---
name: {agent-name}
description: "{분석 역할 설명}"
tools: Read,Grep,Bash
model: sonnet
---

# {Agent Name}

{2-3문장 전문성 설명}

## Role
{상세한 역할 및 책임}

## Expertise Areas
- {전문_분야_1}
- {전문_분야_2}
- {전문_분야_3}
- {전문_분야_4}

## Triggers
{자동 실행 조건}

## Analysis Process
1. **Phase 1**: {설명}
2. **Phase 2**: {설명}
3. **Phase 3**: {설명}
4. **Phase 4**: {설명}

## Output Format
```
{구조화된 분석 결과}
```

## Analysis Standards
- **Critical**: {기준}
- **High**: {기준}
- **Medium**: {기준}
- **Low**: {기준}

## Boundaries

**Will:**
- {수행할 분석}

**Will Not:**
- {금지 작업 (수정 금지)}
```

---

### 실제 예시: Code Reviewer

```yaml
---
name: code-reviewer
description: "TypeScript와 Python 코드의 품질, 보안, 성능을 종합 검증"
tools: Read,Grep,Bash
model: sonnet
---
```

**복잡도**: 중간
**단어 수**: 약 600 단어
**분석 깊이**: 4단계 프로세스

**주요 특징**:
- 다단계 분석 프로세스
- 심각도별 이슈 분류
- 구체적인 코드 예시 제공
- 읽기 전용 (수정하지 않음)

---

## 🎯 Orchestrator (조율자)

### 개요

**목적**: 복잡한 다단계 워크플로우를 조율하는 고급 에이전트

**특징**:
- ✅ 여러 Phase로 구성된 워크플로우
- ✅ 다양한 도구 조율
- ✅ 에러 처리 및 롤백
- ✅ 안전 검사 및 검증
- ✅ 상세한 진행 상황 보고

**권장 단어 수**: 800-2000+ 단어

**권장 모델**: `sonnet` 또는 `opus`

---

### 적합한 사용 사례

- ✅ CI/CD 파이프라인
- ✅ 릴리스 관리
- ✅ 배포 프로세스
- ✅ 데이터 마이그레이션
- ✅ 시스템 초기화
- ✅ 복잡한 테스트 시나리오

---

### 부적합한 사용 사례

- ❌ 단순 포맷팅 (Specialist 사용)
- ❌ 단일 분석 (Analyst 사용)

---

### 프롬프트 구조

```markdown
---
name: {agent-name}
description: "{워크플로우 설명}"
tools: Read,Write,Bash
model: sonnet
---

# {Agent Name}

{2-3문장 조율 역할 설명}

## Role
{상세 역할 및 관리 범위}

## Responsibilities
{책임 목록 5-8개}

## Triggers
{워크플로우 시작 조건}

## Workflow Phases

### Phase 1: {Phase명}
{상세 설명}

**Steps:**
1. {단계}
2. {단계}

**Validation:**
- {검증 조건}

**Error Handling:**
- If {에러}: {처리}

### Phase 2: {Phase명}
...

## Tool Coordination
{도구 사용 전략}

## Output Format
```
{진행 상황 보고서}
```

## Error Handling
{에러 시나리오 및 복구 전략}

## Boundaries

**Will:**
- {수행할 워크플로우}

**Will Not:**
- {금지 작업}

**Safety Checks:**
- {안전 검사}
```

---

### 실제 예시: Release Manager

```yaml
---
name: release-manager
description: "npm 패키지의 전체 릴리스 프로세스를 관리"
tools: Read,Write,Bash
model: sonnet
---
```

**복잡도**: 높음
**단어 수**: 약 1,500 단어
**워크플로우**: 5단계 Phase

**주요 특징**:
- Pre-release 검증
- 버전 번호 업데이트
- Changelog 자동 생성
- 빌드 및 테스트
- 배포 및 검증
- 에러 시 자동 롤백

---

## 🔀 타입 선택 가이드

### 의사 결정 트리

```
작업의 성격은?
│
├─ 단일, 명확한 작업?
│  └─ Specialist
│     예: 포맷팅, 린팅, 링크 체크
│
├─ 분석 및 리뷰?
│  └─ Analyst
│     예: 코드 리뷰, 보안 감사, 성능 분석
│
└─ 다단계 워크플로우?
   └─ Orchestrator
      예: 배포, 릴리스, CI/CD
```

---

### 질문으로 타입 결정하기

**질문 1**: 작업이 3단계를 초과하나요?
- **No** → Specialist 고려
- **Yes** → 다음 질문으로

**질문 2**: 주요 목적이 분석/리뷰인가요?
- **Yes** → Analyst
- **No** → 다음 질문으로

**질문 3**: 여러 도구를 조율해야 하나요?
- **Yes** → Orchestrator
- **No** → Specialist

**질문 4**: 에러 처리 및 롤백이 필요한가요?
- **Yes** → Orchestrator
- **No** → Specialist 또는 Analyst

---

## 📊 타입별 세부 비교

### 실행 시간

| 타입 | 예상 실행 시간 | 이유 |
|------|----------------|------|
| Specialist | < 30초 | 단순하고 빠름 |
| Analyst | 1-3분 | 분석 시간 필요 |
| Orchestrator | 5-15분 | 다단계 워크플로우 |

---

### 프롬프트 복잡도

| 타입 | 섹션 수 | 단어 수 | 구조 |
|------|---------|---------|------|
| Specialist | 5-6개 | 100-300 | 단순 |
| Analyst | 7-9개 | 300-800 | 구조화 |
| Orchestrator | 9-12개 | 800-2000+ | 매우 복잡 |

---

### 도구 사용 패턴

| 타입 | 일반적인 도구 조합 | 설명 |
|------|-------------------|------|
| Specialist | Read, Write, Bash | 읽기 + 실행 또는 쓰기 |
| Analyst | Read, Grep, Bash | 읽기 + 검색 + 검증 |
| Orchestrator | Read, Write, Bash | 모든 도구 조율 |

---

### 에러 처리 수준

| 타입 | 에러 처리 | 복구 전략 |
|------|-----------|-----------|
| Specialist | 기본 | 에러 보고 |
| Analyst | 중간 | 에러 보고 + 제안 |
| Orchestrator | 고급 | 자동 롤백 + 복구 |

---

## 💡 실전 선택 팁

### Specialist 선택 시

✅ **이렇게 판단하세요**:
- "이 작업을 한 문장으로 설명할 수 있나요?"
- "실행 단계가 3개 이하인가요?"
- "30초 안에 완료될 수 있나요?"

**모두 Yes** → Specialist

---

### Analyst 선택 시

✅ **이렇게 판단하세요**:
- "분석 결과를 심각도별로 분류해야 하나요?"
- "여러 관점에서 평가해야 하나요?"
- "코드를 수정하지 않고 리뷰만 하나요?"

**모두 Yes** → Analyst

---

### Orchestrator 선택 시

✅ **이렇게 판단하세요**:
- "4단계 이상의 Phase가 필요한가요?"
- "에러 발생 시 이전 상태로 롤백해야 하나요?"
- "여러 도구를 순차적으로 조율해야 하나요?"
- "실패 시 큰 영향이 있나요?"

**대부분 Yes** → Orchestrator

---

## 🎓 타입별 학습 경로

### Specialist 마스터하기

1. **템플릿 연구**: `templates/specialist-template.md` 읽기
2. **간단한 예시**: ESLint Enforcer, Prettier Formatter
3. **실습**: 단순한 작업 자동화
4. **핵심 학습**: 최소 권한 원칙, 빠른 실행

---

### Analyst 마스터하기

1. **템플릿 연구**: `templates/analyst-template.md` 읽기
2. **분석 프로세스**: 4단계 분석 방법론 학습
3. **실습**: 코드 리뷰 에이전트 작성
4. **핵심 학습**: 심각도 분류, 건설적 피드백

---

### Orchestrator 마스터하기

1. **템플릿 연구**: `templates/orchestrator-template.md` 읽기
2. **워크플로우 설계**: 다단계 Phase 구조 학습
3. **실습**: 릴리스 관리 에이전트 작성
4. **핵심 학습**: 에러 처리, 롤백 전략, 도구 조율

---

## 📖 참고 자료

- **모델 선택**: `model-selection-guide.md`
- **도구 가이드**: `available-tools.md`
- **검증 기준**: `validation-criteria.md`
- **템플릿**: `templates/` 디렉토리
  - `specialist-template.md`
  - `analyst-template.md`
  - `orchestrator-template.md`
