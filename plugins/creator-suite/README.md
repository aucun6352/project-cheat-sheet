# Creator Suite

통합 콘텐츠 생성 플러그인 - Agent, Command, Skill을 하나의 플러그인에서 생성하고 관리합니다.

## 📖 개요

Creator Suite는 Claude Code 생태계를 확장하는 세 가지 핵심 콘텐츠를 하나의 플러그인에서 생성할 수 있는 통합 솔루션입니다:

- **🤖 Agent**: 특정 작업에 특화된 서브 에이전트
- **⚡ Command**: 워크플로우를 자동화하는 Slash 커맨드
- **📚 Skill**: 지식과 패턴을 문서화하는 스킬 가이드

### 왜 Creator Suite인가?

**"Claude Code가 대부분의 작업을 자동으로 해준다면, Agent, Command, Skill도 Claude가 자동으로 만들어주면 어떨까?"**

이런 생각에서 Creator Suite가 탄생했습니다.

Claude Code는 코드 작성, 디버깅, 리팩토링 등 많은 작업을 자동화합니다. 하지만 이런 자동화 도구 자체(Agent, Command, Skill)는 여전히 수동으로 만들어야 했습니다.

Creator Suite는 **도구를 만드는 도구**입니다:

- 💬 **대화만으로 생성**: 복잡한 문법 없이 대화로 Agent/Command/Skill 생성
- 🤖 **자동 분석**: 프로젝트와 대화 패턴을 분석하여 최적의 도구 추천
- ⚡ **즉시 사용**: 생성 즉시 Claude Code에서 바로 활용 가능
- 🎯 **품질 보증**: 검증으로 고품질 도구 보장

**메타 자동화**: Claude가 자동화를 더 잘하도록 도와주는 도구를 자동으로 만듭니다.

---

## 🚀 빠른 시작

### 1. Agent 생성

특정 작업에 특화된 서브 에이전트를 생성합니다.

```bash
/create-agent
```

**생성 위치**: `.claude/agents/{name}.md`

**타입**:
- **Specialist**: 단순하고 명확한 작업 (1-3개 도구)
- **Analyst**: 분석 및 검증 작업 (3-6개 도구)
- **Orchestrator**: 복잡한 다단계 조율 (6개 이상 도구)

**예시**:
```bash
/create-agent

# 대화형 프롬프트:
Name: api-integration-specialist
Description: REST API 통합 및 에러 처리 전문 에이전트
Type: Specialist
Tools: Bash, Read, Write
Model: sonnet
```

### 2. Command 생성

반복적인 워크플로우를 Slash 커맨드로 자동화합니다.

```bash
/create-command
```

**생성 위치**: `.claude/commands/{name}.md` 또는 `plugins/*/commands/{name}.md`

**타입**:
- **Simple Task Command**: 단일 작업 (50-150 단어)
- **Workflow Pipeline Command**: 순차 실행 (150-400 단어)
- **Complex Orchestration Command**: 복잡한 조건/반복 (400+ 단어)

**예시**:
```bash
/create-command

# 대화형 프롬프트:
Name: code-review
Description: 타입 체크, 테스트, 린트를 순차 실행하는 코드 리뷰 워크플로우
Type: Workflow Pipeline Command
```

### 3. Skill 생성

팀 지식과 패턴을 문서화합니다.

```bash
/create-skill
```

**생성 위치**: `{skill-name}/SKILL.md`

**타입**:
- **Quick Workflow**: 간단한 팁/트릭 (50-200 단어)
- **Comprehensive Guide**: 단계별 가이드 (200-600 단어)
- **Deep Dive**: 심층 분석 (600-1500 단어)
- **Pattern Library**: 패턴 컬렉션 (1500+ 단어)

**예시**:
```bash
/create-skill

# 대화형 프롬프트:
Name: react-hooks-patterns
Description: React Hooks 사용 패턴과 모범 사례
Type: Comprehensive Guide
```

