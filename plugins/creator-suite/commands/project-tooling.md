---
name: project-tooling
description: "프로젝트를 분석하여 필요한 역할, 기술 스택, 주요 기능을 파악한 후, 비개발적/프로젝트 고유한 내용을 우선으로 맞춤형 에이전트와 스킬을 생성하는 워크플로우"
---

# /project-tooling

프로젝트의 특성을 깊이 분석하여 맞춤형 개발 도구를 자동으로 생성하는 지능형 워크플로우입니다.

이 커맨드는 프로젝트의 메타데이터, 기술 스택, 비즈니스 로직을 분석하여 **프로젝트에 고유한** 서브 에이전트와 스킬을 우선적으로 추천합니다. 웹에서 쉽게 찾을 수 있는 일반적인 개발 지식보다는, 해당 프로젝트만의 특별한 요구사항과 비즈니스 규칙을 담은 도구를 만드는 것을 목표로 합니다.

예를 들어, E-commerce 프로젝트라면 일반적인 "TypeScript 코드 리뷰어"보다는 "주문 검증 로직 전문가"나 "결제 통합 스페셜리스트" 같은 프로젝트 특화 도구를 우선적으로 제안합니다.

## Triggers

이 커맨드는 다음과 같은 상황에서 사용합니다:

- 새 프로젝트에 맞춤형 개발 도구가 필요할 때
- 프로젝트 특화 에이전트와 스킬을 한 번에 생성하고 싶을 때
- 프로젝트의 비즈니스 로직이나 도메인 지식을 도구화하고 싶을 때
- 팀 협업을 위한 프로젝트 지식 베이스를 구축할 때
- 명시적 호출: `/project-tooling` 실행

## Usage

```bash
/project-tooling
```

현재 버전은 플래그나 옵션 없이 실행됩니다. 실행하면 대화형 인터페이스가 시작되어 추천 목록에서 선택할 수 있습니다.

## Behavioral Flow

### Phase 1: 프로젝트 심층 분석

프로젝트의 전체적인 구조와 특성을 파악합니다.

**Steps:**
1. **메타 파일 검색**: Glob을 사용하여 package.json, requirements.txt, go.mod, pom.xml, Gemfile 등 언어별 메타 파일 탐색
2. **메타 정보 읽기**: Read로 발견된 메타 파일들을 읽어 의존성, 스크립트, 설정 분석
3. **기술 스택 감지**: 언어, 프레임워크, 주요 라이브러리 식별
4. **프로젝트 타입 분류**: E-commerce, SaaS, CMS, API 등 비즈니스 도메인 파악
5. **고유성 평가**: 프로젝트에만 있는 특별한 패턴, 규칙, 요구사항 식별

**Conditions:**
- If 메타 파일이 여러 개 발견됨: 멀티 언어 프로젝트로 판단, 모든 기술 스택 분석
- If 메타 파일이 없음: 파일 확장자 기반으로 언어 추정
- If 비즈니스 키워드 발견 (payment, order, inventory 등): 도메인 특화 도구 우선순위 상향

**Error Handling:**
- 메타 파일 파싱 실패: 에러 무시하고 다음 파일 분석 계속
- 프로젝트 타입 불명확: AskUserQuestion으로 사용자에게 프로젝트 타입 선택 요청
  - 선택지: E-commerce, SaaS, CMS, API Service, Financial, Healthcare, Education, Media/Content, DevOps/Infrastructure, Other (직접 입력)

### Phase 2: 서브 에이전트 추천 및 생성

분석 결과를 기반으로 프로젝트 특화 서브 에이전트를 추천하고 생성합니다.

**Steps:**
1. **에이전트 후보 생성**: 프로젝트 분석 결과를 기반으로 3-5개의 에이전트 추천
2. **우선순위 결정**:
   - **최우선**: 프로젝트 고유 비즈니스 로직 관련 (예: e-commerce-order-validator)
   - **중간**: 프로젝트 도메인 특화 (예: payment-integration-specialist)
   - **최하위**: 일반적 개발 도구 (예: typescript-reviewer)