### 4. 프로젝트 자동 분석 및 생성

프로젝트를 분석하여 맞춤형 Agent와 Skill을 자동으로 추천하고 생성합니다.

```bash
/project-tooling
```

**작동 방식**:
1. **프로젝트 분석**: 메타 파일, 기술 스택, 비즈니스 도메인 자동 감지
2. **맞춤형 추천**: 프로젝트 특화 Agent/Skill을 우선순위별로 추천
3. **사용자 선택**: 추천 목록에서 생성할 항목 선택
4. **자동 생성**: `/create-agent`, `/create-skill` 자동 호출

**예시 시나리오**:
```bash
# E-commerce 프로젝트에서 실행
/project-tooling

# 자동 감지:
# - 언어: TypeScript, Python
# - 프레임워크: React, FastAPI
# - 비즈니스 키워드: payment, order, cart

# 추천 결과:
# ⭐ order-validation-specialist (프로젝트 고유)
# ⭐ payment-integration-coordinator (도메인 특화)
# inventory-sync-manager (도메인 특화)
# typescript-code-reviewer (범용)
```

---

## 📚 핵심 개념

### Phase 시스템

모든 생성 커맨드는 일관된 5단계 Phase 시스템을 따릅니다:

```
Phase 0: 대화 분석 (조건부)
  ↓
Phase 1: 기본 정보 수집
  ↓
Phase 2: 세부 내용 작성
  ↓
Phase 3: 예제 및 추가 정보
  ↓
Phase 4: 파일 생성 및 검증
```

**상세 문서**: `@shared/core/phase-system.md`

### 대화 기반 자동 추출

Phase 0에서는 현재 세션의 대화 히스토리를 분석하여:
- 워크플로우 패턴 자동 추출
- 메타데이터 자동 생성 (name, description, type)
- 추천 모드로 빠른 생성 지원

**상세 문서**: `@shared/core/conversation-analyzer.md`

### 100점 만점 품질 검증

Phase 4에서 생성된 문서를 자동으로 검증합니다:

| 영역 | 배점 |
|------|------|
| 구조 검증 | 20점 |
| Frontmatter 검증 | 15-20점 |
| 필수 섹션 검증 | 20-25점 |
| 콘텐츠 품질 검증 | 25점 |
| 타입 적합성 | 10점 |
| 완성도 | 5-10점 |

**등급 체계**:
- A (90-100점): 우수
- B (80-89점): 좋음
- C (70-79점): 보통
- D (60-69점): 미흡
- F (<60점): 불합격

**상세 문서**: `@shared/core/validation-engine.md`

---

## 🎯 사용 시나리오

### 시나리오 1: 반복 작업 자동화

```bash
# 문제: 매번 API 테스트할 때 인증 → 호출 → 검증 반복

# 해결 1: Agent 생성
/create-agent
Name: api-test-specialist
Type: Specialist

# 해결 2: Command 생성
/create-command
Name: test-api
Type: Simple Task Command
```

### 시나리오 2: 팀 지식 문서화

```bash
# 문제: 프로젝트 특화 패턴을 팀원들과 공유하고 싶음

# 해결: Skill 생성
/create-skill
Name: project-architecture-patterns
Type: Comprehensive Guide

# 사용: 대화에서 @project-architecture-patterns/SKILL.md 참조
```

### 시나리오 3: 새 프로젝트 셋업

```bash
# 문제: 새 프로젝트에 맞춤형 도구가 필요

# 해결: 프로젝트 자동 분석
/project-tooling

# 결과: 프로젝트 특화 Agent 3개, Skill 2개 자동 추천 및 생성
```

---

## 📖 고급 사용법

### 대화 기반 생성

현재 대화 내용을 바탕으로 자동 생성:

```
User: "코드 리뷰할 때 항상 먼저 타입 체크하고, 테스트 실행하고, 린트 검사해"