3. **사용자 선택 인터페이스**: AskUserQuestion으로 추천 목록 제시 (우선순위 순 정렬)
4. **에이전트 생성**: 선택된 에이전트에 대해 SlashCommand로 `/create-agent` 호출
5. **결과 확인**: 생성된 에이전트 파일 경로와 품질 점수 표시

**Validation:**
- 추천 목록이 3개 이상인지 확인
- 프로젝트 고유 에이전트가 최소 1개 이상 포함되었는지 검증
- 일반적 에이전트는 전체의 50% 이하로 제한

**Error Handling:**
- If `/create-agent` 실패: 에러 메시지 표시하고 다음 항목 계속 진행
- If 사용자가 선택 안 함: Phase 3로 건너뛰기

### Phase 3: 스킬 추천 및 생성

프로젝트 지식과 비즈니스 규칙을 스킬로 문서화합니다.

**Steps:**
1. **스킬 후보 생성**: 프로젝트 분석 결과를 기반으로 3-5개의 스킬 추천
2. **우선순위 결정**:
   - **최우선**: 프로젝트 비즈니스 규칙 및 정책 (예: e-commerce-business-rules)
   - **중간**: 도메인 특화 패턴 (예: payment-security-patterns)
   - **최하위**: 범용 개발 모범 사례 (예: react-best-practices)
3. **사용자 선택 인터페이스**: AskUserQuestion으로 추천 목록 제시 (우선순위 순 정렬)
4. **스킬 생성**: 선택된 스킬에 대해 SlashCommand로 `/create-skill` 호출
5. **결과 확인**: 생성된 스킬 파일 경로 표시

**Validation:**
- 추천 목록이 3개 이상인지 확인
- 프로젝트 특화 스킬이 최소 1개 이상 포함되었는지 검증
- 범용 스킬은 전체의 50% 이하로 제한

**Error Handling:**
- If `/create-skill` 실패: 에러 메시지 표시하고 다음 항목 계속 진행
- If 사용자가 선택 안 함: 워크플로우 종료

## Tool Coordination

### Primary Tools

- **Glob**: 프로젝트 메타 파일 패턴 검색
  - 용도: `package.json`, `requirements.txt`, `*.csproj` 등 언어별 메타 파일 찾기
  - 사용 시점: Phase 1 시작 시

- **Read**: 메타 파일 내용 읽기
  - 용도: 발견된 메타 파일의 내용을 파싱하여 의존성, 스크립트, 프로젝트 정보 추출
  - 사용 시점: Glob으로 파일을 찾은 후

- **AskUserQuestion**: 사용자 선택 인터페이스
  - 용도: 추천된 에이전트/스킬 목록에서 사용자가 생성할 항목 선택
  - 사용 시점: Phase 2, Phase 3에서 각각 1회

- **SlashCommand**: 하위 커맨드 호출
  - 용도: `/create-agent`와 `/create-skill` 실행
  - 사용 시점: 사용자가 항목을 선택한 후

### Secondary Tools

- **Grep**: 프로젝트 내 특정 패턴 검색 (선택적)
  - 용도: 비즈니스 키워드 (payment, order, inventory 등) 검색으로 도메인 파악
  - 사용 시점: Phase 1 프로젝트 타입 분류 시

## Key Patterns

### 우선순위 점수 시스템

프로젝트 분석 결과를 기반으로 각 에이전트/스킬 후보에 점수를 부여합니다:

```
점수 계산:
- 프로젝트 고유 비즈니스 로직: +100점
- 도메인 특화 (결제, 주문 등): +50점
- 프로젝트 기술 스택 관련: +30점
- 범용 개발 도구: +10점

예시:
"e-commerce-order-validator" (고유 로직) = 100점
"payment-integration-specialist" (도메인 특화) = 50점
"typescript-reviewer" (범용) = 10점

→ 점수순으로 정렬하여 사용자에게 제시
```

### 비개발적 내용 감지

다음과 같은 요소들을 "비개발적/프로젝트 고유" 내용으로 우선 처리합니다:

- **비즈니스 규칙**: 주문 유효성 검사 로직, 가격 계산 규칙, 할인 정책
- **도메인 지식**: 금융, 의료, 물류 등 산업별 규제 및 표준
- **회사 정책**: 데이터 보존 정책, 보안 요구사항, 접근 제어 규칙
- **워크플로우**: 승인 프로세스, 알림 규칙, 상태 전환 로직
- **통합 사양**: 외부 API 연동 방법, 레거시 시스템 통신 프로토콜

반대로 다음은 "일반적 개발 내용"으로 낮은 우선순위:
- 코드 포맷팅, 린팅 규칙
- 프레임워크 모범 사례
- 일반적인 디자인 패턴
- 범용 테스팅 전략

## Examples

### Example 1: E-commerce 프로젝트

```bash
/project-tooling
```

**프로젝트 분석 결과:**
```
📊 프로젝트 분석 완료!

🔍 감지된 환경:
- 언어: TypeScript, Python
- 프레임워크: React (frontend), FastAPI (backend)
- 주요 라이브러리: stripe, sendgrid, redis
- 프로젝트 타입: E-commerce Platform

🎯 비즈니스 키워드 발견: payment, order, cart, checkout, inventory
```

**Phase 2: 서브 에이전트 추천**
```
💡 추천 서브 에이전트 (프로젝트 고유 우선):

1. ⭐ order-validation-specialist (프로젝트 고유, 100점)
   - 역할: 주문 유효성 검사 로직 전문 (재고, 가격, 할인 규칙)
   - 타입: Analyst

2. ⭐ payment-integration-coordinator (도메인 특화, 50점)
   - 역할: Stripe 결제 연동 및 에러 처리 전문
   - 타입: Orchestrator

3. inventory-sync-manager (도메인 특화, 50점)
   - 역할: 재고 동기화 및 알림 관리
   - 타입: Specialist

4. typescript-code-reviewer (범용, 10점)
   - 역할: TypeScript 코드 품질 검증
   - 타입: Analyst

어떤 에이전트를 생성하시겠습니까? (다중 선택 가능)
```

**사용자 선택:** 1, 2 선택

**Phase 3: 스킬 추천**
```
💡 추천 스킬 (프로젝트 지식 우선):

1. ⭐ e-commerce-business-rules (프로젝트 고유, 100점)
   - 내용: 주문 규칙, 할인 정책, 반품 규정, 재고 관리 규칙

2. ⭐ payment-security-patterns (도메인 특화, 50점)
   - 내용: PCI-DSS 준수, 결제 데이터 암호화, 사기 탐지 패턴

3. react-state-management (범용, 10점)
   - 내용: React 상태 관리 모범 사례

어떤 스킬을 생성하시겠습니까? (다중 선택 가능)
```

**사용자 선택:** 1, 2 선택

**Expected Outcome:**
```
✅ 프로젝트 도구 생성 완료!

📂 생성된 서브 에이전트:
- .claude/agents/order-validation-specialist.md (품질: 95/100)
- .claude/agents/payment-integration-coordinator.md (품질: 92/100)

📂 생성된 스킬:
- e-commerce-business-rules/SKILL.md
- payment-security-patterns/SKILL.md

💡 다음 단계:
1. 에이전트는 자동으로 활성화되며 관련 작업 시 실행됩니다
2. 스킬은 @{스킬-이름}/SKILL.md 형식으로 대화에서 참조하세요
```

**Troubleshooting:**
- **문제**: "프로젝트 타입을 감지할 수 없습니다"
  - **해결**: 프로젝트 루트에 package.json 등 메타 파일이 있는지 확인
- **문제**: "추천 항목이 너무 일반적입니다"
  - **해결**: 프로젝트 코드에 비즈니스 로직이 명확히 표현되어 있는지 확인

### Example 2: 금융 API 서비스

```bash
/project-tooling
```

**프로젝트 분석 결과:**
```
📊 프로젝트 분석 완료!

🔍 감지된 환경:
- 언어: Go
- 프레임워크: Gin
- 주요 라이브러리: jwt-go, gorm, kafka-go
- 프로젝트 타입: Financial API Service

🎯 비즈니스 키워드 발견: transaction, account, balance, audit
```

**추천 에이전트:**
1. ⭐ transaction-audit-specialist (프로젝트 고유, 100점)
2. ⭐ financial-compliance-checker (도메인 특화, 50점)
3. go-api-performance-analyzer (범용, 10점)

**추천 스킬:**
1. ⭐ financial-transaction-patterns (프로젝트 고유, 100점)
2. ⭐ audit-trail-requirements (도메인 특화, 50점)
3. go-concurrency-patterns (범용, 10점)

**Expected Outcome:**
프로젝트의 금융 도메인 특성을 반영한 도구들이 생성되며, 일반적인 Go 개발 도구보다 금융 규제 및 감사 추적에 특화된 도구들이 우선 제공됩니다.

### Example 3: 멀티 언어 프로젝트

```bash
/project-tooling
```

**프로젝트 분석 결과:**
```
📊 프로젝트 분석 완료!

🔍 감지된 환경:
- 언어: TypeScript, Python, Rust
- 프레임워크: Next.js (frontend), Django (backend), Tauri (desktop)
- 프로젝트 타입: Desktop Application with Web Dashboard

🎯 멀티 언어 프로젝트 감지: 각 언어별 특화 도구 추천
```

**추천 에이전트:**
1. ⭐ cross-platform-sync-coordinator (프로젝트 고유, 100점)
2. tauri-integration-specialist (도메인 특화, 50점)
3. typescript-code-reviewer (범용, 10점)

**Expected Outcome:**
멀티 언어 프로젝트의 특성을 반영하여 플랫폼 간 동기화나 통합에 특화된 도구가 우선 제공됩니다.

## Boundaries

**Will:**
- 프로젝트 메타 파일 (package.json, requirements.txt 등)을 분석하여 기술 스택 감지
- 비즈니스 키워드 및 도메인 패턴을 분석하여 프로젝트 타입 분류
- 프로젝트 타입이 불명확할 경우 사용자에게 타입 선택 인터페이스 제공
- 프로젝트 고유 비즈니스 로직 관련 도구를 최우선으로 추천
- 비개발적/도메인 특화 내용을 일반적 개발 지식보다 높은 우선순위로 처리
- 사용자에게 명확한 우선순위 정보와 함께 선택 인터페이스 제공
- `/create-agent` 및 `/create-skill` 커맨드를 활용하여 실제 파일 생성 위임
- 생성된 도구의 품질 점수 및 파일 경로 표시

**Will Not:**
- 사용자 확인 없이 에이전트나 스킬 파일을 자동 생성하지 않음
- 일반적인 개발 도구를 프로젝트 특화 도구보다 높은 우선순위로 추천하지 않음
- 프로젝트 외부 디렉토리나 파일에 접근하지 않음
- 메타 파일 내용을 수정하거나 변경하지 않음 (읽기 전용)
- 에이전트/스킬 생성 실패 시 강제로 진행하지 않음 (에러 표시 후 계속)

**Safety Checks:**
- 프로젝트 루트 디렉토리 내에서만 파일 검색
- 메타 파일 파싱 에러 시 안전하게 무시하고 다음 파일 계속
- 프로젝트 타입이 불명확하면 자동 추정하지 않고 사용자에게 선택 요청
- 추천 목록이 3개 미만이면 경고 표시
- 프로젝트 고유 항목이 0개면 "일반적 도구만 추천됨" 경고 표시
- `/create-agent` 및 `/create-skill` 호출 전 유효성 확인
- 사용자 선택이 없으면 강제 진행하지 않고 종료
